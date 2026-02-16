---
Author: Software is Crap
Full Title: Why do we keep building rotten foundations?
Category: #articles
Publication date: 2016-07-04
URL: https://davmac.wordpress.com/2016/07/05/why-do-we-keep-building-rotten-foundations/
Summary: https://davmac.wordpress.com/2016/07/05/why-do-we-keep-building-rotten-foundations
---

![rw-book-cover](http://i2.mirror.co.uk/incoming/article2784712.ece/ALTERNATES/s615/Tony-Robinson-Blackadder.jpg)

# Notes

[[Software is Crap - Why do we keep building rotten foundations?]]

***

APIs are like bones: sometimes you have to break them to mend them, and it fucking hurts.

It’s not a perfect analogy, but it reflects an observation of (superficially) pragmatic behavioural tendencies, even if not a necessary truth. A broken implementation is bad, but it can be repaired. A broken API can be repaired too, but anything built on it then also needs to be fixed. And often when this occurs we like to throw the baby away with the bath water, start afresh, make a new clean design where we apply the lessons learned in the past attempts and produce something that, this time, will be perfect. Except, of course, we don’t always remember all the lessons, and it never is.

For instance, look at GTK. There was a recent [blog post detailing plans for GTK development](https://blogs.gnome.org/desrt/2016/06/13/gtk-4-0-is-not-gtk-4/) (and a [further followup](https://blogs.gnome.org/desrt/2016/06/14/gtk-5-0-is-not-gtk-5/comment-page-1/)), which starts by discussing:

> … the desire to create a modern toolkit with new features vs. the need to keep a stable API.
> 
> 

… which in my opinion, should not be a conflict, but let’s work our way there. Some choice snippets:

> … and created a hesitation to expose “too much” API for fear of having to live with it “forever”.
> 
> 

(A legitimate concern, and one that gets to the heart of the matter, but let’s stick to the piece for now).

> We want to improve this, and we have a plan.
> 
> 

And of course I read this and I am worried.

![](https://i0.wp.com/i2.mirror.co.uk/incoming/article2784712.ece/ALTERNATES/s615/Tony-Robinson-Blackadder.jpg)“Don’t worry Mr B, I have a cunning plan to solve the problem”

>  We are going to increase the speed at which we do releases of new major versions of Gtk (ie: Gtk 4, Gtk 5, Gtk 6…). We want to target a new major release every two years.
> 
> 

Urgh.

> The new release of Gtk is going to be fully parallel-installable with the old one. Gtk 4 and Gtk 3 will install alongside each other in exactly the same way as Gtk 2 and Gtk 3 — separate library name, separate pkg-config name, separate header directory. You will be able to have a system that has development headers and libraries installed for each of Gtk 2, 3, 4 and 5, if you want to do that.
> 
> 

No! DO NOT WANT! It’s bad enough having Gtk 2 and Gtk 3 (thankfully Gtk 1 is long gone), and you want me to have to litter my system with even more Gtk-sized turds… Please, no.

Oh well, I guess it can’t get any wors-

> Meanwhile, Gtk 4.0 will not be the final stable API of what we would call “Gtk 4”. Each 6 months, the new release (Gtk 4.2, Gtk 4.4, Gtk 4.6) will break API and ABI vs. the release that came before it.
> 
> 

Oh for fucks sake, really? So your “major new release every two years” is, in effect, actually a major new pain-in-the-proverbial every *6 months* fuck fuck fuck.

> We will, of course, bump the soname with each new incompatible release — you will be able to run Gtk 4.0 apps alongside Gtk 4.2 and 4.4 apps, but you won’t be able to build them on the same system
> 
> 

Right, so every 6 months there will effectively be a new version of GTK with its own set of libraries and its own themes (getting to that, bear with me) and our systems will become a myriad of GTK because all the new apps developed during this time aren’t going to have any clue about which version they should target and trying to keep up the with latest API is going to be a futile effort because it’ll pretty much keep changing out from under your feet. Shit, there are still apps making the transition from Gtk 2 to Gtk 3 and they’ve literally had years. (I could at this point tell you how I much prefer Gtk 2 apps anyway, but there’s probably enough material there for a whole other blog post).

> Before each new “dot 0” release, the last minor release on the previous major version will be designated as this “API stable” release. For Gtk 4, for example, we will aim for this to be 4.6 (and so on for future major releases).
> 
> 

I could mention that this seems like exactly the opposite of how major versions should work but, oops I just did. Just to illustrate:

4.0: development, 4.1: development, 4.2: development, …, **4.6: stable**, 5.0: development, …

I mean, if 4.6 is the culmination of a series of changes leading to the next major release, then why *isn’t it the next major release*? Since when does “X.0” not designate a stable release?

> “Gtk 4.0” is the first raw version of what will eventually grow into “Gtk 4”, sometime around Gtk 4.6
> 
> 

Oh lordy *just listen to yourself for one second.*

But really, forget about the ridiculous version numbering scheme; the real problem is the rapid-fire development with lack of API/ABI stability producing a plethora of incompatible versions. Why is this even necessary? Could I suggest that, rather than pumping out new and exciting APIs/features at a faster pace, what GTK really needs to do is sit down and flesh out a decent API that it *won’t have to break every 6 months, for crying out loud*. Because people are going to try to build software on top of your crappy toolkit, and you should accept some responsibility for the API design rather than kicking them in the nether regions twice a year. And if you really don’t want people to try and keep up with the latest and greatest and expect them to keep developing on GTK 3 despite that fact that you’re up to version 4.4 (or whatever) then at least use a sensible versioning scheme that reflects the notion of *development and instability* without having to learn that GTK devs used a different scheme than the rest of the software world, just because they felt like it.

> this gives many application authors and desktop environments something that they have been asking for for a long time: a version of Gtk that has the features of Gtk 3, but the stability of Gtk 2.
> 
> 

You could give them that *right now* if you would just stop screwing around with the API. An API is stable if you don’t mess with it. And if you’re having to mess with your API on a continual basis*, you’re doing it wrong.* Like, the whole software development thing, all wrong.

By all means, declare *parts* of your API as unstable, document them as such and adjust them during a period of development, and then declare them stable (and never touch them again!). But the notion that you think it’s OK to potentially break the whole thing (or more accurately, any part of the whole thing) at any moment is disturbing. (Maybe that’s not what’s really meant; I certainly hope so, but it’s not clear). This idea that you’ll bang away in a frenzy with your software hammers for a few years and what comes out at the end will be “stable” is just hokey. It’s the wrong approach. You’re off the runway and into the harbour.

I really wonder, would it have been so hard to have GTK 3 *add* to the GTK 2 API rather than actually break it? I mean I know there’s the whole CSS thing (which still feels like a sick joke, frankly, especially because of the half-arsed implementation) and HiDPI support, but could these not have been implemented on top of the existing API? Could not the GDK API have been implemented on top of Cairo (if it wasn’t already), and retained for backwards compatibility? Etc etc? Yes, it’d be more work – much more work, perhaps – but that work only needs to get done once, rather than in *each separate application* that builds on the toolkit. And if it’d had been thought through properly at the start, it could have given us a new toolkit with a lot less pain.

> The most common criticism towards GTK+ is a lack of backwards-compatibility in major updates, most notably in the [API](https://en.wikipedia.org/wiki/Application_programming_interface)[[21]](https://en.wikipedia.org/wiki/GTK%2B#cite_note-21) and theming.[[22]](https://en.wikipedia.org/wiki/GTK%2B#cite_note-22)
> 
> 

(I promise I did not edit that in myself. And I mentioned theming earlier, so I should elaborate: not only are GTK 2 themes not compatible with GTK 3, but GTK 3 themes aren’t either… it seems [GTK 3.18 themes don’t generally work well with GTK 3.20](https://github.com/mate-desktop/mate-themes/issues/114), for instance).

That it’s so generally acknowledged that the API stability sucks is really telling.

GTK is a foundation library. And it’s rotten, and they know its rotten. And they keep tearing it down and replacing it – with another rotten foundation. And we all suffer because of it.

*Note: I started out writing this post intending to discuss API breakage in general; unfortunately GTK is such an easy target that the whole post became about the one toolkit.*
