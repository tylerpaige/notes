---
Author: Adrian Holovaty
Full Title: A fundamental way newspaper sites need to change | Holovaty.com
Category: #articles
Publication date: 2006-09-06
URL: http://www.holovaty.com/writing/fundamental-change/
Summary: Newspaper websites need to change how they present information to remain relevant. Traditional story-centric approaches may not effectively convey structured data that can be repurposed. Shifting towards more machine-readable data formats can enhance information dissemination and reader engagement.
---

# Notes

[[Adrian Holovaty - A fundamental way newspaper sites need to change | Holovaty.com]]

***

A blog entry titled [9 Ways for Newspapers to Improve Their Websites](http://www.bivingsreport.com/2006/9-ways-for-newspapers-to-improve-their-websites) has been making the rounds lately. I don’t write about the online news industry on this site as much as I [used to](http://www.holovaty.com/blog/archive/), but this article inspired me to collect my current thinking on what newspaper sites need to do. Here, I present my opinion of one fundamental change that needs to happen.

For background: I have a journalism degree, for what it’s worth, and I’ve worked for newspaper Web sites since 1998 (including the college paper and internships). The sites: [themaneater.com](http://www.themaneater.com/) (now a pale shadow of its former self) at the University of Missouri, [SuburbanChicagoNews.com](http://www.suburbanchicagonews.com/), [ajc.com](http://www.ajc.com/) in Atlanta, [LJWorld.com](http://www.ljworld.com/) / [Lawrence.com](http://www.lawrence.com/) in Lawrence, Kansas, and, for the last year, [washingtonpost.com](http://www.washingtonpost.com/).

Most of the points made in the “9 Ways” entry are OK, if a little overly specific (“make your content work on cell phones and PDAs”) and trendy (tagging!). There’s nothing wrong with that — the online newspaper industry needs all the advice it can get. But more fundamental shifts need to happen for newspaper companies to remain essential sources of information for their communities.

One of those important shifts is: **Newspapers need to stop the story-centric worldview**.

Conditioned by decades of an established style of journalism, newspaper journalists tend to see their primary role thusly:

1. Collect information
2. Write a newspaper story

The problem here is that, for many types of news and information, newspaper stories don’t cut it anymore.

So much of what local journalists collect day-to-day is *structured information*: the type of information that can be sliced-and-diced, in an automated fashion, by computers. Yet the information gets distilled into a big blob of text — a newspaper story — that has no chance of being repurposed.

“Repurposed”?

Let me clarify. I don’t mean “Display a newspaper story on a cell phone.” I don’t mean “Display a newspaper story in RSS.” I don’t mean “Display a newspaper story on my PDA.” Those are fine goals, but they’re examples of changing the *format*, not the information itself. Repurposing and aggregating information is a different story, and it requires the information to be stored atomically — and in machine-readable format.

For example, say a newspaper has written a story about a local fire. Being able to read that story on a cell phone is fine and dandy. Hooray, technology! But what I *really* want to be able to do is explore the raw facts of that story, one by one, with layers of attribution, and an infrastructure for comparing the details of the fire — date, time, place, victims, fire station number, distance from fire department, names and years experience of firemen on the scene, time it took for firemen to arrive — with the details of previous fires. And subsequent fires, whenever they happen.

That’s what I mean by structured data: information with attributes that are consistent across a domain. Every fire has those attributes, just as every [reported crime](http://www.chicagocrime.org/) has many attributes, just as every [college basketball game](http://www2.kusports.com/stats/) has many attributes.

Those three examples are *obvious* candidates for structure, mostly due to ubiquity. People have been slicing and dicing sports stats for years. People have been analyzing crime for years.

But it doesn’t stop at those obvious examples. If you take some time to examine what sort of information newspaper journalists collect, the amount of structure will jump at you. If I may take the liberty of giving examples from Web sites I’ve worked for:

* An obituary is about a [person](http://www2.ljworld.com/obits/2006/sep/03/gus_neitzel/), involves [dates](http://www2.ljworld.com/obits/2005/oct/) and [funeral homes](http://www2.ljworld.com/obits/funeral_homes/warrenmc_elwain_mortuary/).
* A wedding announcement is about a [couple](http://www2.ljworld.com/couples/2006/jul/01/), with a wedding date, engagement date, bride hometown, groom hometown and various other happy, flowery pieces of information.
* A [birth](http://www2.ljworld.com/births/) has parents, a child (or children) and a date.
* A [college graduate](http://www2.ljworld.com/kugraduates/2005/spring/) has a [home state](http://www2.ljworld.com/kugraduates/2005/spring/states/il/), a [home town](http://www2.ljworld.com/kugraduates/2005/spring/il/chicago/), a [degree](http://www2.ljworld.com/kugraduates/2005/spring/degrees/bachelor-of-science-in-journalism/), a [major](http://www2.ljworld.com/kugraduates/2005/spring/majors/history/) and graduation year.
* An [Onion](http://www.theonion.com/)-style [“On the Street”](http://www2.ljworld.com/onthestreet/) feature has [respondents](http://www2.ljworld.com/onthestreet/2006/sep/04/opinion_of_lawrence_tap_water/), answers and a publication date.
* A drink special has a [day of the week](http://www.lawrence.com/drinkspecials/) and is offered at a [bar](http://www.lawrence.com/places/the_bottleneck/).
* The schedule of the U.S. Congress has a [day](http://projects.washingtonpost.com/congress/activity/2006/sep/06/) and multiple agenda items.
* A [political advertisement](http://projects.washingtonpost.com/politicalads/) has a [candidate](http://projects.washingtonpost.com/politicalads/candidates/mike-mcgavick/), a [state](http://projects.washingtonpost.com/politicalads/states/ct/), a [political party](http://projects.washingtonpost.com/politicalads/parties/r/), multiple [issues](http://projects.washingtonpost.com/politicalads/issues/crime/), [characters](http://projects.washingtonpost.com/politicalads/characters/blue-collar-workers/), [cues](http://projects.washingtonpost.com/politicalads/cues/september-11/), [music](http://projects.washingtonpost.com/politicalads/music/ominous/) and more.
* [Every Senate, House and Governor race](http://projects.washingtonpost.com/elections/keyraces/map/) in the U.S. has location, [analysis](http://projects.washingtonpost.com/elections/keyraces/34/), [demographic information](http://projects.washingtonpost.com/elections/keyraces/census/il/), previous election results, [campaign-finance information](http://projects.washingtonpost.com/elections/keyraces/funding/n00027968/) and more.
* [Every known detainee at Guantanamo Bay](http://projects.washingtonpost.com/guantanamo/) has an [approximate age](http://projects.washingtonpost.com/guantanamo/by-age/), birthplace, [formal charges](http://projects.washingtonpost.com/guantanamo/charged/) and more.

See the theme here? A lot of the information that newspaper organizations collect is relentlessly structured. It just takes somebody to realize the structure (the easy part), and it just takes somebody to start storing it in a structured format (the hard part).

Now, I can understand why newspapers are slow to accept this sort of thinking. Journalists aren’t the most tech-savvy bunch, they’re not the most innovative bunch, and they’re (just a *tad*) [resistant to change](http://www.deltadynamicsinc.com/images/stubborn%20mule.gif).

One barrier to this thinking is a sort of journalistic arrogance: “How is this journalism? We’re journalists, and we’ve been trained to explain complex information to people in ways that they can understand. Displaying raw data does not help people; writing a news article *does* help people, because it’s plain English.” I’ve presented these concepts (“journalism via computer programming,” the importance of machine-readable data, etc.) at a number of journalism-industry events, and inevitably somebody asks these questions.

Well, I have a couple of answers.

First, the question of “How is this journalism?” is academic. Journalists should have less of a concern of what is and isn’t “journalism,” and more of a concern for *important, focused information that is useful to people’s lives and helps them understand the world*. A newspaper ought to be that: a fair look at current, important information for a readership.

Second, it’s important to note I’m not making an all-or-nothing proposition; I’m not saying newspapers should turn completely to vast collections of data, completely abandoning the format of a news article. News articles are *great* for telling stories, analyzing complex issues and all sorts of other things. An article — a “big blob of text” — is often the best way to explain concepts. The nuances of the English language do not map neatly to machine-manipulatable data sources. (This very entry, which you’re reading right now, is a prime example of something that could not be replaced with a database.) When I say “newspapers need to stop the story-centric worldview,” I don’t mean “newspapers need to abolish stories.” The two forms of information dissemination can coexist and complement each other.

But beyond the journalistic arrogance, another problem is that newspaper companies’ current software and organizational setup overwhelmingly discourages any sort of “information special-casing.” Just about every newspaper Web site content-management system I’ve ever seen is unabashedly story-centric. Want to post event calendar information into your news-site CMS? Post it as a “news article” object. Want to publish listings of recent crimes in your town? It goes in as a “news article.” There’s not much Joe Reporter, or even Jane Online Editor, can do about this, because Oh We’ve Invested So Much Into This CMS, and/or Our Newspaper Web Site Doesn’t Employ Any Computer Programmers. (The latter of which makes as much sense as a film director refusing to employ cameramen or video editors.)

When I worked at [LJWorld.com](http://www.ljworld.com/), we wrote [a CMS](http://www.ellingtoncms.com/) from the ground up to be able to handle all these distinct types of information. (And we created the Web framework [Django](http://www.djangoproject.com/) to let us handle *new* types of information rapidly.) But, before that, we had an old CMS, and our night producers, whose job it was to copy-and-paste everything from the print newspaper into the Web system, would routinely publish all the newspaper’s little special features as “news articles.” The “photo of the day” newspaper feature would get posted as a story with no text — just a photo. The [Onion](http://www.theonion.com/)-style [“On the Street”](http://www2.ljworld.com/onthestreet/) feature would get posted as a “news article” object containing a question, four photos, and four responses. Each recurring newspaper feature was posted as a “news article,” regardless of whether it actually *was* a news article — simply because that’s all the content-management system knew how to do.

This is a subtle problem, and therein lies the rub. In my experience, when I’ve tried to explain the error of storing *everything* as a news article, journalists don’t immediately understand why it is bad. To them, a publishing system is just a means to an end: getting information out to the public. They want it to be as fast and streamlined as possible to take information batch *X* and put it on Web site *Y*. The goal isn’t to have clean data — it’s to publish data quickly, with bonus points for a nice user interface.

But the goal for me, a data person focused more on the long term, is to store information in the most valuable format possible. The problem is particularly frustrating to explain because it’s not necessarily obvious; if you store everything on your Web site as a news article, the Web site is not necessarily hard to use. Rather, it’s a problem of *lost opportunity*. If all of your information is stored in the same “news article” bucket, you can’t easily pull out just the crimes and plot them on a map of the city. You can’t easily grab the events to create an event calendar. You end up settling on the least common denominator: a Web site that knows how to display one type of content, a big blob of text. That Web site cannot do the cool things that readers are beginning to expect.

Then there’s the serendipity advantage. When I worked for LJWorld.com, we worked with the local weathermen to create [a weather site](http://www.holovaty.com/blog/archive/2003/07/13/1623/) that displayed the weathermen’s forecast for the next few days. I made them a Web interface that let them enter the predicted high temperature, low temperature and sky conditions — all in separate database fields. There really wasn’t any reason to use separate fields for these values other than the fact that the [site’s](http://www.6newslawrence.com/weather/) design called for presenting the temperatures in a different color than the conditions, and we didn’t want the weathermen to have to remember to insert the HTML coloring codes in the right place. But it wasn’t until several months later that we reaped some *real* benefits of databasing the information, when we were putting together [Game](http://www2.ljworld.com/news/sports/game/), an exhaustive database of local little-league teams and games. (Yes, you read that right.) We created a page for every [little-league team](http://www2.ljworld.com/game/baseball/2006/leagues/dcaba_10/falcons/) and every [little-league game](http://www2.ljworld.com/game/2006/games/3095/), and when it came time to create the game pages, one of us said, “You know, these games tend to rain out a lot. It’d be really cool if we could somehow display the weather forecast for each game.” And, boom! One of us realized that we already *had* weather forecast data, in nice, sliceable-and-diceable format, thanks to our database populated by the weathermen. Ten minutes later, our little-league pages displayed weather forecasts. Serendipity.

Finally, I’ll note that some newspaper Web sites are actively looking to improve their understanding of these issues, and a number of Web editors have contacted me to ask if I know of anybody — please, Adrian, anybody! — who has tech skills and would be interested in applying his/her knowledge to a newspaper Web site. If you have these skills, please [contact me](http://www.holovaty.com/contact/), and I’ll put you in touch. The industry needs you.

**UPDATE, several years later:** This essay [inspired the creation of](http://www.mattwaite.com/posts/2007/aug/22/announcing-politifact/) the fantastic [PolitiFact](http://www.politifact.com/), which [won the Pulitzer Prize in 2009](http://www.politifact.com/truth-o-meter/article/2009/apr/20/politifact-wins-pulitzer/) and is a great example of treating news data with respect.
