# Scrolling with pleasure

![rw-book-cover](https://pavelfatin.com/favicon.ico)

## Metadata
- Author: [[pavelfatin.com]]
- Full Title: Scrolling with pleasure
- Category: #articles
- Publication date: 2017-03-19
- URL: https://pavelfatin.com/scrolling-with-pleasure/
- Summary: Smooth, high-precision scrolling improves user experience by making navigation more accurate and responsive. Achieving this requires pixel-level input, system-wide support, and good rendering to ensure consistency and low latency. Many applications still lack smooth scrolling because they do not fully use modern APIs or support acceleration and interpolation.

# Notes

[[pavelfatin.com - Scrolling with pleasure]]

***

![Mouse with a smile](https://pavelfatin.com/images/scrolling-with-pleasure.png)Typing with pleasure
In this article I explain what it takes to implementing high-quality smooth / high-precision scrolling on modern computers and why some systems have it while others don’t. Most of the insights came from my work on implementing [“true smooth scrolling”](https://github.com/JetBrains/intellij-community/blob/71f67e4a50127c031014054e726a32ce6f8dffba/bin/idea.properties#L115-L159) in IntelliJ IDEA.

The article complements my previous article [“Typing with pleasure”](https://pavelfatin.com/typing-with-pleasure/), just like scrolling naturally complements typing in the day-to-day activity of most computer users. Nowadays, some interfaces are based *solely* on scrolling! Because scrolling is so ubiquitous, any improvement in its functioning automatically improves the user experience and can boost our productivity big time.

The topic of scrolling seems to fascinate a surprising amount of people — computer forums are filled with questions like [“Why does Mac OS X trackpad moves so much better than Windows?”](https://www.reddit.com/r/answers/comments/1sr3fd/why_does_mac_os_xs_trackpad_move_so_much_better/) or [“Why no smooth scrolling on Linux?”](https://www.reddit.com/r/linux/comments/2qd564/why_no_smooth_scrolling_on_linux/) . Despite the pry, the answers usually come down to something like “Mac OS uses *magic*, just deal with it”, which only makes the issue even more mysterious. However, as the third [Clarke’s law](https://en.wikipedia.org/wiki/Clarke%27s_three_laws) says “Any sufficiently advanced technology is indistinguishable from magic” — so no actual magic is required — the proper technology is all that is needed.

Although the article contains a great deal of technical details, I hope that it will be interesting not only to computer programmers, but also to people who wonder how scrolling works under the hood, why we have what we have and how we can make the scrolling better.

**Contents**

* [Input](https://pavelfatin.com/scrolling-with-pleasure/#input)
	+ [Scrollbar](https://pavelfatin.com/scrolling-with-pleasure/#scrollbar)

All the code examples are available as a [GitHub repository](https://github.com/pavelfatin/scrolling-with-pleasure).

##### 1. Introduction

Since “smooth scrolling” became popular, the term has been used for many different things — particularly, notion of smoothness and notion of precision got mixed. We should clearly distinguish between different characteristics of scrolling.

###### 1.1 Smooth scrolling

Smooth scrolling is a [scrolling](https://en.wikipedia.org/wiki/Scrolling#Computing) that is visually *continuous*, i.e. not discrete.

What is smooth scrolling good for — isn’t it just “bells and whistles”? Nothing of the kind! Human [visual system](https://en.wikipedia.org/wiki/Visual_system) is adapted to deal with real-world objects, which are moving continuously. Abrupt image transitions place a burden on the visual system, and increase user’s cognitive load (because it takes additional time and effort to consciously match the before and after pictures).

A number of studies have demonstrated measurable benefits from smooth scrolling, for example, here’s a quote from [Klein, 2005](http://hcil.cs.umd.edu/trs/2004-14/2004-14.pdf) PDF:

>  This study shows that animated scrolling significantly improves both efficiency and user satisfaction. The magnitude of this improvement is greatest for repetitive documents lacking visual landmarks. 
> 
> 

To implement continuous scrolling we need continuous [animation](https://en.wikipedia.org/wiki/Animation), however we don’t necessarily need high-precision input for that — for example, it’s possible to scroll content from end to end smoothly after only a single input event (like a keystroke), so that, while positioning is extremely non-precise, the transition is perfectly smooth.

###### 1.2 High-precision scrolling

High-precision scrolling is a scrolling that allows fine-grained *positioning* with steps that are smaller than 1 line (but not necessarily as small as 1 pixel).

As a side effect, applications that don’t animate scrolling may produce smoother result when provided with high-precision events. However, high-precision events cannot guarantee smooth scrolling by themselves, because input resolution is often inadequate for visually smooth transitions.

Here’s what high-precision scrolling really offers (comparing to coarse-grained scrolling):

* Accurate positioning. Even though large scrolling steps can be animated, there’s no way to fixate the content between the steps. High-precision scrolling makes it possible to position the content exactly as the user wants.
* Better reproduction of dynamics. Because high-precision events have higher resolution, they can reproduce fine-grained dynamics of the original motion much better. This improves hand-eye coordination and makes it easier for the user to control the scrolling and to track the content.
* Lower input latency. High-precision events increase the responsiveness of scrolling, because OS can dispatch those event to application immediately, without accumulating multiple events to get sufficiently large delta.

A particular kind of high-precision scrolling, known as “pixel-perfect scrolling”, guarantees minimal step to be 1 pixel. That kind of scrolling usually requires pixel-precise positioning and OS-level input acceleration — to offer both fine-grained- and long-distance scrolling at the same time. Pixel-perfect scrolling also guarantees the lowest possible input latency.

###### 1.3 Summary

Because scrolling precision and scrolling smoothness are semi-independent, in practice, scrolling can be:

1. Non-smooth, non-precise.
2. Smooth, but not precise.
3. Precise, but not truly smooth.
4. Precise and smooth.

Apparently, we want scrolling to be both smooth and precise (ideally, pixel-precise).

To get the idea of scrolling smoothnes, scrolling precision and scrolling latency, you may check the following video (it’s better to [open](https://pavelfatin.com/videos/scrolling.mp4) it in a separate windows):

##### 2. Model

Any application that uses scrolling has an internal model that represents its scrolling state. This model typically contains the following properties:

* minimum & maximum positions,
* current position,
* line step (“unit increment”) & page step (“block increment”),
* visible range,

When application employs default OS / toolkit scrolling widgets, those values are stored in scrollbars, particularly:

If application doesn’t rely on the standard scrolling capabilities (e.g. most web browsers), it often still uses scrollbar components as its scrolling model (however, in principle, programmers may choose to implement a custom scrolling model that is independent of scrollbars).

What’s the deal with scrolling model? Scrolling model either permits or prohibits precise content positioning and smooth scrolling at the most basic level, depending on whether it uses lines as units of measurement.

One way to model scrolling state is to measure all the properties in “lines”. This method is a legacy from the era of [text mode](https://en.wikipedia.org/wiki/Text_mode) when content were represented as characters rather than individual pixels. In text mode, screen consists of uniform character cells and line-based scrolling model goes well with that. Although modern GUI applications are not bounded by character gird anymore, many programs still measure scrolling in lines, rather than in pixels. This is most typical of [text editors](https://en.wikipedia.org/wiki/Text_editor) (for example, Notepad, GVim) and [terminal emulators](https://en.wikipedia.org/wiki/Terminal_emulator), where text lines are apparent, but can also be used in other kinds of programs (e.g. [file managers](https://en.wikipedia.org/wiki/File_manager), etc).

It’s easy to determine whether particular program relies on a line-based model — simply use a mouse pointer to drag scrollbar’s thumb *continuously* and check whether the content is scrolled *discretely* (line-by-line). For example, [Ubuntu](https://en.wikipedia.org/wiki/Ubuntu_(operating_system))‘s terminal emulator uses a line-based model (and, surprisingly, the same goes for the Mac OS Terminal):

![Ubuntu terminal](https://pavelfatin.com/images/scrolling-with-pleasure/ubuntu-terminal.png)
Needless to say that when line-based scrolling model is in use, there is no way to scroll the content smoothly, *in principle* (because it’s impossible to position content with precision enough for smooth animation). No amount of “quality touchpads” can change that.

In addition to the coarse-grained scrolling, line-based model has a few more quirks:

* It produces different “visual” speeds for different line heights — the same input event results in different pixel shifts depending on the document font size — the larger the font, the faster (and the less smooth) is its scrolling animation (thus, scrolling speed also varies between different applications).
* If some of the lines have different heights (e.g. due to different fonts), scrolling becomes non-linear — even when input events are monotonous, visual shifts are not. This can also happen when document font is fixed, but a line with wraps is still considered “a single line” (this is, for example, what [GVim](https://en.wikipedia.org/wiki/Vim_(text_editor)) does, and that feels rather unnatural).

What kind of model do we need to permit smooth scrolling? — we need to store scrolling state with *pixel-level* precision. If scrolling model uses integer variables, this requires the minimum / maximum variables to match real content size, in pixels. With [floating-point](https://en.wikipedia.org/wiki/Floating_point) variables we may use a constant range from 0.0 to 1.0, however, this approach might introduce [rounding errors](https://en.wikipedia.org/wiki/Round-off_error). It seems that the ideal scrolling model should match actual content size, in pixels, but use floating point numbers for further extensibility, because what is considered “real” pixels now can become [device-independent pixels](https://en.wikipedia.org/wiki/Device-independent_pixel) later.

When scrolling model is pixel-accurate, both precise- and smooth scrolling are in principle possible. However, proper model by itself is not enough — resulting precision still depends on the precision of input events and visual smoothness depends on the rendering algorithm.

##### 3. Input

Suppose we have the right model, so what? Now we need to get the input from user and to adjust our model accordingly. The characteristics of these updates vary widely, depending on a particular device and on a specific OS API that we use to acquire the input. Let’s examine the most typical use cases.

###### 3.1 Scrollbar

Using a [mouse](https://en.wikipedia.org/wiki/Computer_mouse) (or a [touchpad](https://en.wikipedia.org/wiki/Touchpad)) to drag a “thumb” (also known as “bar” or “knob”) on a [scrollbar](https://en.wikipedia.org/wiki/Scrollbar) is the most apparent, easily discoverable way to do the scrolling.

![Scrollbar](https://pavelfatin.com/images/scrolling-with-pleasure/scrollbar.png)
This [interaction](https://en.wikipedia.org/wiki/Interaction_technique) seems inherently continuous, but can we really use thumb location to control scrolling position directly? Let’s find out.

It should be noted that scrollbar also allows scrolling by line (by clicking the arrows) and scrolling by page (by clicking the track). Those actions are fundamentally different and we will discuss that type of scrolling later, together with the mouse wheel scrolling.

Keep in mind that scrollbar is not only a means of scrolling as such, but it’s also an essential visual indicator of the scrolling position, regardless of particular scrolling method.

###### 3.1.1 Pointer precision

As we employ a [pointer](https://en.wikipedia.org/wiki/Pointer_(user_interface)) to drag the thumb, we first need to determine how smooth the pointer movement by itself is. For simplicity, I assume that we use a computer mouse to position the pointer, but all the reasoning is applicable to a touchpad as well (especially considering that, both in hardware and in software, touchpad is usually represented as a mouse).

While most of the conclusions can be drawn theoretically, some real-world numbers are always nice to have. To collect quantitative data, I used my current mouse — [Logitech M-UAE55](http://support.logitech.com/en_ch/product/mini-mouse-optical), which is a rather typical office mouse:

![Logitech M-UAE55](https://pavelfatin.com/images/scrolling-with-pleasure/logitech-m-uae55.jpg)
To intercept [USB HID](https://en.wikipedia.org/wiki/USB_human_interface_device_class) reports, I used [Arduino Leonardo](https://www.arduino.cc/en/Main/ArduinoBoardLeonardo) with [USB Host Shield](https://www.arduino.cc/en/Main/ArduinoUSBHostShield) as a “mouse proxy” ([code](https://github.com/pavelfatin/scrolling-with-pleasure/blob/master/arduino/mouse_logger.ino)):

![Arduino Leonardo with USB Host Shield](https://pavelfatin.com/images/scrolling-with-pleasure/arduino-leonardo-with-usb-host-shield.jpg)
Although this device is not a passive [bus analyzer](https://en.wikipedia.org/wiki/Bus_analyzer), because the ports support USB 2.0 and 1000 Hz polling rate, its precision is more than enough for our purposes. The advantage is that we can adjust mouse polling rate regardless of operating system.

Simultaneously, I used an application that tracks movement of the pointer ([code](https://github.com/pavelfatin/scrolling-with-pleasure/blob/master/java/MouseLogger.java)). In this way, it’s possible to analyze both the mouse reports and the corresponding pointer movements in real time.

For those who have no Arduino at hand but wants to repeat the experiments, I’ve also created a software-only mouse analyzer ([code](https://github.com/pavelfatin/scrolling-with-pleasure/blob/master/windows/MouseLogger.c)), though it works only on Windows and it cannot tweak the USB polling rate.

###### 3.1.2 Spatial resolution

For a start, let’s estimate spatial resolution of a mouse pointer (or, simply speaking, pointer’s step length).

Although most [operating systems](https://en.wikipedia.org/wiki/Operating_system) provide methods to access low-level mouse input (for example, [WM\_INPUT](https://msdn.microsoft.com/en-us/library/windows/desktop/ee418864.aspx) in Windows, [XI raw events](https://who-t.blogspot.de/2011/09/whats-new-in-xi-21-raw-events.html) in Linux, [I/O Kit](https://developer.apple.com/library/content/documentation/DeviceDrivers/Conceptual/IOKitFundamentals/Features/Features.html#//apple_ref/doc/uid/TP0000012-TPXREF101) in Mac OS), typical desktop applications never access that data directly. Instead, OS transforms and aggregates the incoming data and then exposes the result to applications as “pointer motion”. In some systems, the distinction is somehow blurred — for example, Windows has [WH\_MOUSEMOVE](https://msdn.microsoft.com/en-us/library/windows/desktop/ms645616.aspx) message that is “posted to a window when the *cursor* moves”. In Linux, the API is more explicit — [Xlib](https://en.wikipedia.org/wiki/Xlib) refers specifically to *pointer* motion: `XPointerMotionMask`, `PointerMovedEvent`, etc.

Because, at the level of operating system, pointer coordinates are usually measured in [pixels](https://en.wikipedia.org/wiki/Pixel), “1 pixel” is the best precision / granularity we can attain. On the one hand, this statement is obvious, yet, on the other hand, it’s rather peculiar, because precision of the input device (mouse) is limited by precision of the output device (display). Thus, high-[resolution](https://en.wikipedia.org/wiki/Display_resolution) display offers not only a more crisper graphics, but also a potentially more fine-grained pointer control (OS X, however, [aligns](https://apple.stackexchange.com/questions/64032/is-it-possible-to-move-the-mouse-by-one-pixel-on-a-retina-display) pointer to [device independent pixels](https://en.wikipedia.org/wiki/Device-independent_pixel), not to physical pixels, so [Retina Display](https://en.wikipedia.org/wiki/Retina_Display) doesn’t improve the pointer accuracy).

Do we need single-pixel precision? As [angular resolution](https://en.wikipedia.org/wiki/Angular_resolution) of human eye is about [0.02°](https://en.wikipedia.org/wiki/Naked_eye#Basic_accuracies), people can differentiate pixels with [density](https://en.wikipedia.org/wiki/Pixel_density) up to 300 PPI (from a typical screen viewing distance). Commonly used monitors have pixel density way below this number (for example, my [Iiyama XB22783HSU](https://us.hardware.info/product/198860/iiyama-prolite-xb2783hsu-b1/specifications) has only 82 PPI). Thus, single-pixel precision is desirable (and, actually, insufficient, as we’ll see later).

So, can we use a mouse to move the pointer pixel-by-pixel?

###### 3.1.3 Sensor resolution

Although the pointer is aligned to the pixel grid, mouse, by itself, doesn’t deal with the notion of pixels — it reports relative movement in abstract “counts” (or “steps”) that are tied neither to pixels, nor to a predetermined physical distance.

The more we move the mouse, the more counts it reports. The exact number of counts depends on mouse’s sensor resolution, which is measured in terms of counts per inch (CPI) — the number of counts the mouse will report when it moves one inch (this number is often incorrectly listed as “dots per inch” or DPI in mouse specification).

By comparing the travel distance with the number of reported counts we can determine resolution of particular sensor experimentally. For example, my mouse reports 7880 counts after traveling 400 mm, which precisely corresponds to 500 CPI.

How to map mouse counts to screen pixels? The simplest way is to shift the pointer by one pixel per reported count. That happens on “medium” pointer speed in most OSes (unless it applies “acceleration”):

![Mouse pointer speed](https://pavelfatin.com/images/scrolling-with-pleasure/mouse-pointer-speed.png)
However, depending on the exact sensor resolution, screen resolution and display size, the direct 1-to-1 mapping might result it either excessively slow or excessively fast pointer response that can make the control uncomfortable. To counter that, operating systems offer to adjust pointer speed, making the cursor move faster or slower. Technically, this adjustment is implemented as a multiplier coefficient that determines average pointer step per mouse count, so the step can be arbitrarily bigger than 1 pixel.

For example, to control my mouse comfortably, I need to shift the pointer speed slider one notch to the right, so that my mouse travels about 65 mm while the pointer crosses my [Full HD](https://en.wikipedia.org/wiki/1080p) screen horizontally (1920 pixels). This corresponds to 1.5 pixel step per mouse count (and on a [4K](https://en.wikipedia.org/wiki/4K_resolution) screen that would be twice as much). By tracking the pointer movement, we can confirm that the average step is indeed 1.5 pixels (which is larger than 1).

As there’s no way to position the pointer within a fraction of a pixel, operating system has to resort to non-uniform pointer steps in spite of uniform input from the mouse (because of [quantization](https://en.wikipedia.org/wiki/Quantization_(signal_processing)). Thus, insufficient sensor resolution results not only in nonoptimal step length, but also in nonuniform pointer motion.

For instance, when my mouse generates `(1, 1, 1, 1, 1, ...)` movement reports, the pointer is shifted by `(1, 2, 1, 2, 1, ...)` pixels respectively, so that the average step approaches 1.5 pixels, yet the movement is not uniform . While 1-pixel oscillation might not seem like a big deal, we need to take subsequent “magnification” by the scrollbar into account (as we will see later).

Although the coarse-grained / uneven pointer motion stems from inadequate sensor resolution, operating system may try to mend the problem with software means. For example, Windows provides an option called “Enhance pointer precision”, which reduces mouse sensitivity to 1.0 when mouse is moved slowly:

![Enhance pointer precision](https://pavelfatin.com/images/scrolling-with-pleasure/enhance-pointer-precision.png)
In a sense, this mode is “on-demand deceleration”, and while it helps to increase the precision (but only down to 1-pixel level), it gives rise to nonlinear pointer response that complicates hand-mouse-eye coordination ([more info](http://us.battle.net/forums/en/sc2/topic/12675389459)). The effectiveness of such a mode is limited to relatively slow movements (because of how USB polling works, as we will see next). Besides, there’s no guarantee that pointer acceleration is supported / enabled in a particular OS.

Only a mouse with sufficient sensor resolution can reliably provide stable 1-pixel pointer step (but we cannot guarantee that user has such a mouse). If you *do* have a mouse with adjustable sensitivity, you should never use sensitivity as a means to control pointer speed (despite “common knowledge”). Instead, it’s reasonable to select the highest CPI (DPI) possible and then rely on OS’ mouse settings to set desirable speed, while keeping the precision. If you have multiple mouses or touchpads with different resolutions, you can use something like [EitherMouse](http://www.eithermouse.com/) to control the speed / acceleration on per-device basis.

###### 3.1.4 Polling interval

When mouse registers a “count” it doesn’t report it via [USB](https://en.wikipedia.org/wiki/USB) immediately. Instead, it waits for a USB host to request the transmission. These request are performed with a fixed interval, known as “polling interval”. Though USB devices request desired polling interval on initialization (down to 1 ms), operating systems usually force 10 ms polling interval for low-speed / full-speed (USB 1.x) devices, which is subsequently rounded to 8 ms (nearest power of 2) by USB controller. As a result, a typical mouse can send no more than 125 reports per second (125 Hz polling rate). In Windows, you can use [Mouse Rate Checker](http://www.softpedia.com/get/System/System-Miscellaneous/Mouse-Rate-Checker.shtml) to determine the polling frequency experimentally (there’s also a web-based [alternative](http://zowie.benq.com/en/support/mouse-rate-checker.html)):

![Mouse Rate Checker](https://pavelfatin.com/images/scrolling-with-pleasure/mouse-rate-checker.png)
As soon as frequency of generated reports exceeds the polling rate, there’s no way to receive those reports one by one. It’s easy to calculate that, for my 500 CPI mouse, this limit is exceeded when mouse speed is above ~6 mm / s — a surprisingly low threshold. Because report generation and report transmission is not synchronized, the effect can occasionally appear on much lower mouse speeds, when the transmission channel is not fully saturated.

What happens when mouse accumulates multiple reports before the transmission? Those reports are merged in a single [HID report](http://eleccelerator.com/tutorial-about-usb-hid-report-descriptors/) inside the mouse, so there’s no way to determine whether there were multiple small steps or there was a single large one.

Insufficient polling rate can increase pointer step even more than insufficient sensor resolution. For example, on moderate-speed movements, my mouse emits 5-7-count reports at 125 Hz, while reporting each count separately at 1000 Hz. That is a 5-7x step increase, yet there’s only 1.5x step increase owing to the low-resolution sensor (we need to multiply the two values to get the net result).

Because report aggregation happens sporadically, it also disrupts step uniformity (much more than the quantization error).

Thus, high sensor resolution per se is not enough — mouse must enforce an appropriate polling rate to preserve 1-pixel pointer step (and we cannot expect this from a usual mouse). If you have a mouse a with high-resolution sensor, make sure that its polling rate is set the maximum possible value (usually, 1000 Hz), otherwise the effective resolution might be hindered (moreover, even a typical mouse can benefit from the increased polling rate — my mouse can easily generate up to 1000 reports per second, given the opportunity).

###### 3.1.5 Temporal resolution

While physical mouse movement is continuous, pointer position is updated discretely. Temporal resolution refers to how frequently we can update the position of the mouse pointer. We need both spatial- and temporal resolution to be sufficient to consider the input “smooth”.

What temporal resolution is enough? We should aim for at least the typical display refresh rate (60 Hz). However, as the input events and the output frames are not synchronized, it’s better to aim for at least twice as much. Another consideration is monitors that support higher refresh rates (up to 240 Hz so far). Can we really achieve that? Yes and no.

The frequency of the updates is influenced by multiple factors:

* physical mouse speed — the faster we move the mouse, the more frequently it generates reports,
* sensor resolution — the higher the resolution, the more updates is generated for the same physical movement,
* polling rate — apparently, the frequency of updates cannot exceed the mouse polling rate,
* pixel density — the smaller the pixels, the more fine-grained updates can be reported by the operating system.

Depending on the particular combination of those factors, temporal resolution might be either extremely high, or extremely low.

First, a low-resolution mouse generates only about 1-3 counts per second during really slow movements. But even if we use a mouse with high-resolution sensor and 1000 Hz polling rate, pixel density will limit the rate of pointer position updates to almost the same number — there is simply not enough pixels along the pointer path to generate more frequent updates when pointer speed is slow. So, display density affects not only mouse’s spatial resolution, but temporal resolution as well.

Alternatively, when we move the mouse fast, we can easily generate too many updates, especially when the polling rate is high and the pixel size is small (up to 1000 updates per second). In such a case, we can easily overwhelm our rendering algorithm if we choose to update image on every input event.

The only way to substantially improve the temporal resolution is to have both a high-res monitor and a high-precision mouse, but even then the update frequency will be variable and often non-optimal.

Those who wants to dig deeper may also read about [polling precision](http://www.overclock.net/t/1550666/usb-polling-precision) and [pointer microstutters](http://www.blurbusters.com/mouse-125hz-vs-500hz-vs-1000hz/).

###### 3.1.6 Software processing

In addition to the hardware report aggregation, operating systems and [widget toolkits](https://en.wikipedia.org/wiki/Widget_toolkit) might do event aggregation of their own.

For example, Xlib Programming Manual explicitly [states](https://tronche.com/gui/x/xlib/events/keyboard-pointer/keyboard-pointer.html) that

>  The granularity of `MotionNotify` events is not guaranteed, but a client that selects this event type is guaranteed to receive at least one event when the pointer moves and then rests. 
> 
> 

— that sounds hardly reassuring.

Likewise, Java, for example, has [Component.coalesceEvents](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#coalesceEvents-java.awt.AWTEvent-java.awt.AWTEvent-) method for aggregating input events. The description states that

>  For mouse move events the last event is always returned, causing intermediate moves to be discarded. 
> 
> 

However, this functionality seems to be disabled by default, which is a good thing.

Unlike Java, GTK 3 enables “event compression” by default, which not only merges several motion events together, but also delays the delivery of events (up to the next frame rendering) and skews the timing. This might [pose a problem](https://bugzilla.gnome.org/show_bug.cgi?id=702392) for applications that require more fine-grained input.

###### 3.1.7 Mapping precision

To perform scrolling, we use the pointer to drag a “thumb” on a scrollbar. The thumb itself is positioned on the screen with 1-pixel precision, and that position determines the resulting scrolling position.

Because scrolling, by definition, implies that content size can be greater than view size, content length can be arbitrary greater than length of the corresponding scrollbar’s track. This mismatch results in suboptimal positioning precision. For example, if content height is, say, 10000 pixels and view height is 1000 pixel, a single track’s pixel maps to 10 content pixels and that can be a text line height (strictly speaking, we need to account for the thumb length and the length of possible arrow buttons, but the simplified calculation captures the essence well enough):

![Scrollbar mapping](https://pavelfatin.com/images/scrolling-with-pleasure/scrollbar-mapping.png)
Thus, 1-pixel pointer steps can result in 10-pixel content steps, and 1-2-pixel inaccuracies can turn into 1-2-line inaccuracies (which is more than noticeable with the naked eye). The bigger the content and the smaller the view, the lower the positioning precision. In extreme cases, when 1-pixel “thumb step” maps to a “content step” that is greater than the corresponding view size, supposedly continuous scrolling will even skip portions of the content. Apparently, 1-pixel pointer precision is not enough to control the scrolling position directly.

To improve the positioning precision, we need [subpixel pointer precision](http://johan.kiviniemi.name/blag/making-x-report-the-mouse-position-with-subpixel-precision/) (you may see a good [video](https://www.youtube.com/watch?v=Cwxyksa7iho) on that topic), however, such a solution is yet to be implemented in reality. The closest thing today is probably [libinput](https://wayland.freedesktop.org/libinput/doc/latest/pointer-acceleration.html) (supports subpixel movements) + [GTK](https://www.gtk.org/) (uses floating-point numbers for almost everything) combination, but it’s still not there, because some internal data is still represented as integer numbers and subpixel accuracy is lost along the way ([XInput 2](https://www.x.org/releases/X11R7.7/doc/inputproto/XI2proto.txt) also reports subpixel coordinates, but only for touchpads; [Qt](https://www.qt.io/) is also [moving](http://lists.qt-project.org/pipermail/interest/2015-September/018895.html) in the right direction). Keep in mind though, that subpixel positioning, by itself, requires actual high-resolution input devices to be effective.

Ideally, we should not rely on physical resolution of a particular input device — operating system should decelerate pointer to the required degree of subpixel precision (which depends on the view size / content size ratio) *dynamically*, on demand — when user drags the thumb and the pointer is moved slowly. This could be a real solution to the inherent scrollbar imperfection, yet that solution requires a new kind of API which is still in the lap of the future. Currently we have what we have and we must deal with it.

Although, at the application level, we cannot increase the positioning precision, we can artificially level out the position transitions, so that the scrolling will be much more smooth, albeit not more precise.

###### 3.1.8 Summary

Let’s summarize the key findings:

* pointer step is often suboptimal,
* pointer movement might be nonuniform,
* pointer update frequency is not guaranteed,
* event rate might interfere with the frame rate,
* 1-pixel pointer precision is still not enough.

So, it’s hardly a good idea to rely on pointer position directly to control scrolling. Sadly, this is what most applications and widget toolkits still do, even those that implement other kinds of “smooth scrolling”. For example, that’s why [Safari](https://en.wikipedia.org/wiki/Safari_(web_browser))‘s, *touchpad* scrolling is much more smooth than *scrollbar* scrolling (and that is, actually, unjustifiable). Using a high-resolution mouse can significantly improve the smoothness (e.g. when I [use](https://twitter.com/pavelfatin/status/888003619869609984) [Zowie FK1](http://zowie.benq.com/en/product/mouse/fk/fk1.html) at 3200 CPI / 1000 Hz the difference is clearly noticeable). However, some limitations are inherent and cannot be solved by a better mouse.

Instead, we need to filter / [interpolate](https://en.wikipedia.org/wiki/Interpolation) the input data and render the image asynchronously if we want scrolling to be truly smooth. As pointer steps are “magnified” by the scrollbar, we need to apply the transformations at the level of *scrolling* position, not at the level of *pointer* position.

On relatively fast movement, when temporal resolution is sufficient, dynamics of mouse motion (produced by inertia of hand / mouse) is mirrored by the pointer movement naturally, while on slow movement we may choose to go beyond linear interpolation and to emulate dynamics via easing curves.

Because filtering and interpolation introduce an additional delay in the input processing, we need to make a reasonable tradeoff between the smoothness and the input lag, as we also want to keep the scrolling latency to a minimum.

An interesting side note: 3D games that popularized [free look](https://en.wikipedia.org/wiki/Free_look) (“mouselook”) faced the same problem of insufficient mouse resolution / interference and had to add mouse interpolation — in case of [Quake](https://en.wikipedia.org/wiki/Quake_(video_game)) series, we can [see this](https://raw.githubusercontent.com/ESWAT/john-carmack-plan-archive/master/by_day/johnc_plan_19980608.txt) at first hand in [John Carmack](https://en.wikipedia.org/wiki/John_Carmack)‘s “.plan files”.

###### 3.2 Mouse wheel

[Mouse wheel](https://en.wikipedia.org/wiki/Scroll_wheel) (or “scroll wheel”) is a ubiquitous means of scrolling on desktop computers. A definite advantage of wheel scrolling is that it allows the mouse pointer to remain close to the content rather than moving to a scrollbar. Apparently, mouse wheel can rotate only vertically, so many applications rely on `Shift` key as a modifier to scroll horizontally.

![Mouse wheel](https://pavelfatin.com/images/scrolling-with-pleasure/mouse-wheel.jpg)
Typical mouse wheel input is highly discrete, because at the level of hardware, mouse uses optical or mechanical switches to detect rotations that correspond to definite tactile wheel “steps” (many mouses that have a “free spinning” wheel is no better — despite the absence of notches, registered events are still coarse-grained). Thus, no matter how slowly you rotate the wheel, mouse reports only a *single* discrete event for each wheel step. Although there exist mouses with “high-resolution scroll wheel” that can detect smaller angular steps, those mouses are still quite rare.

How much to scroll on each wheel step? Discrete wheel steps naturally correspond to discrete text lines — when scrolling model is line-based, mouse wheel steps over a number of lines, when pixel-precise model is in use, mouse wheel increment / decrement equals to exact pixel height of those lines.

Because direct “1 step = 1 line” correspondence results in a rather slow scrolling, operating systems employ different strategies to increase wheel scrolling speed. In Windows and Linux, mouse settings allows us to select a fixed number of lines (or even “one *screen*“) that should be scrolled on each wheel step:

![Mouse Wheel Properties](https://pavelfatin.com/images/scrolling-with-pleasure/mouse-wheel-properties.png)
The default value is `3`, so mouse wheel scrolling is even more coarse-grained. Note, that this setting affects both scrolling speed and scrolling precision simultaneously and we can trade one for another. I have often heard a recommendation to set the number of scrolled lines to `1` to increase the scrolling precision (and thus, smoothnes in legacy applications), however, this tip is hardly practical because it makes scrolling unbearably slow.

Mac OS approaches wheel scrolling differently — it applies “scrolling acceleration”, so that, initially, mouse wheel scrolls content by a single line (or a line part), but when the frequency of steps increases (i.e. when wheel is rotated faster), wheel step increases dynamically and can be arbitrarily large. In a sense, this is a better solution to the tradeoff between scrolling precision and scrolling speed, however it poses a different challenge, as with the non-linear response it’s harder pre-estimate scrolling distance.

Because wheel step is tied to particular lines, mouse wheel scrolling exhibits all the quirks of line-based scrolling model even when the model is pixel-precise, namely:

* visual scrolling speed depends on document line height,
* visual speed varies between different applications,
* the speed is non-linear when line height is variable.

Moreover, with pixel-precise scrolling model, wheel scrolling has one more quirk — when current scrolling position is “inside a line” (for example, as a result of scrollbar usage), a subsequent wheel step might shift the content only to the next line boundary, which can be as close as 1 pixel, and such a step feels as “partially skipped”.

As the discreteness of typical scroll wheel maps well to the discreteness of lines, all the quirks are somehow “natural”. However, for a mouse with a “free-floating” scrolling wheel (or, especially, with a high-resolution scrolling wheel), all those peculiarities are indeed “quirks”. For step-free input, visual speed that is uniform and consistent across different applications is a much more reasonable solution. However, such a control requires a different way to represent scrolling distance and not all OSes support that yet (as we’ll see later when discussing touchpads).

Both spatial- and temporal resolutions of typical mouse wheel input is insufficient for smooth scrolling if we use that input to set scrolling position directly. However, that’s exactly what most applications still do, and that’s why scrolling in those application is not smooth at all. To make wheel scrolling smooth we need to explicitly *animate* the steps. Because it’s relatively easy to add such an animation separately, there exists many browser- and IDE plugins that try to mend the neglect and promise “smooth scrolling”, but their effect is usually limited only to the mouse wheel scrolling, and, in isolation from corresponding rendering improvements, their effectiveness is often subpar comparing to possible built-in implementations. Fortunately, quite a few major applications (e.g. web browsers) recently implemented the wheel scrolling animation natively.

From the viewpoint of user, step animation is supposed to mirror dynamics of the physical wheel rotation, however, all we have is a single discrete event that lacks any information about the original motion. The input event can be generated at different phases of the physical movement, depending on the particular mouse model — some mouses generate an event at the beginning of wheel rotation (before the “click”), some — in the middle (together with the “click”), and others — in the end of the rotation (after the “click”). When the event is postponed, it feels like an “input lag” by the user, a there’s not much we can do about it.

An important consideration is a duration of the first animation step. We should aim for duration of the original physical movement, however, this duration varies depending on the actual wheel rotation speed, which, for the first step, is unknown. It seems, that a period about 140 ms feels most natural. If we increase this duration, the scrolling might feel “laggy”, especially considering that, at the moment of animation start, some part of the physical rotation is already performed. Likewise, we shouldn’t make animation of the initial step too short, because if we do, it will be stopped before a potential subsequent wheel event can be received, and that disrupts scrolling continuity.

Duration of subsequent wheel steps can be predicted with better accuracy because we can rely on time elapsed between the adjacent events to approximate wheel rotation speed. Unlike a single wheel step, a series of wheel steps reproduce general dynamics of the motion good enough. In that case, animation can be considered interpolation.

Physical motion of the mouse wheel is not uniform, it has a period of acceleration in the beginning and a period of deceleration in the end (which, again, depend on particular mouse construction). We may [simulate that](https://material.io/guidelines/motion/duration-easing.html#duration-easing-natural-easing-curves) by applying “ease-in” and “ease-out” [curves](https://msdn.microsoft.com/en-us/library/ee308751.aspx) (also see an [interactive tutorial](http://gizma.com/easing/)). Some toolkits provide helpers for doing that, e.g. Qt’s [QEasingCurve](https://doc.qt.io/qt-5/qeasingcurve.html#details) or Java FX’s [Interpolator](https://docs.oracle.com/javafx/2/api/javafx/animation/Interpolator.html)).

Here’s an example of the cubic ease in / out curve:

![Ease in out curve](https://pavelfatin.com/images/scrolling-with-pleasure/ease-in-out-curve.png)
When animation accurately reflects the dynamics of the physical motion, resulting scrolling feels very natural and user might even think that the system indeed tracks fine-grained wheel movements. While, on the whole, this is a desirable outcome, some users might be baffled by the lack of positioning precision (e.g. 3 lines) alongside with the seemingly precise control.

High-resolution mouse wheel is a special case, which requires a different API. We’ll discuss that later, together with touchpads. Additionally, because Mac OS applies “acceleration” to mouse wheel input and can report “partial rotation” (starting from 1 / 10) on a complete wheel step, this should also be regarded as a particular case of high-resolution wheel (though, I must say, such an approach works decently only with Apple mouses, for example, with [MightyMouse](https://en.wikipedia.org/wiki/Apple_Mighty_Mouse) — because it has a [trackball](https://en.wikipedia.org/wiki/Trackball) instead of a mouse wheel; [MagicMouse](https://en.wikipedia.org/wiki/Magic_Mouse) goes even further — it uses a touchpad).

###### 3.3 Navigation

There are other triggers of scrolling that are very similar to the mouse wheel in their “discreteness”:

* [Arrow keys](https://en.wikipedia.org/wiki/Arrow_keys),
* Auto-scrolling, for example, in a [terminal emulator](https://en.wikipedia.org/wiki/Terminal_emulator) (note that it’s not even a user input).

The same recommendations are applicable to these cases (animate the transitions, apply ease-in / ease-out curves, etc). It’s reasonable to tune the animation duration depending on a particular scrolling distance.

Interestingly, there [exists](http://www.dwuser.com/education/content/quick-guide-adding-smooth-scrolling-to-your-webpages/) many JavaScript libraries that add smooth transition to in-page navigation, instead of instant jumps, which obscure the relationship between different parts of the content (however, major web browsers nowadays support that feature out-of-the-box).

###### 3.4 Touchpad

Unlike the scroll wheel, touchpad (or “trackpad”, as Apple calls it) has an inherent ability to generate precise input to control the scrolling (by using “edge scrolling”, “two-finger scrolling” or “circular scrolling”). The only reason why it has not been the case for so long is because, since the inception, touchpad has been pretending to be a mouse in all respects.

![Touchpad](https://pavelfatin.com/images/scrolling-with-pleasure/touchpad.jpg)
Early touchpads were functionally equivalent to a computer mouse — a sensor to control the pointer and a few buttons — no [multi-touch](https://en.wikipedia.org/wiki/Multi-touch), no [force touch](https://en.wikipedia.org/wiki/Force_Touch), no gestures. A temptation to use existing mouse protocols and APIs was too great, so we end up with touchpads that have a virtual scroll wheel (with “precision” of the mouse scroll wheel — 3 lines, by default). In addition to the loss of precision, such an approach conceals the fine-grained dynamics and increases the latency — if you move fingers slightly, yet not enough to trigger the many-lines scrolling, no visible feedback is produced.

After it became apparent that touchpad is quite a different beast, it was too late — the existing scheme worked decently, while introducing a new kind of input required upgrades at all levels, namely:

1. hardware protocols,
2. device drivers,
3. OS APIs,
4. widget toolkits,
5. applications.

If any element of that chain is missing, the end result is as good as none. So there were not enough incentives to revise any of the elements separately. Only Apple, being a company that controls all parts of the integration, managed to overcome inertia and produce a decent solution early on.

Now we’re finally approaching a point where PC touchpads can (sometimes) be used for high-precision scrolling, but we’re not fully there yet. Let’s put all the pieces of this puzzle together.

###### 3.4.1 Hardware protocol

Although [PS/2 port](https://en.wikipedia.org/wiki/PS/2_port) is now considered a legacy, most laptop touchpads are connected via PS/2 under the hood. Some touchpads (notably, Apple’s ones) use [USB](https://en.wikipedia.org/wiki/USB) for connection. There also exists [I2C](https://en.wikipedia.org/wiki/I%C2%B2C), [SPI](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface_Bus) and [SMBus](https://en.wikipedia.org/wiki/System_Management_Bus) touchpads, but those are in minority (though SPI is used in 2015-2016 MacBook Pro models).

After initialization, touchpads rely on [PS/2](http://www.computer-engineering.org/ps2mouse/) or [USB HID](https://en.wikipedia.org/wiki/USB_human_interface_device_class) protocols to function as a basic computer mouse. This is convenient, because in the mouse mode (known as “relative mode”) touchpad doesn’t require a special driver and can work anywhere (for example, in [UEFI](https://en.wikipedia.org/wiki/Unified_Extensible_Firmware_Interface)). However, in this mode touchpad usually doesn’t generate scrolling events at all — try to use MacBook’s touchpad in Windows without [Bootcamp](https://en.wikipedia.org/wiki/Boot_Camp_(software)) to get the idea.

Receiving low-level data about finger positions, contact areas, pressure, etc. requires sending a special sequence after which touchpad switches to extended version of communication protocol. Because there was no such thing as “standard touchpad protocol”, each vendor introduced its own proprietary format (for example, see [Synaptics Touchpad Interfacing Guide](https://www.aquaphoenix.com/hardware/ledlamp/reference/synaptics_touchpad_interfacing_guide.pdf) PDF). All proprietary protocols require special drivers to communicate with touchpad and interpret the data (so it’s driver that actually handles the scrolling).

In Windows 8.1 Microsoft introduced HID-based [Precision Touchpad](https://www.reddit.com/r/windows/comments/1hky29/precision_touchpads_the_future_of_touchpads_on/) which aims to be a standard touchpad protocol that doesn’t require a special driver to access the low-level data. However, such touchpads are still [uncommon](https://www.reddit.com/r/windows/comments/5bd7yg/list_of_laptops_with_precision_touchpads/?st=iybohqml&sh=be804ec6).

From hardware standpoint, almost any touchpad is capable of producing (relatively) fine-grained input to control the scrolling. Unless touchpad emulates scrolling events in firmware (which some touchpads might rarely do), scrolling precision is largely dependent on the touchpad driver.

###### 3.4.2 Windows

Most [OEM](https://en.wikipedia.org/wiki/Original_equipment_manufacturer#Computer_software) Windows installations bundle touchpad drivers, so one rarely needs to download and install them manually. Additionally, Microsoft requires that touchpad drivers should be available via [Windows Update](https://en.wikipedia.org/wiki/Windows_Update). Although it’s easy to obtain a driver, there’s no way to tweak or modify it, as Windows touchpad drivers are usually [proprietary software](https://simple.wikipedia.org/wiki/Closed_source) that implement vendor-specific protocols (e.g. if Apple’s BootCamp driver doesn’t support high-precision scrolling, we cannot do much about it).

Suppose touchpad driver receives the low-level data and recognizes a scrolling gesture, what should it do? In Windows Touchpad Gesture Implementation Guide Microsoft [states](https://msdn.microsoft.com/en-us/library/windows/hardware/dn614021.aspx#pan) that it should “issue horizontal or vertical wheel inputs, depending on the direction of travel of the fingers”. So, the driver is supposed to emulate a mouse, this time at the software level (because it’s the only API that is supported by most applications).

The emulation is performed by [input injection](https://msdn.microsoft.com/en-us/library/windows/hardware/dn614021.aspx#input_injection), where the driver uses [SendInput](https://msdn.microsoft.com/en-us/library/windows/desktop/ms646310.aspx) function to insert [mouse events](https://msdn.microsoft.com/en-us/library/windows/desktop/ff468877.aspx) to the OS event queue. Particularly, the scroll wheel is emulated by sending [WM\_MOUSEWHEEL](https://msdn.microsoft.com/en-us/library/windows/desktop/ms645617.aspx) messages ([more info](https://msdn.microsoft.com/en-us/library/windows/desktop/ms645601.aspx#_win32_The_Mouse_Wheel)). For horizontal scrolling, driver either emulates a simultaneous `Shift` keypress, or sends [WM\_HMOUSEWHEEL](https://msdn.microsoft.com/en-us/library/windows/desktop/ms645614.aspx) (which is less supported by applications).

At this point it might seem that in this model we’re bound to the ratcheting 3-line positioning, but, fortunately, that is not the case. Starting with Windows Vista, Microsoft introduced an [enhanced wheel support in windows](https://msdn.microsoft.com/en-us/library/windows/hardware/Dn613912.aspx) which [offers](https://msdn.microsoft.com/en-us/library/ms997498.aspx#mshrdwre_topic1) high-precision [WM\_MOUSEWHEEL](https://msdn.microsoft.com/en-us/library/ms645614.aspx) messages. While I’m yet to see a mouse with such a wheel, as an API, this is a handy abstraction.

In theory, touchpad driver can emit the high-resolution scrolling events to produce much more precise input and Microsoft [recommends](https://msdn.microsoft.com/en-us/library/windows/hardware/dn613935.aspx) to do exactly that (by the way, notice, the self-contradictory requirement for the temporal resolution). However, in practice, few drivers choose to proceed along this path. Why so? Because many Windows applications were written with the legacy mouse wheel API in mind and thus [cannot correctly handle](https://www.codeproject.com/Articles/155717/Handling-Enhanced-Mouse-Wheels-in-your-Application) high-resolution messages — the end result might vary from no scrolling to extremely fast scrolling (by the way, you can use the provided [HiResScrollWheelEmulator](https://www.codeproject.com/KB/cpp/HiResScrollSupp/Emulator.zip) to test the processing of high-resolution messages). Another potential problem is performance — when the resolution of input events is increased by an order of magnitude, applications that render the events synchronously might be overwhelmed and the scrolling might lag. Most manufacturers chose a bird in the hand over two in the bush and emulate the “good old” low-precision scrolling events. While this way of thinking proved its value in the past, nowadays it’s unnecessarily restrictive.

To add compatibility with the high-precision wheel events Windows application needs to:

1. take delta value received with the `WM_MOUSEWHEEL` message into account,
2. accumulate the delta until the minimum amount needed to scroll is reached (at least, 1 pixel),
3. reset the accumulated delta when the direction of scrolling is reversed.

To get an idea of what it takes to support high-resolution wheel scrolling events you may take a look at Firefox as an [example](https://bugzilla.mozilla.org/show_bug.cgi?id=605648). As an alternative way to “fix” the compatibility issues, since Windows 8.1 Microsoft provides a [method](https://msdn.microsoft.com/en-us/library/windows/desktop/dn457656.aspx) to opt-out from the high-precision scrolling messages via application manifest.

The distance the wheel is rotated is expressed in multiples or fractions of `WHEEL_DELTA`, [which is chosen to be 120](https://blogs.msdn.microsoft.com/oldnewthing/20130123-00/?p=5473/) (interestingly, 120 is a [smooth number](https://en.wikipedia.org/wiki/Smooth_number)). Because scrolling distance is still measured in wheel rotations, albeit “high-resolution” ones, such a scrolling exhibits all the quirks of line-based scrolling, i.e.:

* visual scrolling speed depends on document line height,
* visual speed varies between different applications,
* the speed is non-linear when line height is variable.

Unlike the mouse wheel, touchpad lacks any perceptible notches, so the underlying rotation / line mapping is completely unobvious to the user (indeed, what is a “rotation” in reference to touchpad, let alone “partial rotation”?). From touchpad-like devices people naturally expect uniform and consistent *visual* scrolling speed that doesn’t depend on the content data. Besides, the relative precision of 1/120 “rotation” is not too great — if line height is more than 40 pixels, the minimal scrolling step becomes larger than 1 pixel. And, on the contrary, if line height is low, initial scrolling events will be skipped, which results in excessive scrolling latency. So, line height also affects the precision and the latency of scrolling and prevents pixel-precise positioning. All in all, “wheel rotation” is imperfect and [leaky](https://en.wikipedia.org/wiki/Leaky_abstraction) abstraction for the touchpad scrolling events.

Because of the limitations of “high-resolution wheel events”, Precision Touchpad integrates with [Direct Manipulation API](https://msdn.microsoft.com/en-us/library/windows/desktop/hh446969.aspx) which [supports](https://msdn.microsoft.com/en-us/library/windows/desktop/hh768902.aspx) “absolute” transformations that are independent of line / row size, as well as asynchronous rendering (that API was originally introduced in Windows 8 for touch-based scrolling). However, any application that seeks to be [cross-platform](https://en.wikipedia.org/wiki/Cross-platform) or needs to function in Windows 7 can hardly depend on such an all-encompassing API. Fortunately, it’s possible to [register](https://codereview.chromium.org/1283913002/#ps20001) a window as a Direct Manipulation consumer and receive the pixel-precise input events without involving the rendering parts of the API ([more info](https://codereview.chromium.org/1283913002/patch/20001/30006)).

The unified, OS-level Precision Touchpad driver [tries hard](https://www.reddit.com/r/windows/comments/1hky29/precision_touchpads_the_future_of_touchpads_on/caw3obp/) to handle the scrolling events gracefully:

1. first, the driver tries to use Direct Manipulation API for “pixel-perfect” scrolling,
2. then, if application doesn’t support the API (very few do), the driver tries to find and manipulate application scrollbar(s) directly (by sending [WM\_HSCROLL](https://msdn.microsoft.com/en-us/library/windows/desktop/bb787575.aspx) and [WM\_VSCROLL](https://msdn.microsoft.com/en-us/library/windows/desktop/bb787577.aspx) messages, like with legacy [touchscreen panning](https://msdn.microsoft.com/en-us/library/windows/desktop/dd562171.aspx)), because, unlike the mouse wheel events, scrollbars can support pixel-precise deltas (this step might not work for widget toolkits that don’t use native scrollbars),
3. finally, if there are neither the Direct Manipulation API, nor a suitable scrollbar available, the driver resorts to sending the high-resolution `WM_MOUSEWHEEL` messages.

Keep in mind though, that all the goodies of unification require a compatible touchpad in the first place, as the universal driver works only for touchpads that support “Precision Touchpad” protocol. Moreover, because of the lacking application-level support, this presumably unified scrolling can still be very [inconsistent](https://answers.microsoft.com/en-us/windows/forum/windows_10-hardware/why-is-scrolling-speedamount-inconsistent-across/d84ebe44-b4c9-40e5-9bd0-f4491f4a3527) in practice.

So, in principle, Windows has everything to support high-precision touchpad scrolling (even without the Precision Touchpad API), yet not all applications handle it correctly and most touchpad drivers choose to play safe. The Precision Touchpad improves things by shifting the gesture processing to the OS itself and by using Direct Manipulation API for pixel-precise scrolling (however, that requires both compatible hardware and proper application support).

###### 3.4.3 Linux

Unlike Windows, Linux has to [reverse engineer](http://blog.forshee.me/2011/11/touchpad-protocol-reverse-engineering.html) touchpad protocols. At first, this was a disadvantage, but later on this approach started to bear its fruits — modern [Linux kernel](https://en.wikipedia.org/wiki/Linux_kernel) supports most touchpads out of the box, and everyone is free to tweak or modify any aspect of this support to one’s heart content (interestingly, MacBook’s touchpads are fully supported in Linux while the official Windows support leaves much to be desired).

Currently touchpad handling in Linux is split between the kernel and [X Windows System](https://en.wikipedia.org/wiki/X_Window_System). The kernel performs low-level communication with the hardware and then exposes device events via [evdev](https://en.wikipedia.org/wiki/Evdev)‘s `/dev/input/evenX` character devices. We can use `cat /proc/bus/input/devices` to list mapping between hardware and event interfaces. For example, on [HP 355 G2](http://h20564.www2.hp.com/hpsc/doc/public/display?docId=emr_na-c04329252) I get the following:

```
I: Bus=0011 Vendor=0002 Product=0007 Version=01b1
N: Name="SynPS/2 Synaptics TouchPad"
P: Phys=isa0060/serio1/input0
S: Sysfs=/devices/platform/i8042/serio1/input/input6
H: Handlers=mouse0 event5

```

This testifies that kernel driver processes data from a PS/2 port and makes the result available via `/dev/input/event5`. In a sense, this abstraction accomplishes the same goal as the Precision Touchpad protocol in Windows, though in software, not in hardware.

Subsequent data processing is done by [synaptics driver](https://wiki.archlinux.org/index.php/Touchpad_Synaptics) inside [Xorg server](https://en.wikipedia.org/wiki/X.Org_Server) and that’s where the scrolling gestures are handled. [Despite its name](https://who-t.blogspot.de/2016/12/xf86-input-synaptics-is-not-synaptics.html) (which is a legacy), the driver handles non-[Synaptics](https://en.wikipedia.org/wiki/Synaptics) touchpads as well. If we look through `/usr/share/X11/xorg.conf.d/50-synaptics.conf` we will see something like:

```
Section "InputClass"
    Driver "synaptics"
    MatchIsTouchpad "on"
    MatchDevicePath "/dev/input/event*"
EndSection

```

This means that this driver is used for all touchpads in Xorg, even for the MacBook ones (yet [Wayland](https://en.wikipedia.org/wiki/Wayland_(display_server_protocol)) uses [libinput](https://en.wikipedia.org/wiki/Wayland_(display_server_protocol)#libinput) as a touchpad driver). We can make sure of that by checking `/var/log/Xorg.0.log`:

```
config/udev:
Adding input device SynPS/2 Synaptics TouchPad (/dev/input/event5)
Using input driver 'synaptics' for 'SynPS/2 Synaptics TouchPad'

```

Now let’s pose the same question — suppose the driver receives the low-level data and recognizes a scrolling gesture, what should it do? In Linux, we don’t have to guess because we have the [code](https://cgit.freedesktop.org/xorg/driver/xf86-input-synaptics/): before version 1.7 the driver [emulated](https://cgit.freedesktop.org/xorg/driver/xf86-input-synaptics/tree/src/synaptics.c?id=xf86-input-synaptics-1.5.2#n2383) mouse button clicks — button 4 / 5 for vertical scrolling and button 6 / 7 for horizontal one. Now you might ask “what those mouse buttons have to do with scrolling?”. The answer is that X Server API was even more deficient than Windows’ one — it had no dedicated scrolling wheel events and represented wheel rotation as clicks of special mouse buttons. This was great from compatibility standpoint, but for high-precision scrolling it was a showstopper.

The only way to overcome the limitation was to introduce a new API that supports more precise scrolling events. This API happened to be [XInput 2.1](https://www.x.org/releases/X11R7.7/doc/inputproto/XI2proto.txt) Xorg [extension](https://www.x.org/wiki/guide/extensions/) which [introduced](https://lwn.net/Articles/485484/) high-precision scrolling events. The API is available since Xorg 2.12, and since version 1.7 the touchpad driver [uses](https://cgit.freedesktop.org/xorg/driver/xf86-input-synaptics/tree/src/synaptics.c?id=xf86-input-synaptics-1.9.0#n2850) the new API. By the way, another thing that was introduced in XInput 2 is separation between device coordinates and screen coordinates (i.e. it’s possible to acquire coordinates with subpixel precision).

If you have `xinput` package installed, you can easily test whether your touchpad generates high-resolution events in XInput — run `xinput test-xi2 --root` command and then perform two-finger scrolling, the output should contain something like:

```
EVENT type 6 (Motion)
  device: 7
  ...
  valuators:
    3: 142.00

```

The valuator value should reflect even slightest finger movements (you can use `xinput list --long` to lookup the device / valuator IDs).

While this is a definite improvement over the legacy API in terms of precision, the new API still retains the rotation / line bond: [XIQueryDevice](https://www.x.org/archive/current/doc/man/man3/XIQueryDevice.3.xhtml) function reports “increment” value that “specifies the value change considered one unit of scrolling down”. Thus, the API is an equivalent of Windows’ high-resolution mouse wheel events, and it’s unsuitable for implementing visually consistent, pixel-precise scrolling (unlike XInput 2, libinput can report scrolling directly in pixels).

Because the API is brand new, there’s no way legacy applications can take advantage of it without implementing the support explicitly. Even though the extension exists for quite a while, many applications and widget toolkits are still lagging behind. Even Firefox still has this [support](https://bugzilla.mozilla.org/show_bug.cgi?id=958868) disabled by default — try to run `env MOZ_USE_XINPUT2=1 firefox` to feel the apparent improvement in the scrolling precision. Chrome is barely [catching up](https://bugs.chromium.org/p/chromium/issues/detail?id=384970) as well.

Widget toolkits, such as [GTK+](https://en.wikipedia.org/wiki/GTK%2B) or [Qt](https://en.wikipedia.org/wiki/Qt_(software)), deserve special consideration, because applications that use their standard scrolling widgets ([GtkScrolledWindows](https://developer.gnome.org/gtk3/stable/GtkScrolledWindow.html) and [QScrollArea](https://doc.qt.io/qt-5/qscrollarea.html) respectively) may support high-precision scrolling out of the box. As of now, GTK+ 3 and Qt 5 support precision scrolling, yet GTK+ 2 and Qt 4 — don’t. Using GTK+ 3 or Qt 5 by itself doesn’t guarantee much, because applications might either rely on a non-standard scrolling implementation or use a line-based scrolling model (for example, the terminal window in [Ubuntu](https://en.wikipedia.org/wiki/Ubuntu_(operating_system)) 16.10 still lacks precision scrolling, the same goes for the terminal and the file browser in [Lubuntu](https://en.wikipedia.org/wiki/Lubuntu) 16.10).

Thus, unlike in Windows, the main obstacle for high-precision scrolling in Linux is not touchpad drivers but applications themselves. Because touchpad input handling is unified, applications are mostly open source and their update is automated, the future of precision scrolling looks more bright on Linux than on Windows, especially for “legacy” touchpads (yet, there’s no X server API for pixel-precise scrolling so far).

###### 3.4.4 Mac OS

OS X has followed almost the same road as the other operating systems. In [Handling Trackpad Events](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/EventOverview/HandlingTouchEvents/HandlingTouchEvents.html) guide Apple confesses that “technically, scroll gestures are not specific gestures but mouse events” with type of [NSScrollWheel](https://developer.apple.com/reference/appkit/nsscrollwheel). In the same way, in OS X Lion they [extended](https://oleb.net/blog/2011/08/whats-new-for-developers-in-lion-part-2/#scrolling-and-gestures) the [NSEvent](https://developer.apple.com/reference/appkit/nsevent) properties to supply high-precision deltas.

The fundamental difference is that Apple managed to provide all that as a complete package, so application developers could rest assured that all the parts work out of the box. Still, it’s not that unusual to stumble on an application that doesn’t support high-precision scrolling, even in Mac OS. For example, some code still rely on legacy [deltaY](https://developer.apple.com/reference/appkit/nsevent/1534158-deltay) and [deltaX](https://developer.apple.com/reference/appkit/nsevent/1534871-deltax) properties instead of the recommended [scrollingDeltaY](https://developer.apple.com/reference/appkit/nsevent/1535387-scrollingdeltay) and [scrollingDeltaX](https://developer.apple.com/reference/appkit/nsevent/1524505-scrollingdeltax) ones (notably, that’s what Java does). Another possible shortcoming is a use of line / row based scrolling model — for example, the built-in [Terminal](https://en.wikipedia.org/wiki/Terminal_(macOS)) can only scroll by line, the same goes for [LibreOffice Calc](https://en.wikipedia.org/wiki/LibreOffice_Calc) (please note that the discreteness of lines / rows is not a principal limitation — it is fully compatible with high-precision scrolling).

Early on, Apple recognized that, unlike mouse wheels, touchpads have no “notches”, “rotations” or “increments” that could correspond to lines, rows or other “scrolling units”. The high-precision scrolling is measured directly in pixels, not in fractions of “increment”. That’s why touchpad scrolling in Mac OS is uniform across different applications and doesn’t depend on the window content. The ability to control the position with pixel-level precision goes well with scrolling acceleration, when initial scrolling events are guaranteed to be 1 pixel shifts, but subsequent steps are increased to facilitate long-distance scrolling. The pixel-precise positioning together with system-wide scrolling acceleration (see below) produce that “Mac OS scrolling experience” people fond of (so, the essence is in software, not in hardware).

###### 3.4.5 Summary

As we have seen, all the major operating systems can support high-precision scrolling, however, to actually make it happen, the support must be available on many different levels, and each OS has its typical pitfalls that break the chain.

For an OS in a virtual machine (e.g. [VirtualBox](https://en.wikipedia.org/wiki/VirtualBox)) it’s even more complicated — both host OS and guest OS must propagate the high-precision events correctly and the virtual machine must be able to properly emulate them. We can greatly increase our chances by exposing touchpad to guest OS directly rather than emulating the device — for example, it’s possible to expose MacBook’s touchpad to a Linux guest inside a Windows host and to enjoy high-precision touchpad scrolling whereas host’s BootCamp driver doesn’t support it.

[Java virtual machine](https://en.wikipedia.org/wiki/Java_virtual_machine) also deserves a special consideration — although Java 7 introduced [getPreciseWheelRotation](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseWheelEvent.html#getPreciseWheelRotation--) method in `MouseWheelEvent`, proper native processing is available only on Windows (partially supported in Mac OS, not supported in Linux). Besides, [JScrollPane](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html) doesn’t actually use this data and continues to rely on the old, coarse-grained [getWheelRotation](https://docs.oracle.com/javase/7/docs/api/java/awt/event/MouseWheelEvent.html#getWheelRotation()) method. Moreover, Java API cannot represent “absolute” scrolling deltas, which are generated by Windows and Mac OS.

Despite that high-precision scrolling is often touted as a means of smooth scrolling, it’s only true for applications that use a naive approach to scroll event processing, and even then the “smoothness” is suboptimal and performance problems can be expected. As we have seen with the mouse pointer, both spatial- and temporal resolutions of input data are not enough to control the rendering directly and interpolation is the way to go. Typical touchpad precision can be even lower than mouse’s one — if we look at the [synaptics driver code](https://cgit.freedesktop.org/xorg/driver/xf86-input-synaptics/), we [can find](https://cgit.freedesktop.org/xorg/driver/xf86-input-synaptics/tree/src/synaptics.c?id=35b9472a189c88415fed137fb4c62a5081caaea5) comments like:

>  The default values 1900, etc. come from the dawn of time, when men where men, or possibly apes. … We expect to be receiving a steady 80 packets/sec (which gives 40 reports/sec with more than one finger on the pad. … We use this to call back at a constant rate to at least produce the illusion of smooth motion. 
> 
> 

— aren’t those those encouraging?

For a properly written scrolling implementation, high-precision scrolling events are mainly good for increasing the precision of positioning and for reducing the latency, yet these are still noble goals. Most modern browsers explicitly distinguish between “high-precision scrolling” and “smooth scrolling”, for example, as one Chrome developer [wrote](https://bugs.chromium.org/p/chromium/issues/detail?id=616308#c23):

>  The switch is named high-precision rather than smooth to avoid confusion with the scroll interpolation feature known as smooth scrolling. 
> 
> 

What if some touchpad can generate sufficiently high-resolution events — can we use those events directly, without interpolation? Unlike the case with mouse pointer, there’s no subsequent magnification of events by a scrollbar, so, in principle, if both spatial- and temporal resolutions are enough for a particular display, we *may* consider using the events without the prior interpolation. However, in practice, touchpad hardware is rarely capable of such a feat. Moreover, there’s no API to reliably detect these capabilities beforehand. That’s why, for instance, Chrome developers [decided](https://bugs.chromium.org/p/chromium/issues/detail?id=602769#c5) to always use the interpolation for touchpads on Windows and Linux.

Now you might ask “…but what about Mac, do they have superior hardware?” While Mac hardware is indeed very good, the underlying reason is different: because Mac OS applies system-wide scrolling acceleration, it can (partially) overcome the problem of insufficient temporal resolution on slow finger motion (as Microsoft [stated](https://msdn.microsoft.com/en-us/library/windows/hardware/dn613935.aspx) “If the finger is not moving or is moving very slowly, the frequency can be lower than 30 Hz.”) — by generating small enough deltas to make up for the low event frequency.

That said, there are reasons to employ the interpolation regardless of the event resolution (to improve rendering, as we will see later).

###### 3.5 Features

To acquire raw user input is only half the story — how we transform and interpret the input is equally important. There exists several important features that can significantly affect the resulting usability.

###### 3.5.1 Pixel-precise scrolling

As we’ve already discussed this topic together with the touchpad APIs, here goes a brief recap that summarizes the major points. There exist several distinct ways to represent the distance to scroll (“scrolling delta”), each method has its advantages when combined with appropriate input devices, and no single method is absolutely superior.

The first method came from the era of text mode — to measure the scrolling distance in lines / rows or a fixed number of lines / rows (in some OSes it’s also possible to use pages instead of lines). This method sets a correspondence between a physical step (“increment”) and a discrete number of visible lines (“scrolling unit”). Thus, it’s a perfect match for coarse-grained scrolling devices that have distinct steps (like a typical mouse wheel or buttons). For such devices, the method is just as relevant today, as back then.

Because unit-based scrolling lacks precision and it’s hardly compatible with high-precision devices (such as touchpads or high-resolution mouse wheels), all the major OSes proceeded along roughly the same path — they allowed to report the scrolling delta as a floating point number, instead of an integer. That solved the lack of precision just fine. However, this well-intended decision had some unintended consequences, because it still preserved the original “increment / unit” bond, which makes this method not very suitable for devices that rely on continuous scrolling. Because the actual scrolling distance (in pixels) depends on a particular “unit” height, visual scrolling speed becomes inconsistent across different applications. Another problem is that, although the resolution can be arbitrarily high, there’s no way for an OS to tell what delta corresponds to 1 pixel, which is crucial, for example, for touchscreen-based scrolling. All in all, this approach is a wrong way to support touchpad-like devices — the only input device that matches such an API is “a high-resolution mouse wheel that reports precise deltas, but, at the same time, has distinct physical steps” (this device is conjectural, as high-resolution wheels usually have no notches). Keep in mind that the high-precision angular (or “relative”) deltas can emulate the original coarse-grained deltas, if necessary. Most modern widget toolkits can make use of the high-precision input, e.g.: Windows GUI, Cocoa, GTK+ 3, Qt 5 (with Java Swing as a notable exception).

A better API for devices that offer continuous scrolling (for example, touchpads) is to measure scrolling directly in pixels — in this way, the resulting distance doesn’t depend on particular content metrics, and it’s under complete control of the OS. That’s how we can attain consistent visual scrolling speed across different applications. Please note that it’s not about increasing the *precision*, it’s about changing the *interpretation* of the value, which requires special support both at the level of OS and at the level of widget toolkit. Because the concept of pixel-precise (or “absolute”) positioning stretches the existing mouse-based APIs, not all operating systems / toolkits support it so far. The “absolute” positioning only complements, not replaces the existing “relative” APIs, because for step-based devices it makes perfect sense to map the steps to content-dependent units (like lines or rows) and such a mapping cannot be expressed in pixels, as the input subsystem is unaware of where and how the generated events will be used.

Currently only Mac OS has a sane API that supports both absolute- and relative scrolling deltas. Windows supports pixel-precise scrolling only in the combination of Precision Touchpad driver and Direct Manipulation API (yet Direct Manipulation is too all-encompassing and is never meant to used just for input). In Linux, Wayland can generate pixel-precise deltas, while X server still lacks that kind of an API.

As for cross-platform widget toolkits: Qt’s [QWheelEvent](https://doc.qt.io/qt-5/qwheelevent.html) has separate [angleDelta](https://doc.qt.io/qt-5/qwheelevent.html#angleDelta) and [pixelDelta](https://doc.qt.io/qt-5/qwheelevent.html#pixelDelta) properties. Although GTK+ [handles](https://github.com/GNOME/gtk/blob/7bee22bcb6ad9a71bfcf8a38edc070a685a627a3/gtk/gtkscrolledwindow.c#L1245) absolute deltas under the hood, it does that without a well-defined API. In JavaFX, [ScrollEvent](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/input/ScrollEvent.html) distinguishes between [pixel-based](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/input/ScrollEvent.html#getDeltaY--) and [line-based](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/input/ScrollEvent.html#getTextDeltaY--) scrolling values, while in Java, [MouseWheelEvent](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseWheelEvent.html) contains only “rotations”.

In web browsers, DOM Level 3 [wheel event](https://developer.mozilla.org/en-US/docs/Web/Events/wheel) explicitly separates pixel / line / page [deltaMode](https://developer.mozilla.org/en-US/docs/Web/API/WheelEvent/deltaMode) (the API is now [W3C](https://en.wikipedia.org/wiki/World_Wide_Web_Consortium)‘s Working Draft).

Pixel deltas are also good for supporting [touchscreen](https://en.wikipedia.org/wiki/Touchscreen) scrolling (which, obviously, requires pixel-precise positioning) without a separate API. For example, that’s what Mozilla [does](https://developer.mozilla.org/en-US/docs/Web/Events/wheel#The_deltaMode_value) by converting touchscreen-specific [WM\_GESTURE](https://msdn.microsoft.com/en-us/library/windows/desktop/dd353242.aspx) messages to the general-purpose `wheel` events.

###### 3.5.2 Scrolling acceleration

Scrolling *input* acceleration (don’t confuse with the *rendering* acceleration) is a feature that adjusts input scaling depending on the input velocity. For example, without accelerations, 1 cm finger movement always results, say, in 100 px content movement, regardless of the speed (the relationship between the input and the output is [linear](https://en.wikipedia.org/wiki/Linearity)). With acceleration, however, slow 1 cm finger movement might result in only 10 px content movement, while fast 1 cm finger movement might result in 1000 px content movement (thus, the relationship is non-linear). The feature mimics [acceleration](https://en.wikipedia.org/wiki/Acceleration) in physics, so that the user input controls not just velocity, but also the rate of change of the velocity.

The resulting dynamics depends on various OS-specific acceleration parameters. For instance, here’s the observed data from Mac OS X Sierra ([code](https://github.com/pavelfatin/scrolling-with-pleasure/blob/master/mac/) for acquiring the data):

![Scrolling acceleration](https://pavelfatin.com/images/scrolling-with-pleasure/scrolling-acceleration.png)
Built-in OS settings can usually adjust only the acceleration rate, while more advanced utilities might provide a way to fine-tune the acceleration curve directly ([example](http://ryanballantyne.name/2011/07/mice-run-faster-under-lions/)). In some cases, the parameters of scrolling acceleration tied to parameters of the mouse pointer acceleration. In other cases, for example in Linux, [pointer acceleration](https://wiki.archlinux.org/index.php/Mouse_acceleration) doesn’t cover the scrolling acceleration — tests ([code](https://github.com/pavelfatin/scrolling-with-pleasure/blob/master/linux/ScrollingLogger.c)) show that there’s no scrolling acceleration in Xorg.

What’s the purpose of scrolling acceleration? Here are the main advantages:

* It’s good for scrolling through large distances (which otherwise would be rather tiresome).
* It can boost the positioning precision on slow scrolling (like “Enhance Pointer Precision” does for the pointer motion).
* In combination with pixel-precise positioning, scrolling acceleration can be used to implement “pixel-perfect” scrolling — so that the initial delta is guaranteed to be 1 pixel, regardless of the particular hardware precision. In a sense, this is a way to unleash the pixel-precise positioning API in practice. This also reduces the input latency, because the initial hardware event always results in actual scrolling (without preliminary 1 pixel delta accumulation).

Despite the advantages, scrolling acceleration complicates hand-eye coordination and makes it harder for the user’s brain to predict the result of scrolling. Just like with the pointer acceleration, [proper implementation](https://www.x.org/wiki/Development/Documentation/PointerAcceleration/) is crucial to achieve the predictability.

Generally, it makes sense to apply the acceleration only to devices with continuous scrolling (e.g. touchpads, free-floating wheels, etc). Step-based devices (like a typical mouse wheel) rely on the tight increment / unit bond, which is disrupted by the scrolling acceleration so the result might feel rather unnatural. For example, because Mac OS applies scrolling acceleration to all mouse wheels (Apple mouses are “free-floating”, you know) people that use non-Mac mouses are often [frustrated](https://discussions.apple.com/message/25182869#25182869) (that’s a very good description of the problem, by the way). Some users even write [petitions](https://discussions.apple.com/thread/7500150?start=0&tstart=0) asking Apple to disable the acceleration for “external” mouses — all in all, a more flexible configuration would be definitely useful (third-party [USB Overdrive](http://www.usboverdrive.com/USBOverdrive/News.html) driver [offers](https://discussions.apple.com/message/28026322#message28026322) that flexibility).

In principle, if a particular OS doesn’t support scrolling acceleration, this feature can be emulated at the level of application (yet there’s no way to distinguish between the input sources). Ideally, scrolling acceleration should be configured system-wide to ensure the uniformity of user experience.

###### 3.5.3 Kinetic scrolling

Kinetic (or “momentum”) scrolling emulates [inertia](https://en.wikipedia.org/wiki/Inertia) of physical objects and continues the movement for some time after immediate scrolling events are stopped. The inertial scrolling is stopped either by user or by “friction”. Kinetic scrolling feels quite natural and, together with the scrolling acceleration, makes it easier to scroll through large distances.

Kinetic scrolling is triggered by a “flick” gesture when user lifts the fingers, so the feature is applicable only to touchpad / touchscreen scrolling.

Here’s the observed data from Mac OS X Sierra (note that, in the beginning, the image incorporates the acceleration curve from the previous chart, which constitutes the direct user input):

![Kinetic scrolling](https://pavelfatin.com/images/scrolling-with-pleasure/kinetic-scrolling.png)
Interestingly, due to a combination of the initial acceleration and the subsequent “coasting”, the resulting trajectory closely resembles the cubic ease in / out curve which we examined earlier (in application to mouse wheel).

Let’s take a closer look at the scrolling velocity (i.e. at the first derivative of the distance, or at how scrolling delta had been changing over time):

![Scrolling velocity](https://pavelfatin.com/images/scrolling-with-pleasure/scrolling-velocity.png)
As we can see, the second part of the chart is much more “smooth” — that’s because it contains *synthetic*, generated data (as opposed to the raw user input).

One way to implement kinetic scrolling is to do that at the level of operating system. For example, Mac OS itself might continue to send scrolling events for some time after user-generated input stops. In [Cocoa API](https://en.wikipedia.org/wiki/Cocoa_(API)) applications can rely on [momentumPhase](https://developer.apple.com/reference/appkit/nsevent/1525439-momentumphase) property to detect kinetic scrolling. At higher level of abstraction, for example, in Java, those events are indistinguishable from the user-generated input (which is OK, because all the necessary processing is already performed by the OS). Having system-wide support of kinetic scrolling is good for the consistency of user experience.

Windows’ Precision Touchpad driver also aims to support kinetic scrolling system-wide, however, the actual results can be very inconsistent because of the relatively poor application compatibility with high-resolution scrolling in general. If you use a custom touchpad driver, support of kinetic scrolling is at the whim of manufacturer’s fancy.

In Linux, the `xf86-input-synaptics` Xorg driver implements kinetic scrolling natively (as [coasting](https://www.x.org/releases/X11R7.6/doc/man/man4/synaptics.4.xhtml)), while `xf86-input-libinput` driver doesn’t support this feature [by design](https://wayland.freedesktop.org/libinput/doc/latest/faq.html#faq_kinetic_scrolling).

Another way to implement kinetic scrolling is to do that at the level of widgets. This might be necessary when particular OS doesn’t support kinetic scrolling directly. For example, both GTK+ and Qt support the functionality at the level of toolkit: GtkScrolledWindow has a [method](https://developer.gnome.org/gtk3/stable/GtkScrolledWindow.html#gtk-scrolled-window-set-kinetic-scrolling) to enable / disable kinetic scrolling, while Qt has [QScroller](https://doc.qt.io/qt-5/qscroller.html) class that enables kinetic scrolling for any scrolling widget. This approach is more flexible (because clients can differentiate the events), but potentially less consistent — each toolkit might implement the behavior differently (gesture detection, deceleration curve, etc).

If a particular application doesn’t rely on the standard scrolling widgets (e.g. a web browser) and OS doesn’t support kinetic scrolling natively, the only way to add the scrolling inertia is to implement it separately, at the level of application. Although, conceptually, this is relatively easy to do, the task requires processing of low-level “touch” events, because fingers should be lifted off the device for the inertial scrolling to start and user should be able to stop the scrolling simply by placing fingers back on a touchpad / touchscreen. Some libraries, like libinput, define special [scroll sources](https://who-t.blogspot.de/2015/03/libinput-scroll-sources.html) and generate dedicated high-level events that help to implement kinetic scrolling.

###### 3.5.4 Elastic scrolling

Elastic scrolling allows us to temporarily scroll past the edge of the content using a touchpad and to “bounce back” when we remove fingers from the touchpad. This action displays the content at positions that are outside of scrolling model and exposes underlying background:

![Elastic scrolling](https://pavelfatin.com/images/scrolling-with-pleasure/elastic-scrolling.jpg)
At the first glance, you might think that this feature is just a visual gimmick that has nothing to do with input as such. However, elastic scrolling helps to maintain a consistent mapping between finger- and content positions, and thus to provide a more predictable control and to create a feeling of “firm grip” on the content. Without this feature, excessive scrolling is simply discarded and when we reverse the direction, scrolling is started immediately, from arbitrary position of the fingers — this corresponds to slipping in the physical world, and, just like real slipping, this reduces the steerability.

The only desktop OS that offers elastic scrolling is Mac OS, which introduced the feature in OS X Lion. Existing alternative implementations fall short: GtkScrolledWindow [can](https://developer.gnome.org/gtk3/stable/GtkScrolledWindow.html#GtkScrolledWindow-edge-overshot) display visual “overshoot” indication when the content is pulled beyond the end, however the finger / view position correspondence is not preserved; likewise, Precision Touchpad driver in Windows 10 has “touch bounce” feature that yanks window in the direction of overscroll, but that’s just an animation without any input adjustments, moreover, because that animation is applied to the whole window, its effect might be too indirect and obtrusive (there are many guides on how to [disable touch bounce](http://errorfixer.co/disable-overscroll-animation-windows-10/) in Windows).

In principle, it’s possible to implement elastic scrolling at the level of application, though this would require handling of low-level touch events to detect finger contact — plain scrolling events are not enough. A potential drawback of supporting this feature at the application level is a loss of consistency, because such an application might behave differently from other programs. Widget toolkit is a different story — when toolkit is used as a foundation of OS graphical interface (e.g. in Linux), elastic scrolling can be supported system-wide and so the effect can be (more or less) consistent.

##### 4. Rendering

Although high-resolution input is good to increase positioning precision, reproduce dynamics and reduce latency, it’s rendering that ultimately determines the actual “smoothness” of scrolling. Rendering of scrolling has several unique features that worth detailed examination.

###### 4.1 Performance

The first consideration is rendering performance — application must be able to generate enough [frames per second](https://en.wikipedia.org/wiki/Frame_rate) (FPS) for the motion to be perceived as smooth. The temporal sensitivity of human visual system differs between individuals, yet, on average, it takes 240 – 250 FPS for the motion to be considered “realistic”. In practice, feasible frame rate is limited by display [refresh rate](https://en.wikipedia.org/wiki/Refresh_rate). The refresh rate of a typical LCD monitor is only 60 Hz, however, displays that support higher rates (e.g. 144 Hz, up to 240 Hz so far) are becoming widespread. Thus, the rendering should aim at least at 60 frames per second (or somewhat higher when V-sync is disabled). Ideally, we need to deliver up to 240 FPS if we are to unleash the full potential of contemporary monitors.

Can a typical desktop application deliver such a frame rate? It is highly unlikely — even computer games often struggle to reach 60 FPS, let alone go beyond that. You might say that computer games are more complicated than desktop applications, but then again, computer games are supposed to be run on powerful specialized hardware that meets “minimum requirements”, with modest screen resolution and reasonable settings to ensure quality / performance balance. Desktop applications, on the contrary, can be run even on a low-powered machine, that is, nevertheless, connected to a 4K-5K display, and there’s no automatic setting fine-tuning. Moreover, game engines are *designed* with [throughput](https://en.wikipedia.org/wiki/Throughput) in mind, while most desktop programs have nothing to do with the notion of “FPS”. That worked just fine in the past, but the rise of smooth / high-precision scrolling changed what is considered a sufficient rendering performance of desktop application.

If rendering in a particular program is simple enough to guarantee a decent frame rate, then such a rendering can be used directly, but applications that display more complex graphics can benefit from a neat trick known as “blit acceleration”.

###### 4.2. Blit-acceleration

Scrolling is a very specific type of animation — major portion of the image is simply moved to a new location “as is”, without any modifications. Can we take advantage of this peculiarity to speed up the rendering? It turns out that yet, we can.

Since [PDP-10](https://en.wikipedia.org/wiki/PDP-10) computers have so called [block-transfer instructions](https://en.wikipedia.org/wiki/Block-transfer_instruction) to efficiently copy one region of memory to another. In the field of computer graphics, this functionality corresponds to [bit blit](https://en.wikipedia.org/wiki/Bit_blit) which allows to combine multiple bitmap images together. The procedure is usually performed by [GPU](https://en.wikipedia.org/wiki/Graphics_processing_unit), directly in [video memory](https://en.wikipedia.org/wiki/Video_card#Video_memory) and it’s extremely fast. Thus, it’s possible to reuse the major part of the image and to paint just the recently exposed part, without re-drawing the whole area.

This technique is widely supported, both at the level of OSes and at the level of widget toolkits. The example of the former approach is Win32’s [ScrollWindow](https://msdn.microsoft.com/en-us/library/windows/desktop/bb787591.aspx) (or [ScrollWindowEx](https://msdn.microsoft.com/en-us/library/windows/desktop/bb787593.aspx)) function which scrolls contents of the specified window’s client area. The example of the latter approach is Java’s [JViewport](https://docs.oracle.com/javase/8/docs/api/javax/swing/JViewport.html) which uses [BLIT\_SCROLL\_MODE](https://docs.oracle.com/javase/8/docs/api/javax/swing/JViewport.html#BLIT_SCROLL_MODE) by default. Most standard scrolling widgets nowadays support blit-acceleration out of the box (for instance, see [how scrolling works](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/NSScrollViewGuide/Articles/Basics.html#//apple_ref/doc/uid/TP40003461-SW10) in Cocoa). Besides, it’s also possible for an application to perform bit blit directly, in case of custom scrolling implementation.

After a part of the content is moved, the “exposed” part needs to be re-painted, which is usually done via a standard painting mechanism in OS / toolkit (e.g. [WM\_PAINT](https://msdn.microsoft.com/en-us/library/windows/desktop/dd145213.aspx) message in Windows). In such a case, application needs to take actual “update region” (e.g. [GetUpdateRect](https://msdn.microsoft.com/en-us/library/windows/desktop/dd144943.aspx)) into account ant to perform a partial rendering (yet, for simplicity, many applications just re-draw everything regardless of the update region). To maximize the effectiveness of the technique, application must be able to efficiently partition its content area and to draw elements only within given [clip region](https://en.wikipedia.org/wiki/Clipping_(computer_graphics)#Clipping_in_2D_graphics). When it comes to text content, it’s usually easier to isolate a number of discrete lines rather than a specific segment in all the visible lines, so vertical scrolling gets a bigger boost than horizontal one (which is fine, considering the typical use cases).

Interestingly, hardware acceleration of scrolling is exactly how early “side-scrolling” video games [managed](https://en.wikipedia.org/wiki/Adaptive_tile_refresh) to compensate for the poor graphics performance of PCs in the early 1990s. Moreover, the “ancient approach” closely resembles what is now considered a “modern approach” (which we’ll discuss later).

###### 4.3 Double buffering

The idea of blit-acceleration looks so useful and elegant, right? OK, here’s the problem: as the existing image is stored in [framebuffer](https://en.wikipedia.org/wiki/Framebuffer), the result of the initial copying will be displayed immediately, possibly much earlier than the subsequent rendering is finished (i.e. those two actions are not “atomic”).

As a consequence, the scrolling can exhibit noticeable “flicker” (even though the partial update is [double-buffered](https://en.wikipedia.org/wiki/Multiple_buffering#Double_buffering_in_computer_graphics)). This effect looks almost like [screen tearing](https://en.wikipedia.org/wiki/Screen_tearing), except that genuine screen tearing splits the image only horizontally (because of how transmission over video interfaces works), while blit-copying can also split the image vertically (besides, the usual screen tearing cannot be captured using a screenshot — as it’s manifested in the *display*, not in the *framebuffer*). When there’s no V-sync (and so the ordinary screen tearing is also present), we might observe “double tearing” (horizontal + horizontal or vertical + horizontal) and that hardly adds to the visual “smoothness”.

Surprisingly, many built-in scrolling mechanisms do nothing to prevent that kind of flickering — for example, if we take Windows’ [example](https://msdn.microsoft.com/en-us/library/windows/desktop/hh298420.aspx) on how to scroll a bitmap and add a delay in `WM_PAINT` handler to emulate complex rendering ([code](https://github.com/pavelfatin/scrolling-with-pleasure/blob/master/windows/TearingExample.c)), we can observe that flickering in all its “charm” (and once there was a similar [issue](https://bugzilla.mozilla.org/show_bug.cgi?id=532544) in Firefox):

![Screen tearing](https://pavelfatin.com/images/scrolling-with-pleasure/screen-tearing.jpg)How can we circumvent this effect? An obvious way is to reorder the sequence of operations that constitute double buffering like that:
1. pre-render the partial update to the back buffer,
2. shift a part of the existing image using a bit blit,
3. copy the pre-rendered image to the framebuffer.

By swapping the first two steps we can minimize the pause between the image shift and the partial update. Particularly, this approach is [used](https://blogs.gnome.org/alexl/2009/02/10/how-to-remove-flicker-from-gtk/) in GTK 2.

Yet, if you think about it, such a solution is imperfect — it only minimizes the probability of flickering, not eliminates it. When V-sync is enabled, we can time the steps to reduce the probability more reliably, but when there’s no V-sync, the flickering is still quite possible as there’s no true “atomicity”.

The proper way to attain the atomicity is to maintain a “persistent back buffer”. The typical implementation of double buffering doesn’t preserve the back buffer content between the updates — each update draws over the existing content, so that the back buffer content might resemble something like ([code](https://github.com/pavelfatin/scrolling-with-pleasure/blob/master/java/BufferContent.java) that generated the image):

![Back buffer](https://pavelfatin.com/images/scrolling-with-pleasure/back-buffer.png)
As a result, the typical double buffering cannot be applied to scrolling. However, in principle, it’s possible for the back buffer to mirror the content in the frame buffer (simply by performing each drawing at a proper position), and in such a case, we can do the following:

1. shift a part of the existing image *in the back buffer* using a bit blit,
2. render the partial update to the back buffer,
3. copy the back buffer content to the framebuffer.

That’s how we can guarantee that the resulting image will be presented at once. This approach is used, for example, by OpenJDK’s [JViewport](https://docs.oracle.com/javase/8/docs/api/javax/swing/JViewport.html) which relies on so-called [true double buffering](https://docs.oracle.com/javase/8/docs/technotes/guides/swing/enhancements-6.html#DoubleBuffering). Another example is Apple’s [NSClipView](https://developer.apple.com/reference/appkit/nsclipview) which [performs](https://lists.apple.com/archives/cocoa-dev/2006/Nov/msg00525.html) scrolling in a “backing store”.

This method also solves the case when there’s a static background image that should not be moved on scrolling (so, we cannot perform a blit in the framebuffer) — in such a case, we can utilize [alpha compositing](https://en.wikipedia.org/wiki/Alpha_compositing) to support independent layers.

A modified version of that approach uses a back buffer that is larger than the visible area, so it’s possible to pre-render the partial update without shifting the existing image at all (at least, for some time) and then to copy the result to the framebuffer, starting from a new position. This method is used mostly by web browsers, because it goes beyond how typical widget toolkits work (it seems that only GTK 3 has [adopted](https://mail.gnome.org/archives/gtk-devel-list/2013-May/msg00011.html) this approach so far). Additionally, this technique makes it possible to pre-render yet-to-be-visible parts of the image ahead of time (for example, when user grabbed scrollbar’s thumb / placed fingers on a touchpad, but not yet started scrolling). In principle, it’s even possible to show the image that is incompletely rendered (or rendered at a lower resolution) — to increase the smoothness at the expense of so-called [checkboarding](https://www.chromium.org/developers/how-tos/trace-event-profiling-tool/anatomy-of-jank#TOC-Checkerboarding).

###### 4.4 Asynchronous rendering

So, now we know all about *how* to do the rendering, but… *when*? Surprisingly, “when” is an important topic in itself.

The naive way is to render the incoming scrolling events synchronously, upon receipt, and that’s exactly what most standard scrolling widgets do. Although this solution is great from the point of simplicity, it is seriously lacking from all other standpoints.

The first problem is that, as we already know, the resolution of scrolling events might be either too low or too high for the direct rendering. The both outcomes are equally undesirable:

1. When the frequency of scrolling events is lower than the display refresh rate, the resulting motion will be perceived as “jerky”. According to the Wikipedia article on [frame rate](https://en.wikipedia.org/wiki/Frame_rate), humans can process up to 15 images per second *individually*, and, “anything less than 46 frames per second will strain the eye”. As the ultimate goal is 240 FPS and the reasonable goal is, at least, the usual [technical limitation](https://en.wikipedia.org/wiki/Technical_limitation) of 60 FPS, the rendering frame rate cannot depend on the frequency of input events.
2. When the frequency of scrolling events is too high, the final outcome depends on a particular machine performance. If the hardware is capable of rendering frames at the higher than needed FPS, then the excessive frames will be (potentially) discarded, yet the CPU / GPU / power consumption will exceed what is really necessary. On less powerful hardware, the outcome is even less favorable — although most OSes / widget toolkits “coalesce” painting request (for example. see [consolidation](https://msdn.microsoft.com/en-us/library/windows/desktop/ms644927.aspx#queued_messages) of `WM_PAINT` messages), very few of them can merge scrolling requests. As a result, the queued scrolling events will be processed one by one, but at a slower speed, so that the rendering will “lag and freeze” (remember about the compatibility of legacy applications with high-resolution events?).

While the second scenario can be addressed by limiting the frequency of incoming scrolling requests at the level of application (by merging them), the first issue is more fundamental — it’s a typical problem from [digital signal processing](https://en.wikipedia.org/wiki/Digital_signal_processing) (DSP), when we need to increase the sampling rate at the output of one system so that another system operating at a higher sampling rate can input the signal. That problem has a typical solution, which is [interpolation](https://en.wikipedia.org/wiki/Interpolation#Interpolation_in_digital_signal_processing). The [asynchronous](https://en.wikipedia.org/wiki/Asynchrony_(computer_programming)) scheme works like the following:

1. Store the incoming scrolling request in a buffer.
2. Set a timer to schedule the rendering at fixed intervals (say, 8 ms).
3. Use interpolation to estimate the scrolling position at intermediate points.

That’s how we can smooth out the input and reduce the resolution of disparate input sources to a common denominator — the frequency of rendering (this also prevents [beats](http://www.blurbusters.com/mouse-125hz-vs-500hz-vs-1000hz/) caused by frequency difference between the event rate and the frame rate):

![Interpolation](https://pavelfatin.com/images/scrolling-with-pleasure/interpolation.png)
Please note that, in this context, “asynchronous” means only that we’re processing the events / rendering the frames separately — that doesn’t necessarily imply a separate thread (though, as we will see later, such a thread can be beneficial).

So far so good, this scheme indeed performs much better than the naive synchronous rendering, but here comes a catch question — what is the *ultimate* frequency of rendering? It turns out that we can not know beforehand: while typical computer monitors use something around 60 Hz, the exact number might vary from 58 Hz to 60 Hz; laptops might drop the refresh rate to 50 Hz in power-saving mode; gaming monitors support refresh rates up to 240 Hz (so far)… you get the idea.

The display is a thing-in-itself that updates its image at a fixed rate which we cannot control (at the level of application), so we need to “feed” it new frames at very specific intervals, to match the actual moments of the screen updates. The only reliable way to do this is to use [vertical synchronization](https://en.wikipedia.org/wiki/Analog_television#Vertical_synchronization) (V-sync). However, vertical synchronization is applied to the whole screen and, unless application runs in a full-screen mode, V-sync is controlled by OS / video driver / [window manager](https://en.wikipedia.org/wiki/Window_manager) — some configurations have it, some don’t.

When V-sync is disabled, the best thing we can do is just that — to set a timer and to render new frames at some reasonable rate (usually, somewhat higher than 60 Hz). In such a case, the display might show information from multiple frames and there will be [screen tearing](https://en.wikipedia.org/wiki/Screen_tearing). The tearing is less noticeable on vertical motion (which is our primary use case). Besides, we can reduce the manifestation of this effect by increasing the rendering frequency (so that the screen will have several narrower tears instead of a single wider one). Apart from the tearing, there’s a problem with timing — because the moment of the image rendering differs from the moment of showing the result, the approximated scrolling position might be slightly inaccurate, which lowers the fine-grained motion uniformity. Despite these issues, the absence of V-sync has its advantages: the frequency of rendering can be adjusted continuously and there’s no impact on the latency.

When V-sync is enabled, the story is very different. First, we better be able to keep up with the display refresh rate — because of [how V-sync works](https://hardforum.com/threads/how-vsync-works-and-why-people-loathe-it.928593/), it’s only possible to do the rendering at fractions of the display refresh rate (so, if, for example, the performance is enough only for 59 FPS, we will end up with 30 FPS as a result). Obviously, that’s a serious concern, and that’s why some video adapters offer “adaptive V-sync” option that will only turn on vertical synchronization when the frame rate of the software exceeds the display’s refresh rate (and that’s one more reason to exceed the refresh rate when we use a timer). A complete solution is adaptive synchronization technologies, such as [FreeSync](https://en.wikipedia.org/wiki/FreeSync) and [G-Sync](https://en.wikipedia.org/wiki/Nvidia_G-Sync), which adapts the *display* refresh rate to the rendering (this requires support from both the video adapter and the display). That said, we cannot expect any of these technologies to be available in each particular case, so we should be able to handle possible V-sync decently, as is.

Another fundamental problem with V-sync is that, even if we determine the actual refresh rate, we cannot rely on a timer to synchronize the rendering with the display frames perfectly — the resolution of [programmable timers](https://en.wikipedia.org/wiki/Programmable_interval_timer) is usually insufficient and also configuration dependent (e.g. it depends on whether [HPET](https://en.wikipedia.org/wiki/High_Precision_Event_Timer) is present / supported). The imperfect synchronization leads to discarded /skipped frames, which reduces the fluidity of animation. Surprisingly, human eye is very sensitive to small irregularities, so even a single dropped frame is quite noticeable (the effect is known as [“jank”](http://jankfree.org/)). Here’s [a very good video](https://www.youtube.com/watch?v=hAzhayTnhEI) that explain the problem in detail.

If interval timer is not enough, what should we do? The answer is V-sync-driven timer — we need an API that tells us when to render the next animation frame, so that the display will fetch it in time, regardless of particular refresh rate. Modern web browser has such an API, which is [requestAnimationFrame](https://www.html5rocks.com/en/tutorials/speed/rendering/). This API allows to register a callback that will be called either in-sync with the display refresh rate (when V-Sync is on), or by timer (when V-Sync is off). Additionally, the callback receives a sub-millisecond frame [timestamp](https://developers.google.com/web/updates/2012/05/requestAnimationFrame-API-now-with-sub-millisecond-precision) that can be used, instead of the current time, to better estimate the animation state (in our case — the interpolated scrolling position).

Some GUI toolkits offer similar APIs: GTK has [GtkFrameClock](https://developer.gnome.org/gdk3/stable/GdkFrameClock.html), [Qt Quick](https://www.qt.io/qt-quick/) scene renderers [can rely](https://doc.qt.io/qt-5/qtquick-visualcanvas-scenegraph.html#scene-graph-and-rendering) on OpenGL support of V-sync, JavaFX provides [AnimationTimer](https://docs.oracle.com/javase/8/javafx/api/javafx/animation/AnimationTimer.html) class. Still, many toolkits (e.g., Java’s AWT / Swing) lack such a support, which makes producing *truly* smooth animation impossible.

We can improve the scheme even further — because waiting for vertical synchronization [blocks](https://stackoverflow.com/questions/24135853/at-what-point-does-vsync-wait-block) the rendering thread, which, in many cases, is also used to process the input events, we can avoid the unnecessary blocking and increase responsiveness by performing rendering in a separate thread. For instance, that [was](https://bugzilla.mozilla.org/show_bug.cgi?id=555834) a problem in Firefox, which is now solved by [asynchronous panning and zooming](https://hacks.mozilla.org/2016/02/smoother-scrolling-in-firefox-46-with-apz/#checkboarding) (APZ) — the [reports](https://www.reddit.com/r/firefox/comments/2iq8sm/enable_async_panzoom_to_massively_increase/) are quite encouraging. Another example is Qt Quick [threaded renderer loop](https://doc.qt.io/qt-5/qtquick-visualcanvas-scenegraph.html#threaded-render-loop-threaded). Windows [Direct Manipulation API](https://msdn.microsoft.com/en-us/library/windows/desktop/hh446969.aspx) also claims that “To optimize responsiveness and minimize latency, Direct Manipulation processing occurs on a separate, independent thread from the UI thread. As a result, output transforms can run in parallel to activity on the UI thread.”

When implemented properly, scrolling animation looks substantially more “smooth” when V-sync is enabled. Because scrolling motion is relatively monotonous and lacks distinct phases, V-sync-induced latency is usually not an issue. However, because V-sync is applied system-wide, other activities (such as typing) might be [affected](https://pavelfatin.com/typing-with-pleasure/#methodology). Ideally, it should be possible to selectively bypass V-sync on latency-sensitive screen updates. Adaptive synchronization technologies, such as FreeSync or G-Sync, is an even better solution that offers the best of both worlds (though it’s not yet widespread).

##### 5. Display

Display is probably the less-adjustable element in the whole chain. However, certain display characteristics might influence scrolling rather significantly.

One property that worth mentioning is [refresh rate](https://en.wikipedia.org/wiki/Refresh_rate). As display’s refresh rate is the upper limit of actual [frame rate](https://en.wikipedia.org/wiki/Frame_rate), it determines the maximum possible smoothness of scrolling animation. Although most typical computer monitors are stuck at 60 Hz, more capable models are becoming more and more widespread: today’s gaming monitors offer at least 144 Hz, while high-end specimens can boast 240 Hz (i.e. the threshold of “real-life” motion). What is the *practical* frame rate limit? That’s no single answer to that so far: Wikipedia [says](https://en.wikipedia.org/wiki/Frame_rate#Frame_rate_and_human_vision)) “240 FPS when played back at normal speed on a 240 Hz monitor is also near the limits or about of perceivable smoothness”, but you might also check [this article](https://xcorr.net/2011/11/20/whats-the-maximal-frame-rate-humans-can-perceive/) or [that article](http://www.pcgamer.com/how-many-frames-per-second-can-the-human-eye-really-see/) (yet, keep in mind, that visual system of [some people](https://www.reddit.com/r/askscience/comments/1vy3qe/how_many_frames_per_second_can_the_eye_see/) might be especially sensitive, so better be safe than sorry).

Another thing is [LCD motion artifacts](https://www.blurbusters.com/faq/lcd-motion-artifacts/), including:

* Motion blur because of slow [pixel response time](https://en.wikipedia.org/wiki/Response_time_(technology)#Display_technologies) (and because of asymmetric pixel transitions).

These artifacts might appear as blur, dividing or flickering (in any combination). We’re not going to delve deep into the specifics, suffice it to say that display properties can affect scrolling big time, and all displays are not created equal.

##### 6. Configuration

Although both high-precision scrolling and smooth scrolling are surely nice, there are several use cases that require special consideration:

* Slow CPU / GPU — in such a case, the hardware might be unable to attain sufficient frame rate, regardless of the rendering optimizations. Since animation is destined to be “janky” anyway, we can resort to the coarse-grained mode, which, in these conditions, might perform better (because it’s at least *predictably* “janky”).
* Power saving mode — when a [low-power](https://en.wikipedia.org/wiki/Power_management) profile is activated, we can be pretty sure that either the battery is low, or the user wants to save the battery. In any case, it makes perfect sense to switch to the coarse-grained scrolling to consume less CPU cycles and thus to reduce the power consumption.
* Remote desktop — when [RDP](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol), [VNC](https://en.wikipedia.org/wiki/Virtual_Network_Computing) or [other](https://en.wikipedia.org/wiki/Remote_desktop_software) similar technology is in use, the smoothness of animation will be inevitably degraded during the transmission. In spite of that, a lot of CPU resources will wasted on both sides to produce / encode / decode the image (in addition to overconsuming the network bandwidth). When remote desktop is active, it’s wise to fallback the old school scrolling in order to improve the usability and to avoid wasting the resources unnecessarily.

In all these cases scrolling can be adjusted automatically, without user intervention. However, in some instances, user might want to have a direct control. For example, a particular display might exhibit too much ghosting / overdrive artifacts, or, user might choose coarse-grained scrolling simply because of personal preference (keep in mind, that human perception [really](https://en.wikipedia.org/wiki/The_dress) differs). For such occasions, it’s prudent to have a “Smooth Scrolling” checkbox in OS / application settings (for simplicity, it should probably affect both the interpolation and the processing of high-precision events simultaneously).

##### 7. Summary

Let’s summarize the main findings to get the big picture:

| Tip | Rationale |
| --- | --- |
| Clearly distinguish between smooth scrolling and high-precision scrolling. | To offer “smoothness” independently of the input precision. |
| Use pixel-accurate scrolling model. | To enable high-resolution- and smooth scrolling in principle. |
| Prefer a mouse with high-resolution sensor & high USB polling rate. | For more fine-grained control of scrollbar thumb. |
| Capture pointer motion with subpixel precision (when proper API is available). | To overcome the limitation of screen resolution. |
| Store scrollbar position with subpixel precision. | To actually benefit from the two previous recommendations. |
| Use contemporary APIs to acquire high-precision scrolling events. | To go beyond the legacy coarse-grained input. |
| Distinguish between angular- and pixel deltas (when possible). | To correctly handle both step-based input devices and devices that offer continuous scrolling. |
| With virtualization, capture high-precision events in host OS and retranslate them in guest OS. | To provide high-precision input for the guest machine. |
| Animate / interpolate all kinds of input (from scrollbar, mouse wheel, trackpad, keyboard, navigation, etc.). | To compensate for the insufficient spatial- and temporal resolutions of the typical sources. |
| Apply ease-in & ease-out curves. | To make the motion look more natural. |
| Provide scrolling acceleration / kinetic scrolling / elastic scrolling (when appropriate). | To improve control accuracy and to facilitate long-distance scrolling. |
| Use blitting (or a special buffer) to speed up the rendering. | To generate enough FPS even on less powerful hardware. |
| Make sure that the output is atomically double buffered. | To prevent rendering-related tearing. |
| Process the events and render the frames asynchronously. | To generate frames at a frequency that makes the motion smooth and that doesn’t depend on the frequency of input events. |
| Use V-sync or adaptive synchronization technology (preferable). | To avoid refresh-related tearing. |
| When V-sync is enabled, rely on a system API to properly synchronize the frame generation. | To attain perfect timing and to avoid “jank” (owing to skipped frames). |
| Perform the rendering in a separate thread. | To prevent interference from other activities and to avoid waiting for possible V-sync. |
| Use a display that supports high refresh rate. | To make the resulting motion more smooth. |
| Prefer a display with minimal motion blur / ghosting. | To reduce the image artifacts. |
| Switch to coarse-grained scrolling when hardware performance is insufficient, when remote desktop is active or when power saving is in use. | To improve the usability and to save the resources. |
| Provide an option to control high-resolution / smooth scrolling. | To account for individual preferences. |

Some of these recommendations can be implemented directly in application, some parts require support at the level of OS, and some actions can be taken by ultimate users.

Most findings are tested in practice — it’s possible not only to overtake, say, Safari, but to actually *surpass* it. With the right approach, this can be done in any toolkit (even in such seemingly “unfit” one as [Java Swing](https://en.wikipedia.org/wiki/Swing_(Java))). Thus, no magic is required — technology is the key.

A lot of work has been done in the area of scrolling, yet, there are still many things that can (and should) be improved, so we can indeed scroll with pleasure.

See also:

* [Typing with pleasure](https://pavelfatin.com/typing-with-pleasure/) — Human- and machine aspects of typing latency, experimental data on latency of popular text / code editors.

Tags: [acceleration](https://pavelfatin.com/tag/acceleration/), [asynchronous](https://pavelfatin.com/tag/asynchronous/), [blit](https://pavelfatin.com/tag/blit/), [buffering](https://pavelfatin.com/tag/buffering/), [ease](https://pavelfatin.com/tag/ease/), [elastic](https://pavelfatin.com/tag/elastic/), [high-precision](https://pavelfatin.com/tag/high-precision/), [interpolation](https://pavelfatin.com/tag/interpolation/), [kinetic](https://pavelfatin.com/tag/kinetic/), [mouse](https://pavelfatin.com/tag/mouse/), [scrollbar](https://pavelfatin.com/tag/scrollbar/), [scrolling](https://pavelfatin.com/tag/scrolling/), [sensor](https://pavelfatin.com/tag/sensor/), [smooth](https://pavelfatin.com/tag/smooth/), [tearing](https://pavelfatin.com/tag/tearing/), [touchpad](https://pavelfatin.com/tag/touchpad/), [trackpad](https://pavelfatin.com/tag/trackpad/), [wheel](https://pavelfatin.com/tag/wheel/)

##### 33 Comments

1. Both Java and JavaFX platform-specific implementations don’t relay high-precision scrolling events (with the exception of angular deltas in Windows):

 \* in Mac OS, [deltaY](https://developer.apple.com/reference/appkit/nsevent/1534158-deltay) and [deltaX](https://developer.apple.com/reference/appkit/nsevent/1534871-deltax) are used instead of [scrollingDeltaY](https://developer.apple.com/reference/appkit/nsevent/1535387-scrollingdeltay) and [scrollingDeltaX](https://developer.apple.com/reference/appkit/nsevent/1524505-scrollingdeltax);  
 \* in Linux, legacy [button events](https://www.x.org/releases/X11R7.7/doc/libX11/libX11/libX11.html#Keyboard_and_Pointer_Events_b) are used instead of [XInput 2.x](https://www.x.org/releases/X11R7.7/doc/inputproto/XI2proto.txt) valuators;  
 \* in Windows, only (angular) [WM\_MOUSEWHEEL](https://msdn.microsoft.com/en-us/library/ms645614.aspx) events are used, not Direct Manipulation API [pixel-precise events](https://codereview.chromium.org/1283913002/#ps20001).

 In Java, [MouseWheelEvent](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseWheelEvent.html) can represent only angular (albeit, high-precision) deltas. In JavaFX, [ScrollEvent](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/input/ScrollEvent.html) includes pixel deltas as well.

 In Java, [JScrollPane](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html) ignores high-precision scrolling events (both angular- and pixel ones). In JavaFX, [ScrollPane](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/control/ScrollPane.html) can handle high-precision events correctly.

 Neither toolkit can animate / interpolate scrolling.

 As a result: there’s no high-precision scrolling in Java (AWT / Swing), while JavaFX has a limited high-precision scrolling in Windows (you can check this by running [an example](https://stackoverflow.com/questions/32548011/smooth-panning-with-javafx) together with [HiResScrollWheelEmulator](https://www.codeproject.com/KB/cpp/HiResScrollSupp/Emulator.zip)). And even then, because there’s no interpolation, visual “smoothness” will depend solely on the actual resolution of particular hardware events: with certain touchpads / high-resolution mouse wheels, scrolling will be more or less smooth (but not *truly* smooth). Besides, there will be no smooth scrolling at all when an “ordinary” mouse wheel is used or when a touchpad / mouse driver doesn’t relay high-precision WM\_MOUSEWHEEL messages (which is a very typical case in Windows).

 For high-precision scrolling, we need to correctly relay high-precision events in each platform-specific runtime (besides, in the case of AWT / Swing, we also need to add pixel deltas to MouseWheelEvent and to employ high-precision events in JScrollPane). For smooth scrolling, we need to support interpolation in the standard scrolling widgets.
2. Thoroughly enjoyed this. Thank you!

 It’s not an exaggeration to say that scrolling changed the course of my career. It was the scrolling that impressed me so much on the first iPhone, ultimately leading me to become an iOS dev. My impression has always been that the iOS tech stack was built around optimizing scrolling.

 But I never knew what really went into it until I read this. The irony is that I’m always saying, “scrolling is cheap,” when we are debating UX designs.
3. I really enjoyed the depth of this article.  
 You have mentioned that Linux might have the edge in implementing these techniques.

 What would the steps be to advance Linux in this regard? I feel a bit overwhelmed by the different parts which need to be touched to get this to work.

 Still, I want to help bring this to life.
4. Linux might have an edge because it doesn’t depend on proprietary touchpad drivers — unlike in Windows, most touchpads can report high-resolution scrolling events in Linux. However, there are many, many ways to improve scrolling further. Some of them are obvious and are already implemented in other OSes, others are “innovations” that are still in the lap of the future.

 Sure, there is a lot of technical information that is related to scrolling, however, one really needs to “grok” the details to truly understand the big picture.

 Though I already described all the current shortcomings and the possible improvements in the article, here’s a brief summary of what can be done to improve scrolling (please note that I draw the state of affairs from memory, from the time of writing the article):

 **Possible roadmap for improving scrolling on Linux**

 **Applications:**  
 \* Store scrolling state with pixel-level precision to allow high-precision / smooth scrolling. Many applications, such as text editors, terminal emulators, spreadsheets and file managers still rely on line-based scrolling models.  
 \* Use XInput 2.1 API, which supports high-precision scrolling events. The legacy X Server API reports scrolling as clicks of special mouse buttons, which is neither precise, nor smooth. Many applications still rely on the old API (even Firefox still requires `env MOZ_USE_XINPUT2=1` to enable XInput 2.1). Applications that rely on standard scrolling widgets depend on the XInput API indirectly, via a particular widget implementation.

 **Input subsystem:**  
 \* Support subpixel pointer precision in XInput for mouses (currently, subpixel pointer coordinates are reported only for touchpads). This is required to get even more benefit from having a high-DPI mouse.  
 \* Support acceleration curves for scrolling. It seems that XInput applies acceleration curves only to pointer movement, but not to scrolling events. An ability to use custom acceleration curve for scrolling would make scrolling more convenient and create MacOS-like “feeling”.  
 \* Report pixel deltas in XInput 2. Currently, XInput 2 scrolling events are high-precision, but “dimensionless” (that is, just some numbers). We need to clearly distinguish between “relative” events (e.g. from a mouse wheel) and “absolute” ones (e.g. from a touchpad). I discussed that with [Peter Hutterer](https://who-t.blogspot.de/) and I can forward you the relevant email conversation (in short, he would be happy to find the volunteers).

 **Scrollbars:**  
 \* Capture pointer movement with subpixel precision. This is required to take advantage of high-resolution input devices (e.g. a high-DPI mouse / touchpad).  
 \* Store and report scrollbar thumb position with subpixel precision (and, subsequently, perform view position calculation using floating-point numbers). This is necessary to complement the previous point.  
 \* Experiment with on-demand pointer deceleration on slow thumb movement. This may help to overcome the inherent scrollbar limitation (content / track lengths mismatch). As far as I know, there are no real-world implementation of such a functionality so far, but the idea looks reasonable.

 **Scrolling widgets (or applications):**  
 \* Distinguish between relative and absolute scrolling deltas and handle them as appropriately (particularly, don’t tie pixel deltas to the content properties, such as line height). This will make scrolling uniform across different applications.  
 \* Interpolate mouse wheel events. Currently, only web browsers do that (which is known as “smooth scrolling”), but there’s no reason why standard scrolling widgets can’t do the same.  
 \* Interpolate scrollbar input. To my knowledge, no browser / scrolling widgets do that as of now, yet this concept is just a logical extension of mouse wheel interpolation and can help to “smooth” scrolling. I [experimented](https://github.com/JetBrains/intellij-community/blob/71f67e4a50127c031014054e726a32ce6f8dffba/bin/idea.properties#L144) with scrollbar interpolation in IntelliJ IDEA (AWT / Swing) — the result is very promising (scrolling via a scrollbar thumb is much more smooth).  
 \* Use “true double buffering”. Many widgets still either don’t use double buffering at all (which results in “pseudo-tearing” or use non-persistent double buffering (which only minimizes, but not eliminates the “pseudo-tearing”.  
 \* Render scrolling asynchronously, ideally with V-sync-driven timer — for frame-perfect, Vsync-ed animation.  
 \* Allow the back buffer to be larger than the visible area to pre-render content in batches and so to improve the scrolling performance.

 **System:**  
 \* Allow to increase USB polling rate for USB devices (statically or dynamically, on demand). Increasing the polling frequency can often improve input “smoothness” even with low-resolution mouses / touchpads.  
 \* Add system-level setting to enable / disable smooth-scrolling for all applications / widgets simultaneously.  
 \* Automatically disable smooth scrolling when RDP / battery saving is active.  
 \* Automatically disable high-precision scrolling with RDP / battery saving (particularly, switch to line-based scrolling, even via scrollbars).

 So, there are many areas for potential improvement to choose from. It seems reasonable to start with something simple, for example, with a particular application. You may consider contacting XInput / libinput / GTK / QT / application developers for more info. Besides, feel free to write [me](mailto:mail@pavelfatin.com) an email.
5. It seems, that it should be perfectly possible to implement circular scrolling for mouse by:  
 1) capturing the mouse pointer movement,  
 2) detecting the circular scrolling gestures (alongside with a pressed mouse button?),  
 3) generating proper mouse wheel events.
6. To get smooth scrolling etc, ideally the GUI would be running on a hard-real-time OS. Here’s an off-the-wall idea to approximate that:

 The JACK audio connection kit provides an infrastructure for passing real-time audio streams between audio hardware and applications. Why not treat the mouse, scroll wheel, etc as audio-generating devices, and feed their signals through JACK to applications? Then your scroll wheel events will update in real-time, throughout the software stack, at audio rates (96 KHz).

 “For those with experiences rooted in the Unix world, JACK presents a somewhat unfamiliar API. Most Unix APIs are based on the read/write model spawned by the “everything is a file” abstraction that Unix is rightly famous for. The problem with this design is that it fails to take the realtime nature of audio interfaces into account, or more precisely, it fails to force application developers to pay sufficient attention to this aspect of their task. In addition, it becomes rather difficult to facilitate inter-application audio routing when different programs are not all running synchronously.”  
 [link](http://www.jackaudio.org/api/)

 JACK is mature foss software, originally developed for desktop pro audio, and now used in [Android devices](http://developer.samsung.com/galaxy/professional-audio).

 Many applications would benefit from “misusing” JACK for non-audio real time binary data streams – from the user’s perspective the scrolling and every other gui event would be very smooth and low latency. JACK could evolve into the new standard for inter-process communication.

 After JACK has been generalized beyond audio, how best to interact with it? Since it is almost always binary data streams that we want to pass around in real-time, it would be fitting to evolve the old unix text oriented model (std in, std out, std error) into (binary data in, binary data out, view in, view out, error). For more on this see [Termkit](https://acko.net/blog/on-termkit/).
7. Ah I see. JACK passes audio data between applications in real-time with low latency by using the linux real-time extension. So instead of modifying the gui software and user applications to pipe non-audio data through JACK (a comically huge task), one could leave the gui and user applications as they are, and simply tell the kernel’s scheduler to allocate cpu time to them in round-robin fashion.

 So for example the thread which polls the mouse’s scroll wheel would run for x microseconds every y microseconds, and the thread that processes the updated scroll wheel signal would run just after that one, and so forth through the entire gui software stack. Thus the whole stack would be updated every y microseconds.

 Or, to prevent lots of cpu time being spent updating the gui when no user event happened, run just the mouse polling thread in round robin mode, and if there is a scroll event, the kernel stops whatever it is working on, and runs the gui stack.

 If you want to see if the rt kernel could improve the unmodified gui’s latency and smoothness, try out one of the preconfigured audio-focused real-time linux distributions. (Though note that since they are audio-focused, they may have \*reduced\* the priority of the gui threads in favour of audio-related threads. Thus you might have to tinker with the real time scheduler. Which might be fun!)

 To see if tweaking the scheduler improves the latency and smoothness of the unmodified Gnome gui, try [AVLinux](https://en.wikipedia.org/wiki/AV_Linux). For KDE, try [kxstudio](http://kxstudio.linuxaudio.org/).

 refs:  
 [“The scheduling requirements of JACK to achieve sufficiently low latencies have been one of the driving forces behind the real-time optimization effort for the Linux kernel 2.6 series… Real-time tuning work has culminated in numerous scheduling improvements to the mainline kernel and the creation of an -rt branch for more intrusive optimizations in the release 2.6.24, and later the CONFIG\_PREEMPT\_RT patch.”](https://en.wikipedia.org/wiki/JACK_Audio_Connection_Kit#Low-latency_scheduling)

 [“Through the use of the real-time Linux kernel patch PREEMPT\_RT, support for full preemption of critical sections, interrupt handlers, and “interrupt disable” code sequences can be supported. Partial mainline integration of the real-time Linux kernel patch already brought some functionality to the kernel mainline. Preemption improves latency, increases responsiveness, and makes Linux more suitable for desktop and real-time applications.”](https://en.wikipedia.org/wiki/Linux_kernel#Preemption)
8. Ok, the linux kernel does offer real time scheduling. But Pavel’s exhaustive analysis indicates the gui stacks cannot quite make use of a real time kernel due to internal delays, aggregating scroll wheel events, etc. What progress is being made on fixing the gui stacks? 

 [“29. September 2017 by Martin Flöser (developer of the KDE Plasma Compositor and Window Manager)

 Today I landed a change in KWin master branch to enable real time scheduling policy for KWin/Wayland. The idea behind this change is to keep the graphical system responsive at all times, no matter what other processes are doing.”](<https://blog.martin-graesslin.com/blog/2017/09/kwinwayland-goes-real-time/>)

 His strategy, as far as I understand, is to adjust the scheduler priority without further modification of the gui software stack. This is a step in the right direction, but won’t reduce delays within the KDE gui, if there are any. It also sounds like he’s told the scheduler to run the gui stack when there is a user input. 

 That’s progress! I wonder how it scrolls?
9. I don’t know what motivated you to go on a deep-dive into scrolling and report back to the world with your findings, but I love that you did. While the vast majority of the article is not new information to me, it collects a lot of useful technical details about input handling and UI rendering, so I’ll definitely keep a bookmark of this as a handy reference. 

 That said, hardware adaptive sync as implemented by freesync and g-sync don’t currently provide a solution to display framebuffer sync issues for windowed (non-fullscreen) applications. This is because the ‘refresh’ rate is still applied to the monitor as a whole, rather than actually allowing the computer to just update arbitrary pixels or quads of pixels.

 There’s some partial support for using adaptive sync monitors with windowed applications in Windows (possibly only with games and g-sync, not sure), but it basically works by just matching the monitor refresh rate to whichever window happens to be active / focused, all others be damned.

 Maybe in the future there’ll be more of a paradigm shift and we’ll get monitors that simply let you update arbitrary pixels whenever you like, within some maximum rate limitation and at some fixed latency. For now, as discussed you very much want to de-couple your input and render processing, threaded or not, and either:  
 a) Use a display-synchronized timer to trigger frame rendering, as you mentioned, or  
 b) Render constantly, keep two back-buffers, write out the current frame to whichever happens to be the older of the two, and then whenever vblank approaches you update the actual output framebuffer with whichever buffer isn’t currently being written to. This is really only better than option a if you have the processing power to render several frames between each monitor refresh, want to be sure you have the most up-to-date image possible, and don’t mind ‘wasting’ some processing power to get it.
10. Scrolling really is too interesting.

 In addition to using floats throughout the scroll model, let the floats represent meters, not pixels or lines or abstract “events”. Giving the floats physical units would heighten developers’ awareness of physical reality; this awareness would help them to make good decisions when designing a gui for different screen sizes, refresh rates, mouse sample rates, etc.

 For example, when a document is loaded into a text editor, its full length would be calculated in meters, as rendered on the current physical screen, with current font and column width.

 A float would represent distance in meters from the start of the document to the top of viewing window.

 Each mouse movement event means the hand moved a certain distance in meters. Each scroll wheel and trackball event means the finger moved a certain distance in meters. 

 For consistency the OS would have a single multiplier to convert distance the hand moves into distance the cursor moves on screen in meters. And another ratio to convert distance the finger moves over the trackball (or scroll wheel) to distance the cursor moves (or text line scrolls). In meters on screen.

 Upon seeing all these values in units of meters, the developer would immediately conceptualize the hand, finger and on-screen page as physical objects, and code accordingly.

 —  
 It just occurred to me that using a float to represent “position in the document” is synergistic with vector graphics. Using them together, you can achieve the ultimate in subpixel precision. Imagine a page of text rendered from a vector graphics font. As you scroll down, the top visible line of text can be chopped off at an infinite number of cut points. With anti-aliasing, you could render that line of text thousands of times as it rises up a distance of one pixel. Now THAT is what I call smooth scrolling!!!

 —  
 Here’s an alternate design for a scroll bar which makes full use of a float-based scroll model: after left clicking on the scroll bar “thumb”, move your hand down to scroll down, move left to decrease the hand-distance-to-on-screen-distance ratio, move right to increase the ratio. This gives you independent control of position-in-document and scroll rate, and thus allows you to land on any point in the document, regardless of length of document, number of pixels in the scroll bar, or size of “thumb”.

 Though frankly no scroll bar will ever be as nice as a good hardware scroll controller. The scroll ring on my trackball is ok; it helped me see that the ultimate scroll control is the jog shuttle wheel.

 Of course vi-style jump commands are the most efficient way to navigate through text because they allow one to keep one’s hands on the keyboard; perhaps scroll animation would make them less jarring. A keyboard with pressure-sensitive cursor movement buttons would also help to humanize scrolling in vi.

 I tried using a trackball to scroll in 2d. Since it sends high res events it is a practical off the shelf replacement for the low res scroll wheel on a mouse. But the Linux script I used translated the trackball events into mouse scroll wheel events. Since each scroll wheel event still makes text jump one line, the higher rate of events makes scrolling faster but not smoother. Oh well, at least the hardware part of the solution is in place.
11. I really enjoying this of this ! You digging the scrolling technology comprehensively.

 Actually in years ago (from the time i post this response) i just made an application base on MacOS by handling the scroll events for smooth scrolling.

 The application just use the easiest way to simulation the ‘true smooth scrolling’  
 It just capturing the system-wide scrolling event, and use liner-interpolation (lerp) to break up and reorganization those events, and repost it back to the system.  
 Not precision, but very useful.

 REPO: <https://github.com/Caldis/Mos>
12. We can use those programs to enable “smooth scrolling” in programs that support high-resolution scrolling events, but, for some reason, don’t interpolate mouse wheel input.
13. I just have to say this is such a fantastic article, and I also loved your article on typing with pleasure. This is all such good information that was previously scattered to the wind. Keep up the awesome work.
14. On Linux AntiX, while browsing/scrolling (irrespective of browser) with a touchpad, it regularly happens that the cursor will spontaneously scroll back to the top of the page.

 In /etc/X11/xorg.conf.d/synaptics.conf I changedVertEdgeScroll to zero, which seems to have helped a bit. Then, after reading your article I thought it might be a Coasting issue so in the same file I changed CoastingSpeed to zero, i.e. disabled Coasting. I have not tried it since the Coasting change, but is there any other setting I need to adjust – I am getting fed up with doing piecemeal adjustments that don’t solve the problem effectively and for good.
15. Thanks a ton for this write up, incredibly informative. Hoping to use some of it while adding actual precise scrolling support to SDL2 and implementing smooth scrolling on Linux in Revery.

 Question before I waste a bunch of time on it, would doing predictive pre-panning be a pointless optimization? I’d think you could make things feel just a bit more responsive by rendering half a frame ahead by whatever velocity is currently interpolated, keeping the average delta between on-screen content and what the user expects based on input just a bit lower. I \*think\* windows already does this for touchscreen input, but I’m not sure how well it would carry over to touchpads.
16. If you use both interpolation (or, more precisely, *extrapolation*) and V-sync-driven timer, you render an image at some position in the future, thus “predicting” the input and effectively reducing the *perceived* latency (that is, you are *compensating for* latency).

 Latency is mostly not an issue for scrolling. When a motion is relatively monotonous and has no distinct phases (comparing to, say, typing), it’s hard for humans to detect the *temporal* desynchronization. However, this is only applicable when the input and output are separated in space, as in the case with scrollbar / mouse wheel / touchpad input.

 When the input and output are physically combined, as in the case with a touchscreen or pen, the *spatial* desynchronization (which stems from the temporal desynchronization) is apparent, as demonstrated by the [excellent video](https://www.youtube.com/watch?v=vOvQCPLkPt4) from Microsoft Research. Taking all the input / output delays into account during interpolation / extrapolation doesn’t reduce the latency per se, but reduces the spatial mismatch, which is perceived as latency by human visual system.

 It should be possible to consider not just the V-sync delay, but input / output delays due to other factors (which are described in [Typing with pleasure](https://pavelfatin.com/typing-with-pleasure)). The problem however is that those delays are less predictable and are configuration-dependent. While V-sync timer tells you exactly when the frame will be presented, there’s no (genuine) timestamps on input events.

 That said, there’s probably some non-zero “scroll-ahead” that is the golden mean, and which is worth adding when touchscreens are involved. We may experiment with typical hardware / software configurations to determine the precise value.
17. @Pavel thanks for that detailed blog post. Unfortunately I did not have the time to read it completely.

 I came here from an issue that I created to improve scrolling on Linux: <https://gitlab.freedesktop.org/libinput/libinput/issues/185>

 So I thought I’d give you a heads up.

 You also created a roadmap for this: <https://pavelfatin.com/scrolling-with-pleasure/#comment-68549> however it seems a bit outdated because you are still talking about XInput, correct?

 I created a meta issue to keep track of the development on this: <https://gitlab.gnome.org/GNOME/gnome-control-center/issues/379>

 Maybe your roadmap can be merged into this (after bringing it up to date)? Let me know what you think.
18. I’ve done some work to get precise scrolling to be supported on SDL2, and have started work on <https://github.com/szbergeron/libscroll> to try to get some standard input processing library going. Hopefully it works out, not sure what kind of adoption it’s likely to see.

 BTW, I heard you did some work on getting smooth scrolling into Intellij but it only seems to work on windows for me. Is there a way of getting it working on linux too?
19. It amazes me how relevant nice scrolling still is for Linux.

 Any chance you have had a look at this topic in 2021? Wayland has improved a lot, Elementary OS and Ubuntu have pixel perfect scrolling…

 What is your opinion on todays state of the matter?
