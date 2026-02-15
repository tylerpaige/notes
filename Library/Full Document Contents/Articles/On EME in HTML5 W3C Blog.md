# On EME in HTML5 | W3C Blog

![rw-book-cover](https://www.w3.org/favicon.ico)

## Metadata
- Author: [[Tim Berners-Lee]]
- Full Title: On EME in HTML5 | W3C Blog
- Category: #articles
- Publication date: 2017-02-28
- URL: https://www.w3.org/blog/2017/02/on-eme-in-html5/
- Summary: https://www.w3.org/blog/2017/02/on-eme-in-html5/

# Notes

[[Tim Berners-Lee - On EME in HTML5 | W3C Blog]]

***

The question which has been debated around the net is whether W3C should endorse the Encrypted Media Extensions (EME) standard which allows a web page to include encrypted content, by connecting an existing underlying Digital Rights Management (DRM) system in the underlying platform. Some people have protested “no”, but in fact I decided the actual logical answer is “yes”. As many people have been so fervent in their demonstrations, I feel I owe it to them to explain the logic. My hope is, as there are many things which need to be protested and investigated and followed up in this world, that the energy which has been expended on protesting EME can be re-channeled other things which really need it. Of the things they have argued along the way there have also been many things I have agreed with. And to understand the disagreement we need to focus the actual question, whether W3C should recommend EME.

The reason for recommending EME is that by doing so, we lead the industry who developed it in the first place to form a simple, easy to use way of putting encrypted content online, so that there will be interoperability between browsers. This makes it easier for web developers and also for users. People like to watch Netflix (to pick one example). People spend a lot of time on the web, they like to be able to embed Netflix content in their own web pages, they like to be able to link to it. They like to be able to have discussions where they express what they think about the content where their comments and the content can all be linked to.

Could they put the content on the web without DRM? Well, yes, for a huge amount of video content is on the web without DRM. It is only the big expensive movies where to put content on the web unencrypted makes it too easy for people to copy it, and in reality the utopian world of people voluntarily paying full price for content does not work. (Others argue that the whole copyright system should be dismantled, and they can do that in the legislatures and campaign to change the treaties, which will be a long struggle, and meanwhile we do have copyright).

#### Given DRM is a thing,…

When a company decides to distribute content they want to protect, they have many choices. This is important to remember.

If W3C did not recommend EME then the browser vendors would just make it outside W3C. If EME did not exist, vendors could just create new Javascript based versions. And without using the web at all, it is so easy to invite ones viewers to switching to view the content on a proprietary app. And if the closed platforms prohibited DRM in apps, then the large content providers would simply distribute their own set-top boxes and game consoles as the only way to watch their stuff.

If the Director Of The Consortium made a Decree that there would be No More DRM in fact nothing would change. Because W3C does not have any power to forbid anything. W3C is not the US Congress, or WIPO, or a court. It would perhaps have shortened the debate. But we would have been distracted from important things which need thought and action on other issues.

Well, could W3C make a stand and just because DRM is a bad thing for users, could just refuse to work on DRM and push back wherever they could on it? Well, that would again not have any effect, because the W3C is not a court or an enforcement agency. W3C is a place for people to talk, and forge consensus over great new technology for the web. Yes, there is an argument made that in any case, W3C should just stand up against DRM, but we, like Canute, understand our power is limited.

But importantly, there are reasons why pushing people away from web is a bad idea: It is better for users for the DRM to be done through EME than other ways.

1. When the content is in a web page, it is part of the web.
2. The EME system can ‘sandbox’ the DRM code to limit the damage it can do to the user’s system
3. The EME system can ‘sandbox’ the DRM code to limit the damage it can do to the user’s privacy.

[![](https://www.w3.org/blog/wp-content/uploads/2017/02/provider-options3.png)](https://www.w3.org/blog/wp-content/uploads/2017/02/provider-options3.png)
As mentioned above, when a provider distributes a movie, they have a lot of options. They have different advantages and disadvantages. An important issue here is how much the publisher gets to learn about the user.

* If they sell a DVD or Blu-ray disk, they never get to know whether the user watches it. From the user’s point of view they can watch each bit of it as many times as they like without the feeling they are being watched.
* If they put it on the web using EME, they will get to record that the user unlocked the movie. The browser though, in the EME system, can limit the amount of access the DRM code has, and can prevent it “phoning home” with more details. (The web page may also monitor and report on the user, but that can be detected and monitored as that code is not part of the “DRM blob”)
* If they put it on an app in a closed system like an iPhone, then they get to make whatever DRM they like. They also get to watch exactly how and where the user watches which bits of the movie. If they can persuade the user to allow them other access, such to the user’s calendar, they can completely profile the user, and correlate this with their movie-watching habits.
* If they distribute it using an app on an open system like Android or Mac OS X, then they can get the same feedback as on an iPhone app. However as the OS is not a locked-down system, the app may be able to further abuse the user, by possibly exfiltrating further information, and also like, in the[Sony Rootkit](https://en.wikipedia.org/wiki/Sony_BMG_copy_protection_rootkit_scandal) case, installing spyware on the system.
* If they distribute it with their own closed system, like a game console or a set-top box, then the user is protected from spying on their computer. The publisher has complete control of information which is sent back about the user’s play and pause, and so on. The user has no way though to have this as part of their connected web life. There are no links in or out.

So in summary, **it is important to support EME** as providing a relatively safe online environment in which to watch a movie, as well as the most convenient, and one which makes it a part of the interconnected discourse of humanity.

I should mention that the extent to which the sandboxing of the DRM code protects the user is not defined by the EME spec at all, although current implementations in at least Firefox and Chrome do sandbox the DRM.

##### Spread to other media

Do we worry that having put movies on the web, then content providers will want to switch also to use it for other media such as music and books? For music, I don’t think so, because we have seen industry move consciously from a DRM-based model to an unencrypted model, where often the buyer’s email address may be put in a watermark, but there is no DRM.

For books, yes this could be a problem, because there have been a large number of closed non-web devices which people are used to, and for which the publishers are used to using DRM. For many the physical devices have been replaced by apps, including DRM, on general purpose devices like closed phones or open computers. We can hope that the industry, in moving to a web model, will also give up DRM, but it isn’t clear.

We have talked about the advantages of different ways of using DRM in distributing movies. Now let us discuss some of the problems with DRM systems in general.

#### Problems with DRM

Much of this blog post is W3C’s technical perspective on EME which I provide wearing my Director’s hat – but in the following about DRM and the DMCA, that (since this is a policy issue), I am expressing my personal opinions.

##### Problems for users

There are many issues with DRM, from the user’s point of view. These have been much documented elsewhere. Here let me list these:

* Fair use of the material is not possible, such as excepting for commentary, educational purposes, and so on
* This prevents remixing into derivative works
* The user cannot take a backup copy
* Having a DRM blob in one’s computer is a security threat, in that it could attack the machine

DRM systems are generally frustrating for users. Some of this can be compounded by things like attempts to region-code a licence so the user can only access when they are in a particular country, confusion between “buying” and “renting” something for a fixed term, and issues when content suppliers cease to exist, and all “bought” things become inaccessible.

Despite these issues, users continue to buy DRM-protected content.

##### Problems for developers

DRM prevents independent developers from building different playback systems that interact with the video stream, for example, to add accessibility features, such as speeding up or slowing down playback.

##### Problems for Posterity

There is a possibility that we end up in decades time with no usable record of these movies, because either their are still encrypted, or because people didn’t bother taking copies of them at the time because the copies would have been useless to them. One of my favorite suggestions is that anyone copyrighting a movie and distributing it encrypted in any way MUST deposit an unencrypted copy with a set of copyright libraries which would include the British Library, the Library of Congress, and the Internet Archive.

#### Problems with Laws

Much of the push back against EME has been based on push back against DRM which has been based on specific important problems with certain laws.

The law most discussed is the US [Digital Millennium Copyright Act](https://en.wikipedia.org/wiki/Digital_Millennium_Copyright_Act) (DMCA). Other laws exist in other countries which to a greater or lesser extent resemble the DMCA. Some of these have been brought up in the discussions, but we do not have an exhaustive list or analysis of them. It is worth noting that US has spent a lot of energy using the various bilateral and multilateral agreements to persuade other countries into adopting laws like the DMCA. I do not go into the laws in other countries here. I do point out though that this cannot be dismissed as a USA-only problem. That said, let us go into the DMCA in more detail.

Whatever else you would like to change about the Copyright system as a whole, there are particular parts of the DMCA, specifically section 1201, which put innocent security researchers at risk of dire punishment if they are deemed to have thrown light on any DRM system.

There was an attempt at one point in the W3C process to refuse to bring the EME spec forward until all the working group participants would agree to indemnify security researchers under this section. To cut a very long story short, the attempt failed, and historians may point to the lack of leverage the EME spec had to be used in this way, and the difference between the set of companies in the working group and the set of companies which would be likely to sue over the DMCA, among other reasons.

###### Security researchers

There is currently (2017-02) a related effort at W3C to encourage companies to set up ‘bug bounty” programs to the extent that at least they guarantee immunity from prosecution to security researchers who find and report bugs in their systems. While W3C can encourage this, it can only provide guidelines, and cannot change the law. I encourage those who think this is important to help find a common set of best practice guidelines which companies will agree to. A [first draft](https://w3c.github.io/security-disclosure/) of some guidelines was [announced](https://lists.w3.org/Archives/Public/public-html-media/2017Feb/0003.html). Please help make them effective and acceptable and get your company to adopt them.

Obviously a more logical thing would be to change the law, but the technical community seems to have become resigned to not being able to positive effect on the US legislative system due to well documented problems with that system.

This is something where public pressure could perhaps be beneficial, on the companies to agree on and adopt protection, not to mention changing the root cause in the DMCA. W3C would like to hear, by the way of any examples of security researchers having this sort of problem, so that we can all follow this.

#### The future web

The web has to be universal, to function at all. It has to be capable of holding crazy ideas of the moment, but also the well polished ideas of the century. It must be able to handle any language and culture. It must be able to include information of all types, and media of many genres. Included in that universality is that it must be able to support free stuff and for-pay stuff, as they are all part of this world. This means that it is good for the web to be able to include movies, and so for that, it is better for HTML5 to have EME than to not have it.

TimBL
