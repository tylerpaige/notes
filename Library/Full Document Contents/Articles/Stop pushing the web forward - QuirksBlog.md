# Stop pushing the web forward - QuirksBlog

![rw-book-cover](https://quirksmode.org/pix/logo_quirksmode_twitter.png)

## Metadata
- Author: [[quirksmode.org]]
- Full Title: Stop pushing the web forward - QuirksBlog
- Category: #articles
- Publication date: 2015-07-28
- URL: http://www.quirksmode.org/blog/archives/2015/07/stop_pushing_th.html
- Summary: The author suggests taking a break from adding new features to web browsers for about a year to focus on using existing features more responsibly and having a fundamental conversation about the future direction of the web. The proposal aims to shift the focus from constantly emulating native apps to enhancing the overall user experience and reducing the reliance on polyfills and unnecessary tools. The moratorium on new features would encourage browser vendors to prioritize bug fixes, improve existing features, and ultimately promote a more thoughtful and innovative approach to web development.

# Notes

[[quirksmode.org - Stop pushing the web forward - QuirksBlog]]

***

This is the blog of [Peter-Paul Koch](https://www.quirksmode.org/about/), web developer, consultant, and trainer. You can also follow him on [Twitter](http://twitter.com/ppk) or [Mastodon](https://mastodon.social/@ppk).  

[Atom](http://www.quirksmode.org/blog/atom.xml) [RSS](http://www.quirksmode.org/blog/index.xml)

If you like this blog, why not [donate a little bit of money](https://www.quirksmode.org/donations.html) to help me pay my bills?

Categories:

Fair warning. You’re going to hate this one. I want to stop pushing the web forward for a while. I want a moratorium on new browser features for about a year or so.

Recently I’ve been having serious doubts about the whole push the web forward thing. Why should we push the web forward? And forward to what, exactly? Do we want the web to be at whatever we push it forward to? You never hear those questions.

Pushing the web forward currently means cramming in more copies of native functionality at breakneck speed — interesting stuff, mind you, but there’s just too *much* of it.

Quick, name all the new features browsers shipped in 2015! You see? You can’t. That’s the problem.

We get ever more features that become ever more complex and need ever more polyfills and other tools to function — [tools that are part of the problem](https://www.quirksmode.org/blog/archives/2015/05/tools_dont_solv.html), and not of the solution.

I don’t think this is a particularly good place to push the web forward to. Native apps will [always be much better at](https://www.quirksmode.org/blog/archives/2015/05/web_vs_native_l.html) native than a browser. Instead, we should focus on the web’s strengths: simplicity, URLs and reach.

The innovation machine is running at full speed in the wrong direction. We need a break. We need an opportunity to learn to the features we already have responsibly — without tools! Also, we need the time for a fundamental conversation about where we want to push the web forward to. A year-long moratorium on new features would buy us that time.

##### Current situation

Because you’re awesome, here is an artist’s impression of the modern browser:

![Artist's impression of the modern browser and its features](https://www.quirksmode.org/blog/pix/word-features.jpg)
Browser vendors and web developers focus on features and forget about the experience.

##### Example: Navigation Transitions

To me, [Navigation Transitions](http://chrislord.net/index.php/2015/04/24/web-navigation-transitions/) exemplifies what’s wrong with new browser features today. Its purpose is to allow for a smooth transition from one web page to another, to the point of synchronising the animations on the source and destination pages.

This sounds cool, but *why* would we want to do that? We’ve done without for years. More importantly, end users have done without for years, and are quite used to a slight delay when they load another page.

Recently, web developers felt a pressing need for these transitions, and therefore adding them to browsers makes sense. But *why* do web developers want navigation transitions? In order to emulate native apps, of course. To me, that’s not good enough.

In addition, Navigation Transitions would likely need yet another polyfill, increasing our tool footprint yet again.

Interestingly, Microsoft added a [similar feature](https://msdn.microsoft.com/en-us/library/ms532847(v=vs.85).aspx#Interpage_Transition) to IE4, and deprecated it as of IE9. The idea didn’t catch on because back then nobody wanted to emulate native apps (which didn’t exist), and therefore nobody particularly cared if the users had to briefly wait for the next page.

##### Feature focus stage

Jared Spool created a [three-stage model](http://www.quora.com/Should-I-focus-on-a-good-user-experience-or-push-something-out-quickly) for software market maturity that is useful for browsers as well. (See also his [1997 article](http://www.uie.com/articles/market_maturity/).)

1. **Technology focus stage**: Users will put up with any number of UX glitches or missing features because your product performs a task that no other software performs.
2. **Feature focus stage**: Users decide which of several competing products to use based on features. Your product will be a winner if you can determine which features users really need and why.
3. **Experience focus stage**: Users can use pretty much any competing product, since they all have the same features. It’s here that the user experience becomes paramount: if your product makes it easier for your chosen audience to do their job, they’ll buy it. You can even leave out features now, as long as the functioning of your product matches what users think it should be doing and how.

Think browser for product. Think **web developers** (and not end users!) for users. What do web developers expect of a browser?

First we were in the technology focus stage, where it was a miracle that HTML, JavaScript, and, later, CSS, worked at all. We happily worked around the most terrible incompatibilities because we were so excited by the technology itself.

Once we got used to the fact that browsers actually worked we shifted our focus to features. At the time that was an excellent idea, since different browsers supported different features, and they had to be kept on the straight and narrow. Besides, we got better and better at defining which features we needed, and putting pressure on browser makers to implement those features.

The feature focus phase was very useful, don’t get me wrong. But I think it has gone on for too long. I think it’s time to shift our focus to the experience.

##### Experience focus stage

Jake Archibald [puts it best](https://twitter.com/jaffathecake/status/606780058980782080):

>  I think most people value features over experience. Hence why perf / offline / progressive enhancement is a hard sell, but push messaging = insta-hit.
> 
>  

I’m not talking about the experience of the website, but of the **website creation process**. I’m not sure if Jake is, but his remark is apt either way.

Web Components (or any other exiciting new proposal, for that matter) is a feature, and everbody loves it. Progressive enhancement is an experience, and it’s considered “unrealistic” because nobody turns off JavaScript anyway. (Lots of wrongness there, but that’s for another time.)

Currently I’m thinking that the progressive enhancement experience will never amount to much if we continue to get distracted by shiny new browser features. Drop the features, bring in the experience. (Fair’s fair, when I tweeted this thought I got [a lot of pushback](https://twitter.com/ppk/status/625233881462108160). So don’t believe it just because I say it; think about it.)

##### Moratorium

We’re pushing the web forward to emulate native more and more, but we can’t out-native native. We are weighed down by the millstone of an ever-expanding set of tools that polyfill everything we don’t understand — and that’s most of a browser’s features nowadays. This is not the future that I want to push the web forward to.

Therefore I call for a moratorium on new browser features of about a year. Let’s postpone all completely new features that as of right now don’t yet work in any browser.

Browsers are encouraged to add features that are already supported by other browsers, and to write bug fixes. In fact, finding time for these unglorious but necessary jobs would be an important advantage of the moratorium. As an added bonus it would decrease the amount of tools web developers need.

The moratorium would hit Chrome much harder than it would the other browsers, since it’s Google that is proposing most of the new features nowadays. That may not be entirely fair, but it’s an unavoidable consequence of Chrome’s current position as the top browser — not only in market share, but also in supported features. Also, the fact that Google’s documentation ranges from lousy to non-existent doesn’t help its case.

##### Stifling innovation

The main counterargument is that the moratorium would stifle web innovation. Since web innovation is currently defined as “emulating native even more,” I think a bit of stifling would actually be a good idea. Once web innovation is redefined as something that’s actually about the web we can proceed as usual.

Another counterargument is “But we need feature X!” Everybody will have a favourite upcoming feature that would be hit by the moratorium — mine is offline capabilities. But an extra round of defending the need for feature X without any reference to native apps sounds like a good idea to me.

The final counterargument is IE6. Web development was seriously hampered in the five years that Microsoft refused to upgrade its browser. New features were pushed into the other browsers, but not into IE.

Still, did this lack of new features also mean lack of innovation? Not really. As James Edwards [puts it](https://twitter.com/brothercake/status/606793616317685761):

>  Limitation spurs creativity, I remember that period quite fondly. 
> 
>  

Being forced to work around IE6’s issues required quite a bit of creativity, and although not every solution stood the test of time (most libraries and frameworks from that era, for instance), it still made the creative juices flow.

Besides, the IE6 era forced us to think about what we web developers really wanted. We had to define and defend every single feature we requested (see for instance [this old post about IE7 and JavaScript](https://www.quirksmode.org/blog/archives/2006/04/ie_7_and_javasc.html)) and set priorities.

It would be good to return to that creative mindset for a while. Not for five years — that’s clearly too long — but a break of about a year or so sounds great.

##### Stop pushing the web forward

So let’s stop pushing the web forward for a year. It’ll free us from the churn of ever more features and ever more tools.

Once we’ve spent a blessed year without any new features we’ll have a much better idea how to use the current features, and where we want the web to be pushed forward to. We might even grow out of our native obsession.
