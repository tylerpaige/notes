---
Author: molily.de
Full Title: What Google AMP means for the JavaScript community · molily
Category: #articles
Publication date: 2017-05-28
URL: https://molily.de/amp/
Summary: By neglecting web performance, the JavaScript community unintentionally paved the way for AMP.
---

![rw-book-cover](https://molily.de/img/spidermum-gray-bg-square-small.png)

# Notes

[[molily.de - What Google AMP means for the JavaScript community · molily]]

***

**By neglecting web performance, the JavaScript community unintentionally paved the way for AMP.**

Google’s [Accelerated Mobile Pages project (AMP)](https://www.ampproject.org/) focuses current conflicts on the web like a lense: The mobile revolution, trustworthy and sustainable journalism, content monetarization and advertising, web standardization, web performance and tech industry monopolies.

A lot has been said about AMP. In this post, I won’t explain what AMP is and I won’t reiterate all criticism it drew. Please see these introductory posts on AMP:

I’d like to talk about one detail of the whole AMP picture: It’s use of JavaScript and its stance on JavaScript usage.

#### The JavaScript Problem

Since I’ve made contact with JavaScript, I’ve contemplated on the possible, reasonable and beneficial uses of JavaScript. Ever since JavaScript gained ground on the web, it was misused and overused: Annoying popups. Impaired user interaction. Fragile, browser-specific, inaccessible code. Using JavaScript when [a more robust technology further down in the stack](https://molily.de/javascript-web-apps/) would be the better tool.

[In 2005 I wrote](https://blog.selfhtml.org/2005/12/17/javascript-einsatz/): “Today JavaScript is stuck in a serious crisis, just check the current JavaScript usage for usability, accessibility and compatibility.” Users weren’t fully convinced that JavaScript is used for their benefit. A fraction of web-savvy people even deactivated JavaScript completely, and [some still take such measures](https://www.torproject.org/projects/torbrowser.html.en) for performance, privacy and security reasons.

Fast-forward to today. The evolution of web standards, the advancement of JavaScript as a language with powerful APIs has removed some pitfalls. JavaScript is an essential part of the web, but it’s misused like never before. Most importantly, JavaScript is a major performance bottleneck for rendering web content on mobile devices with slow connections and less computing power.

[The mobile web user experience is frustrating](https://molily.de/mobile-web-performance/), and JavaScript is at least partly to blame. Unfortunately the JavaScript performance problem is complex. Here’s just a quick, incomplete overview:

* Too many scripts are loaded from several servers. On certain sites, scripts from third-party services dominate and are out of control.
* Scripts are large, generate network traffic and cost the users money.
* The code of scripts, especially library and framework code, is mostly unused on the requested page. Similar code is shipped multiple times.
* Script loading, parsing and execution blocks loading and rendering of the most important content.
* Scripts load content lazily so the page builds up gradually. Content jumps around, which frustrates user interactivity.
* Script execution puts pressure on the CPU, interferes with user interaction and drains the battery.
* Scripts disrupt the rendering cycle of the browser, causing “yank” and interruptions.
* Scripts are [still fragile](https://kryogenix.org/code/browser/everyonehasjs.html) because of network failure, different browser runtimes and compatibility issues.
* Advertisement and analytics scripts collect data about the device, create fingerprints and save them in a local storage, violating the user’s privacy.

It’s important to realize that the JavaScript problem lies at the heart of the miserable state of web performance.

#### Web performance optimization falls short

Removing these performance obstacles requires a thorough, manual audit of a site. It requires custom JavaScript development to analyze, reduce and refactor the scripts on the site. This is a continuous process for frequently-updated sites.

Unfortunately, a lot of web authors are not aware of these problems, lack the technical expertise or the resources to address them. Every web engineering department today would need a web performance specialist with strong knowledge of JavaScript. But compared to all web development and JavaScript programming, the web performance crowd is tiny, so there’s a shortage of workforce.

Since most scripts are written by third parties or consist of third-party library code, it’s not a problem an individual site can tackle. It’s a problem of the web ecosystem as a whole.

#### Google weighs in with its market power

Google is the biggest company that bets on the success of the web. Google not only dominates web search, but also mobile operating systems, advertising, mapping, web-based office tools, e-mail, video and analytics. It was mainly Google that advanced the web from a document delivery system to a rich application platform under the “HTML5” umbrella. Today, Google has an almost absolute power over the web and its technical progress.

Google examined the messed-up web performance situation and looked for a solution. Google recognizes that it cannot change the whole web in one day. So it creates new web standards, builds a faster browser, educates web authors and provides authoring tools with performance principles baked-in. Since Google depends on web content producers, it relies on the “democratic” approach of developer evangelism.

Two steps are necessary to cut the Gordian knot of web performance:

1. The expertise and the [means](https://en.wikipedia.org/wiki/Means_of_production) to come up with a viable solution.
2. The power to implement and enforce this solution.

It turns out the web community lacks both. It’s not the case that no one has the expertise, but the means are scattered and the efforts are uncoordinated. More importantly, nobody in the web community has the power to enforce rules on a large amount of the sites.

Except for Google.

#### An authoritarian solution

Google realized the “democratic” approach was not able to alter the course quickly. So Google tried an “authoritarian” approach. In my opinion, AMP mirrors the power structures of the web, especially Google’s political and economic predominance.

In a bold move that Silicon Valley capitalists would surely praise as “disruptive”, Google decided to put away with all the messy JavaScript code that is slowing down the web. Accelerated Mobile Pages (AMP) were born.

Creating a viable solution is just half the work. Forcing it on the larger web is the other half. So Google favors AMP pages in the Google mobile search. AMP pages appear in the “news carousel” at the top and are marked with a lightning symbol. They are loaded “inline” so navigation between the search results list and the individual search result is seemless.

#### One JavaScript to Rule Them All

AMP is a complex technology with numerous requirements and parts. But at the heart lies the idea to write *One JavaScript to Rule Them All*. AMP gets a large amount of its speed improvements from banning almost all existing JavaScript code. Google puts on the emergency brake for client-side scripting in order to start from scratch.

With AMP, you only need one JavaScript, the so-called AMP runtime. This is [an open-source JavaScript developed by Google](https://github.com/ampproject/amphtml). It’s not a library you can use in your own JavaScript. In fact, you shouldn’t write JavaScript any longer. Instead, you declare custom elements in your HTML and AMP’s JavaScript turns them into interactive components.

[Loading third-party JavaScript code](https://www.ampproject.org/docs/guides/third_party_components) for advertisement, video content and social media widgets is possible, but it’s controlled and rendered harmless by AMP’s JavaScript. Third-party code does no longer interfere with the loading of the main content.

AMP’s JavaScript is neither particularly small nor is it written in a revolutionary new way. The script infrastructure and its components simply adhere to a [set a principles](https://www.ampproject.org/learn/about-how/) that eventually lead to high performance. In that, AMP’s JavaScript is a great piece of engineering that sets a new standard for web interactivity.

We’ve seen other declarative solutions that tried to “reset” the JavaScript world. But only Google has the power to force its solution on web authors. One could also argue that it was inevitable that Google came up with an authoritarian approach given its monopoly on mobile search.

#### The Open Web community should concede defeat

One and a half year after Google launched AMP, several notable media outlets jumped on the bandwagon. Initially, Google envisioned AMP as a format for news articles for mobile reading. But according to the [AMP roadmap](https://www.ampproject.org/roadmap), Google plans to “broaden support toward a variety of content formats, including news articles, recipes, local listings, product listings, and more”.

In my opinion, AMP is a defeat for the Open Web community against privatization, monopolization and centralization. This loss is somehow self-inflicted. The Open Web community failed to provide clear and easy performance frameworks, rule sets and validation tools like AMP does. In the face of the mobile revolution, the community failed to make the web accessible to mobile devices.

Especially the JavaScript community failed to address the rampant misuse of JavaScript and JavaScript’s adverse effects on web performance. The average site nowadays [loads 23 scripts with a total amount of 426 kB](http://www.httparchive.org/trends.php#bytesJS&reqJS) of compressed JavaScript code. Today it’s easy to set up a project with React, Angular or even jQuery, add some common third-party scripts and end up with megabytes of blocking JavaScript code from several servers.

The JavaScript community innovated quickly in the last years. Unfortunately, establishing performance metrics and rules, fighting fragmentation and reconciling similar approaches was not a goal. The situation got so bad that one powerful company that builds its business on the web tries to hit JavaScript’s reset button.

#### Democratize web performance

It is no surprise that a unilateral, authoritarian solution is more effective than scattered efforts of JavaScript and web performance experts. But it’s neither desirable nor sustainable to build a “second web” primarily for Google mobile search. Banning all existing JavaScript and rewriting it from scratch is not a viable solution. So the impact of AMP on the larger web is still unclear.

What is clear to me is that the JavaScript community needs to sweep in front of its own door. It needs to criticize AMP but also learn from AMP. The broad web community needs to adapt technical principles that improve performance. But instead of following a centralized, authoritarian approach, we need to fix the web in a concerted community effort.

*I’d love to hear your feedback! Send an email to [molily@mailbox.org](mailto:molily@mailbox.org) or [message me on Twitter: @molily](https://twitter.com/molily).*
