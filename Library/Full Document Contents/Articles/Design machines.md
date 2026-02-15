# Design machines

![rw-book-cover](https://louderthanten.imgix.net/articles/dm-banner.png?auto=format&crop=focalpoint&domain=louderthanten.imgix.net&fit=crop&fp-x=0.5&fp-y=0.5&h=630&ixlib=php-3.3.1&q=82&w=1200&s=5bbed686929e2f0c16f766448e149fcb)

## Metadata
- Author: [[Travis Gertz]]
- Full Title: Design machines
- Category: #articles
- Publication date: 2015-07-10
- Document Tags: [[favorite]] 
- URL: https://louderthanten.com/coax/design-machines
- Summary: The text discusses the challenges in digital design, emphasizing the importance of considering content and collaboration in the design process. It calls for a shift towards a more holistic approach that values content development and breaks down traditional divisions within design teams. The author advocates for a change in how designers work together and approach design systems to enhance expression and creativity in digital design.

# Notes

[[Travis Gertz - Design machines]]

***

Donald trawls Google Fonts for ten minutes. There are a few candidates, but Don’s not so confident in his font evaluation skills. He lands on Open Sans. Good ol’ faithful. He sets the actionable headline text to 48pt using the light weight in white. It’s perfectly centred over the stock photo of anonymous hands fondling an electronic device. The semi-transparent overlay assures the marketing team that the headline will always be readable. Because who knows what the headline will be next month—or what stock photo will appear underneath.

Wait, we’ve seen this before. It looks hauntingly familiar.

The home page is due yesterday. The PM is lining up the next phase. The marketing team has been brainstorming headlines to A/B test. And if management can’t jack up the ROI this quarter, winter’s going to knock us back like the ice age. Let’s play it safe and go with what we know works. Something that’s easy for marketing to update next month.

*“What did AirBnb do? Can we do something like that? It really seemed to work for them.”*

There’s a problem, though. Everyone else wants to know what Airbnb did, too. The picture is coming into focus. I’m beginning to remember the nightmare. It lives:

 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%27588%27 style=%27background:%23343236%27 /%3E)
Everywhere.

Without the logos, could you tell which companies own which screenshots? Does it matter? The pattern’s become its own trademark. Just one of the popular yet mediocre ones plaguing modern screen-based design.

What’s wrong?

Everything looks the same

Why is this happening?

The debate over the reasons for digital design[1](https://louderthanten.com/coax/design-machines#fn-1) homogeneity in the last few years goes something like this... Some claim that techniques, [like responsive design](http://esbueno.noahstokes.com/post/44088237921/where-has-all-the-soul-gone), are the cause. Others blame trends like “[flat design](http://www.elischiff.com/blog/2015/4/7/fall-of-the-designer-part-i-fashionable-nonsense)” for painting the web all beige.[2](https://louderthanten.com/coax/design-machines#fn-2)

But I say horse pocky.

The problem isn’t a design trend or construction technique. I think it’s more systemic than that. It begins with…

Pressure

Design is a chaotic field to be in as the world becomes more integrated into “The Matrix.” There’s a higher demand for better-designed interfaces, but that doesn’t necessarily put designers in a more favourable position.

The products we work on are meticulously monitored, optimized, tracked, and tweaked, more than any other medium and during any other time in history. Design decisions are scrutinized by an army of managers, marketers, UX researchers, business analysts, and even clients with an arsenal of metrics and analytics. I’m not sure design has ever been evaluated and so clearly tied to the success or failure of a business.

The practice of design involves a whole new mountain of influence crunching down, strong-arming decisions, imposing standards, and questioning abilities. This weight manifests itself in a number of ways. Here’s three of them:

 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%271658%27 style=%27background:%23CCC%27 /%3E)
#### #1Metrics-obsessedpseudo-science

We now have seemingly god-like access to the ways people use our products. It’s like having an all-seeing eye gazing directly into the hearts and minds of our audience. We tool up our products with analytics and trackers, A/B test up the rectum, and scour the data for any patterns that emerge—always hunting for the skeleton key to increased engagement, conversion, and usability.

Metrics are the internet’s heroin and we’re a bunch of junkies mainlining that black tar straight into the jugular of our organizations. They make our products easy to quantify. They attach something concrete onto the inherently mushy experience visitors go through when using our digital doohickies. Anybody can read a collection of analytics and make guesses as to why the numbers look a certain way and how to improve them. These guesses are obsessive and constantly influence the words we display and design decisions we make.

> We can’t trust the data. And those who do will always be stuck chasing a robotic approach to human connection.
> 
> 

For the record, I’m a strong advocate of testing, research, and using data to learn from design—in moderation, and under no illusion of scientific integrity. Unfortunately, the majority of those designing tests, interpreting statistics, doling out design criticism, and making design revisions are not exactly scientifically literate. The tests are flawed. The environments are a colander of uncontrollable variables. The data is faulty. We’re merely substituting design assumptions with arguably more nefarious scientific assumptions.

Did user segment ‘A’ spend less time on our site because they were bored, or because they got what they needed efficiently and moved on with all of their goals met? Did button ‘A’ perform better because the text was better than button ‘B’ or was it that, in the context of the entire page design, it made a flawed page slightly more coherent? Did people click button ‘A’ more because it misled them to do something they didn’t necessarily want to do? Was segment ‘B’ visiting our site during the distraction of the World Cup? Was the weather nice? Is the real problem that the product stinks?

If a flawed sense of science is not enough to convince you, think of how often metrics are used to push an agenda or even justify a simple opinion. Numbers can be manipulated and misinterpreted. The way we set up tests can hold a strong confirmation bias… especially when we are in charge of testing our own work, setting variables, and collecting the data.

We’re great at questioning design but rarely question our science.

Trust in shaky numbers now outweighs a designer’s experience, intuition, and intelligence. Metrics influence design to the point where design becomes the long arm of the data and intelligent design thinking is second-guessed and de-prioritized.

 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%27889%27 style=%27background:%23CCC%27 /%3E)
#### #2Copycat culture

The best and worst thing about the information age is the ability and our penchant for sharing every damn thought that enters our minds. When designing, testing, and marketing our digital products, we feel compelled to blog our findings, tweet our opinions, and speak about the shit that works and doesn’t work. It doesn’t take long for opinions to morph from one organization’s experience, to industry-wide opinion, to black and white standards and best practices.

When another company achieves success, there’s a lot of pressure to investigate what they did right and apply that to our own organizations.

*Product ‘A’ started using a hamburger menu and it was a great way to implement complex navigation on mobile devices. Everybody should now use hamburger menus.*

*Product ‘B’ ran some tests and found that hamburger menus didn’t work on their app and their customers found it confusing. Nobody should ever use hamburger menus because they are a usability disaster.*

We bounce between the pillars of absolutism with little regard to nuance or context. And all of it to capture success or avoid the failure that one of our peers experienced.

This extends beyond design to the way we evaluate business and marketing.[3](https://louderthanten.com/coax/design-machines#fn-3)

Whether it’s a lack of our own critical thinking or external pressure clamping down, we shy away from carving our own path. Originality is risky. It’s difficult to quantify and defend. Why try something new when someone else has already tested it for us?

When we let the success and failure of others superficially guide design decisions, we skip over the context and uniqueness of what makes our products different. Design becomes a game of catch-up. Not an intelligent pursuit of finding unique formulas that help the organization stand out on its own.

 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%271444%27 style=%27background:%23CCC%27 /%3E)
#### #3Crap content selling crap

Content on the web is not king. Half the time it’s barely the jester. Unless created by an individual or one of the dwindling sources of legitimate journalism, it’s rarely produced for noble intentions like education or entertainment. It’s a tool with an agenda manufactured to drive business interests. Traditional blogs churn out dozens of tiny superficial nuggets per day to feed invasive advertising. News sites and digital magazines continue to slice journalism budgets and increasingly find ways to sneak advertorial[4](https://louderthanten.com/coax/design-machines#fn-4) into their patchwork templates. Businesses create blogs packed with keywords just to pump up the Google juice.

There’s a lot of writing happening, but so little of it has any substance. Superficial content is cheap. It’s outsourced to content farms and social media consultants. We hire interns, students or project managers to “maintain the blog.” Publications have reduced the number of in-house staff and rely on armies of underpaid freelancers who have no budget or support for investigation, research, or anything more than a shallow thought.

What are we putting out in the world? If design is the expression of content, and the content is worthless, what is the point of good design? Most of the shit we are compelled to put out in the world doesn’t deserve the pixels it’s rendered on… and you know what? No one seems to care. We’ll even interrupt the readers who were baited into reading crap content with a popup badgering them to sign up for more crap content to fill their inboxes so we can “increase our reach.”

We don’t actually care about content. We only care about what content can do for us. Why should anyone care about how it reads?

This pressure created by phoney metrics, fear of risk, and worthless content has squeezed expression and emotion right out of the design process. The products we work on fall under the deep scrutiny of adjacent departments, managers, and even ourselves. We’re pressure cooked to build one-size-fits-all systems that need to contain *any* type of content produced by *any* team member without further design assistance.

We design like  

machines

The work we produce is repeatable and predictable. It panders to a common denominator. We build buckets and templates to hold every kind of content, then move on to the next component of the system.

Digital design is a human assembly line.

The machines are here.  

And they want our jobs.

Photo: [Spencer Cooper](https://www.flickr.com/photos/spenceyc/7481166880) via [Flickr](https://www.flickr.com/photos/spenceyc)

While we’ve been streamlining our processes and perfecting our machine-like assembly techniques, others have been watching closely and assembling their own machines. We’ve designed ourselves right into an environment ripe for automation. Applications like Squarespace (and soon The Grid) are here and are clamouring for our jobs.

Now, these are tremendous products for individuals and small businesses. Having a website is as important for most companies as having a restroom. These services provide them that piece of the puzzle in exchange for minimal investment without interfering with their main business directives, like making punishing tacos, selling artisanal toilet paper, or brewing $6.00 coffee. Anything that makes good design accessible to the masses is good in my books.

But it would be silly for us not to acknowledge that these services will continue to affect the industry. And to a greater extent: how we communicate, read, and distribute our culture. Let me explain.

Take a minute and look at these Squarespace templates:

 ![A list of Squarespace templates](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%27951%27 style=%27background:%232d2b2c%27 /%3E)Which of these sites are just like the others?
They look familiar, don’t they? A lot like the sites I poked fun at in the intro. Globs of boring centred sans-serif type laid over darkened stock photos. Some of the sites in the intro could actually *be* Squarespace installs.

The thing is, Squarespace doesn’t care about content. Its entire business model relies on the fact that you can paste any ’ol passage of slop into their system and it will look acceptable. Squarespace is doing as good a job as many gainfully employed designers.

Just ask your former clients.

The Grid cares more about content than a lot of designers do.

Squarespace may not care about content, but The Grid does. At least the technical characteristics of that content. It recognizes people and objects in your photos, the length of your copy, your tonal preferences, and eventually, the frequency of your bowel movements. How many of us consider these things when designing our systems? What does it mean for the web design industry when this machine does a *better* job of caring for content than most designers?

To be honest, I’m not going to lose sleep over the threat of a shrinking web design industry. It could use a good pruning.

I’m much more concerned with how the increasing adoption of machines like these affect the global presentation and sharing of information. As flexible and accommodating as Squarespace is—and The Grid will allegedly be, we are increasingly surrendering our visual culture to the templates of a few corporations.

Medium is an even more insidious example. As a publishing tool, it works well. A “free” platform that lets anyone publish their thoughts in an easy interface, while earning plenty of exposure. Like Squarespace, Medium couldn’t care less about the words you write. We are publishing and sharing millions of stories through the presentational lens of a single corporation’s perspective. Unlike Squarespace, you are also bolstering Medium’s brand and advertising profits for the small price of your ideas.

Take a minute and bookmark Matthew Butterick’s excellent essay, *The billionaire’s typewriter*:

> Mr. Williams describes Medium’s key benefit as rescuing writers from the ​“terrible distraction” of formatting chores. But consider the cost. Though he’s baiting the hook with design, he’s also asking you, the writer, to let him control how you offer your work to readers. Meaning, to get the full benefit of Medium’s design, you have to let your story live on Medium, send all your readers to Medium, have your work permanently entangled with other stories on Medium, and so on — a significant concession.
> 
> 

So much of our best independent writing is contributing to a single monolithic platform under the pretence of convenience, distribution, and freedom. In reality, Medium is little more than an advertising platform that uses your content as a thinly veiled delivery mechanism for advertising and metrics gathering.

I’m not sure what the cultural impact of a single corporation homogeneously presenting such a massive chunk of our ideas for disguised profit is, but it doesn’t leave an overly inspiring outlook for online expression.

This platformization of content doesn’t just apply to individuals. Our major news organizations are beginning to submit to our Silicon Valley robot overlords.

Publications like the *New York Times* and *National Geographic* will soon be publishing directly into our social networks and devices through Facebook Instant articles and Apple News. Publications whose primary product is content, and who still don’t know how to handle digital. Now they’re letting a few big tech corps do it for them. They are giving up on the freedom and potential of an open platform, and moving into the walled garden of a few controlled native applications. All of which have already branded their content like beige curtains. And all shamelessly under the guise of convenience and performance.

This seems like a bleak time for well designed online content. And [it may well be](http://uxmag.com/articles/why-web-design-is-dead?). It’s become pretty fashionable to predict the “death of the web.” And truthfully, I don’t have a lot of hope for a richer online experience among the mainstream.

However, I’m not ready to accept this fate. Call me naive, but I refuse to sit back and let our digital culture fade to corporate beige. That’s not a digital world I want to be a part of, and I’m going to fight every step of the way to fuck up the plan.

I believe there’s an opportunity for those of us who are willing to stick our necks out. Nothing makes a drop of colour brighter than when it’s set against a wall of grey.

We have an advantage. An advantage that the machines and machine-thinkers will always be reluctant to admit:

Humans are unpredictable mushy bags of irrationality and emotion

There’s a lot of hubris hidden in the term “user experience design.” We can’t design experiences. Experiences are *reactions* to the things we design. Data lies to us. It makes us believe we know what a person is going through when they use our products. The truth is that it has no insight into physical mental or physical ability, emotional state, environmental conditions, socioeconomic status, or any other human factor outside of their ability to click on the right coloured box in the right order. Even if our machines can assume demographic traits, they will never be able to identify with each person’s unique combination of those traits. We can’t trust the data. And those who do will always be stuck chasing a robotic approach to human connection.

This is where our power lies. When we stop chasing our tails and acknowledge that we’ll never truly understand what’s going on in everyone else’s minds, we can begin to look at the web and human connection from a fresh lens. Instead of fruitlessly trying to engineer happy experiences for the everyman, we can fold ourselves into our content and listen to its heartbeat. We can let the content give design her voice.

Designing from the heart of our messages out means we fully acknowledge that they will not speak the same way to every person. We’re no longer chasing numbers. Instead, we’re thinking about how we should treat each piece of content, designing to reflect its subtle personality. The content should speak to the few people who can identify with this personality because this is the only audience that matters. No machine will ever A/B test its way to a more meaningful relationship.

Ask yourself, who would you rather connect with: People who try to please you? Or people who stand by their own unique personality? Who do you respect more? Our content and our products deserve the same respect.

We need to be better than the machines. It’s time to step up and design with heart.

So how do we do it? Where can we look for inspiration and guidance? The history of graphic design is full of emotion and it’s got a mighty heart… but we’ve rarely captured it online. I believe we can find the answers in something dear to my heart: editorial design.

#### Obsessed with the glossies

I fell in love with the magazine in grade seven. I’d spend hours poring through publications every day. I’d read every article, study every sequence shot, and gaze through full-page spreads in titles like *Motocross Action*, *Snowboarder*, *Thrasher*, *Big Brother*, *Modern Drummer*, and *Adbusters*. Magazines were the obsession that drove all my other obsessions. After high school, there was nothing I wanted more than to run my own publication, so I went to school for it. Now I design for the web using what I learned about glossies.

I’m obsessed with the transformative power of magazines. They have the ability to fuel us, change our ideas, and enrich our interests in everything from blood sports to politics. It’s an old medium with familiar constraints that have shaped the rules and standards that define it, yet still manages to stay fresh, explosive, and relevant. Editorial design is unparalleled in expression, and we’ve been trying to copy it on the screen since the invention of Flash and CSS… with very little success. Then we gave up.

It’s easy to apply the visual techniques of editorial design to the screen, but to simply translate them across mediums is a fool’s errand. Doing so ignores the root causes of *why* magazine aesthetics were formed in the first place. It disregards how much they were shaped by editorial history, production methods, and physical properties. Editorial design is really about *connecting* the content to the medium.

We’ve never been good at connecting content to our medium. Hell, we barely understood the medium of the web.

That’s changing now. Our tools are more mature. We’re comfortable with the quirks of screen-based design. We’re beginning to understand how to work with the flexible nature of the web’s grain.[5](https://louderthanten.com/coax/design-machines#fn-5) Let’s look at what makes editorial design so emotive and how we’ve failed to adapt despite designing for a platform that holds so much potential for expression. It’s time to start asking *“how do magazines achieve rich reactions and connections, and how do we translate those approaches to the screen?*”

I want to talk about three areas where editorial design diverges from digital design:

1. How we design systems
2. How we treat content
3. How we collaborate

 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%27360%27 style=%27background:%23CCC%27 /%3E)
Design systems still feel like a novelty in screen-based design. We nerd out over grid systems and modular scales and obsess over style guides and pattern libraries. We’re pretty good at using them to build repeatable components and site-wide standards, but that’s sort of where it ends.

There’s still an imperative to design for maximum template efficiency. Templates are built to house the content of several pages, changing only when we need a special behaviour or interaction. Our prototypes and wireframes are designed to promote automation, repetition, and consistency.

But to stop there is to ignore the true purpose and potential of a design system. If we’re guilty of anything in digital design, it’s our short memories. We like to borrow concepts from other design disciplines, then forget where we got them and why they were invented in the first place.

Let’s look at an editorial design system. Here are the wireframes for the (now-defunct) Transworld Surf 2011 redesign:

 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%271046%27 style=%27background:%23d7d6d6%27 /%3E)
 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%271046%27 style=%27background:%23c5c5c7%27 /%3E)
 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%271010%27 style=%27background:%23dedcd9%27 /%3E)
 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%27909%27 style=%27background:%23d4d5d4%27 /%3E) Design by , [Wedge & Lever](https://www.behance.net/gallery/transworld-surf-redesign/13052023)

Look familiar? Wireframes and prototypes are used for designing magazines too—but there’s an important difference. Unlike the web, editorial systems are designed for variation, not prescription. They are a starting point, not a final deliverable.

The wireframe boxes are flexible enough to accommodate variation. There’s no labelling. No prescriptive text. No explicit details. There’s room for interpretation in each section of the layout.

Also, notice the breadth of page modules. Each article can use a number of different layouts depending on the type of content it contains. There’s no single wireframe called “article.” If there’s no template that adequately fits the content, we have the flexibility to make one on the fly. The system allows for it. Magazine designers use the wireframe to rapidly get to an acceptable standard, but the bulk of design work happens on top of that.

On the web, once we reach the system level, we freeze. We’ve got our system. We stop worrying about content and design and call it a day. In editorial, the system is where design begins.

#### #2How we treat content

Well intentioned web designers advocate for the idea of content-first design, but here’s the big dirty secret:

*Content-first is actually no better than content-last.*

We design with content-first design so we know how big to make the boxes. But we all know that the content is going to change anyway. Most systems have to accommodate those changes long after the designer’s done, because, let’s face it, it’ll probably be two years before another person touches it again. Modern CMSs and responsive design have made our systems flexible enough that the boxes will adapt. Squarespace doesn’t complain about getting your content early or late. It just makes it fit.

Editorial designers know that the secret isn’t content first or content last… it’s content and design at the same time.

> Design is at the service of content, and the designer, as a facilitator of communication, collaborates with the editor to create a product that may be used in a changing media society. In it design becomes planning and storytelling, the media become distribution and conversation, authors become value generators and a reason for connecting people.
> 
> 

Instead of ‘*how big do we make the boxes,*’ we could be asking… *‘do we even need boxes?’*

When we design with content the way we should, design augments the message of the content.

 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%272156%27 style=%27background:%23888889%27 /%3E)
 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%271065%27 style=%27background:%23d17277%27 /%3E)
 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%272157%27 style=%27background:%23d5d8da%27 /%3E)
 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%272092%27 style=%27background:%23c2bbba%27 /%3E)
 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%272078%27 style=%27background:%23321c1a%27 /%3E)
 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%271057%27 style=%27background:%237f282a%27 /%3E)
 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%271045%27 style=%27background:%232d8b69%27 /%3E)
 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%272202%27 style=%27background:%23282026%27 /%3E)
[Offset](http://offsetartsjournal.com/)  
[Esquire](http://www.esquire.com/)  
[New York](https://nymag.com/)  
[Bloomberg Businessweek](https://www.bloomberg.com/)  
[Adbusters](https://www.adbusters.org/)  
[Runner’s World](http://www.runnersworld.com/)  
[Eureka](https://www.flickr.com/photos/timeseureka/)  
[Colors](http://www.colorsmagazine.com/)  
[The Economist](https://www.economist.com/)

These layouts show engaging, powerful messages that internalize without even reading the piece. The art direction isn’t a substitution for the content, but it begs us to dive in and get dirty. The cover gives us a hint about what’s inside, and teases out our emotion, begging us to read the rest.

None of these concepts would exist if designed by a content-first or content-last approach. It’s not enough. This level of conceptual interpretation requires a deep understanding of and connection to the content. A level we best achieve when we work with our editors and content creators *as* we design. This requirement doesn’t just apply to text. Notice how every single photo and illustration intertwines with the writing. There are no unmodified stock photos and no generic shots that could be casually slipped into other stories. Every element has a purpose and a place. A destiny in the piece it sits inside of.

We should treat content development with the same respect that we have for design and engineering. Stop putting the responsibility of writing solely in the hands of the intern, social marketing expert, or worst of all, the client. Your clients sure as shit don’t want to write content—they just don’t want to think they’re paying extra for it. They don’t understand that it’s just another step in the process. Take that responsibility off their plates and weave content into your process. Incorporate it into the budget and hire someone who is qualified to write it, just as you would hire a designer or developer.

Which brings us to…

#### #3How we collaborate

Years of awkward tools and processes, limited technology, and tool and technique-driven education has chiselled a unique division of labour around digital design teams. As web applications became more sophisticated and interactive, we quickly learned the weaknesses of purely aesthetics-driven design. We began to focus on “user experience design” to make up for poorly thought-out superficial design processes. But instead of empowering our designers, we divided them into a number of sub-disciplines. Here’s a gross simplification of one of the common arrangements we see:

##### UX Design

*“How it works”*

* Layout
* Interaction
* Information architecture
* Wireframing
* Prototyping
* Research
* Testing

*“How it looks”*

* Typography
* Colour
* Form
* Illustration
* Photography
* Detail

In our digital world, too many teams assign the responsibilities of things like wireframing, prototyping, user flows, research, testing, and content strategy under the umbrella of UX design. Then we push the responsibilities of typography, colour, form, illustration, photography, and visual details to a separate visual or UI designer… as if those factors don’t affect the visitor’s experience. But this line is arbitrary and has led us to divorce expression from interactivity and layout. Choosing readable or expressive typefaces and proper use of white space is just as important as user flow. In fact, it’s a vital part of it.

Editorial Designers know the design stack from a system-level and how to flow it all the way up to the individual expression of each piece of content. Instead of breaking up roles by stages of a project (wireframes, prototypes, branding, typography), they break up more vertically (global system, issue, features).

There’s also more respect for content. Editors work directly with designers to make sure the piece conveys and expresses the right messages in the most effective ways possible. They don’t just ship copy down the assembly line and let the designers have at ‘er. It’s a relationship that requires collaboration and deep understanding of each other’s roles.

> … an editorial designer should take as much interest in the content of a publication as the editor, because designing a magazine is unquestionably an extension of editing it. Both roles are creative ones that are rooted in and play part of a creative process, and how they function together will nearly always determine the success or failure of an editorial publication.
> 
> 

Content drives design and design augments the content. Editors and writers help with strategy, choosing graphical media that works with the content. They also get a say on how people should interpret it.

A few years ago, Bloomberg Businessweek filmed a wonderful series of short discussions between then, creative director, Richard Turley, and editor Josh Tyrangiel, about the concept behind each week’s cover design. Watch how their collaborative approach unveils how design and content work together (and often fight together) around an editorial piece.

Bloomberg Businessweek editor, Josh Tyrangiel, and creative director, Richard Turley, on their collaboration for the ​“Bang Head Here” cover.

 ![](data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 width=%271568%27 height=%272098%27 style=%27background:%23f79409%27 /%3E)
One last thing to point out: I’m suspicious of putting designers in charge of testing and evaluating their own work. Ideally, testing should probably be conducted by people who are actually trained in statistics, gathering data, conducting research, and interpreting numbers. The collaboration between them is the stuff that sticks.

Researchers understand the scientific method and the possible shortcomings of their data. It’s up to editors and designers to interpret these findings and apply them in ways they see fit, using the light of their own experience and intuition to guide them. This removes some of that confirmation bias and puts the power of creativity back into designers’ and editors’ hands.

Designing better systems and treating our content with respect are two wonderful ideals to strive for, but they can’t happen without institutional change. If we want to design with more expression and variation, we need to change how we work together, build design teams, and forge our tools.

Let’s destroy the arbitrary division between UX and visual design, let’s pull research away from our own reality distortion fields, and let’s give our content more responsibility.

#### The machines are here

We’ve designed ourselves into a corner. As the machines begin to squirm in through new entry points, they’ll shrink that corner and make it harder for us to breathe. They may even replace most of us and do our jobs without any feeling at all.

But we still have a chance. As long as we run brave organizations made up of even braver souls who are willing to embrace expression, trust their intuition and experiences, and stand up when everyone else is sitting down, we will survive. The machines will get bored and probably even forget about our corner.

So yes. We are going to fight the machines and we might even lose. But regardless, we’re going to use these gooey bags of flesh, blood, and brains to do it.

How will you prove you’re better than a machine?

#### Good resources

1. I use the terms digital, web, product, UI, and UX design throughout the piece. In most cases, I’m talking about any form of design for a screen. [↩](https://louderthanten.com/coax/design-machines#fnref:1)
2. The vast majority of graphic design through history has been flat, save a few decades in the relatively narrow field of user interface design. Flat design doesn't cause a lack of expression or originality, but it does illustrate the insulated bubble we live in on the web and in our products. [↩](https://louderthanten.com/coax/design-machines#fnref:2)
3. [Advertorial](https://en.wikipedia.org/wiki/Advertorial) is advertising disguised as editorial content. It's gross. [↩](https://louderthanten.com/coax/design-machines#fnref:4)
4. Read Frank Chimero’s essay, [The Web’s Grain](http://www.frankchimero.com/writing/the-webs-grain/) for a smart examination of our medium’s unique characteristics and some fresh ways to approach it. [↩](https://louderthanten.com/coax/design-machines#fnref:5)

Travis is a co-founder and Director of Operations & Design at Louder Than Ten. He wants digital workers to be as passionate about workplace democracy as they are about design systems and development frameworks.
