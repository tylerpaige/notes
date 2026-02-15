# What should follow the web? – Mike’s blog

![rw-book-cover](https://miro.medium.com/v2/resize:fit:1200/1*mdqGiMeX8-LDOHihp44QUQ.png)

## Metadata
- Author: [[Mike Hearn]]
- Full Title: What should follow the web? – Mike’s blog
- Category: #articles
- Publication date: 2018-06-19
- URL: https://blog.plan99.net/what-should-follow-the-web-8dcbbeaccd93
- Summary: The article discusses the need to replace the web due to its poor productivity and unfixable security problems. The author proposes a competitor to the web called NewWeb, which would require a clear design philosophy and a unified data representation. The article suggests the use of an app browser for deployment and sandboxing benefits, and a JVM core for the NewWeb design. The author also emphasizes the need for a shallow learning curve, simplicity, and a design that discourages developers from building UI that is too complicated.

# Notes

[[Mike Hearn - What should follow the web? – Mike’s blog]]

***

In [part 1 of this series](https://blog.plan99.net/its-time-to-kill-the-web-974a9fe80c89) I argued that it’s time to start thinking about how to replace the web. The justifications are poor productivity and unfixable security problems.

Some people felt that what I wrote was too negative, and didn’t recognise the web’s good points. That’s right: part one was “let’s discuss the idea that we’ve dug ourselves a big hole”, part two is “how do we design something better?” This is a huge topic so it will actually take more than two parts.

Let’s call our competitor to the web, NewWeb (er, branding can come later). To start we must understand why the web became successful in the first place. The web out-competed other ways of writing apps that had far better tools for building graphical applications, so clearly it had some qualities that overwhelmed its flaws. If we can’t match those we’re doomed.

Some content could not be imported from the original document. [View content ↗](https://cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2F8CI718A8OUI%3Fstart%3D180%26feature%3Doembed%26start%3D180&url=http%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3D8CI718A8OUI&image=https%3A%2F%2Fi.ytimg.com%2Fvi%2F8CI718A8OUI%2Fhqdefault.jpg&key=a19fcc184b9711e1b4764040d3dc5c07&type=text%2Fhtml&schema=youtube) 

Building a GUI with Matisse, a UI designer with automatic alignment and constraints. Long live the (newly stylish) King?
We also need to focus on cheapness. The web has vast teams of full time developers building it. Much of their work is duplicated or thrown away, some of what they’ve created can be reused, and small amounts of new development might be possible … but by and large any NewWeb will have to be assembled out of bits of software that already exist. Beggars can’t be choosers.

Here’s my personal list of the best 5 things about the web:

1. Deployment & sandboxing
2. Shallow learning curve for users and developers
3. Eliminates the document / app dichotomy
4. Advanced styling and branding
5. Open source and free to use

These aren’t exactly the most famous design principles: if you asked most developers to sum up the essence of the web’s architecture they’d probably say URLs, view source, cross platform, HTTP and so on. But those are just implementation details. The web didn’t take over from Delphi and Visual Basic because JavaScript rocked so much more. There were deeper reasons.

The above things are pretty self explanatory. I’ll analyse them more as we go. To beat the web we must go further than just matching what it does well, we must also offer unique improvements the web cannot easily incorporate. Otherwise there’d be no point: we’d just work on making the web better instead.

I offer two things in this text: some design principles I think any serious web competitor should follow, and a concrete instantiation cobbled together out of various open source projects. Many readers won’t like that concrete instantiation because they’ll have other projects or languages in mind they like better, and that’s fine. I don’t care. It’s just an illustration — really it’s the principles that matter. I name specific codebases only to show that dreaming is not totally unrealistic.

### Design principles

The web lacks a coherent design philosophy, at least for the app parts. Here’s some things I think it should have but lacks:

1. **A clear notion of app identity**
2. **A unified data representation from backend to frontend**
3. **Binary protocols and APIs**
4. **Platform level user authentication**
5. IDE oriented development
6. Components, modules and a great UI layout system — just like regular desktop or mobile development.
7. It will also need the things the web does well: instant start of streaming apps with no installation or manual updates required, sandboxing of those apps, sophisticated UI styling, mix and match of “documenty” stuff with “appy” stuff (like being able to link to particular app views), a very incremental learning curve and a design that discourages developers from building UI that is too complicated.

The first four items in bold are all security related. That’s because security issues are my motivation for taking such an extreme position. Perhaps the low productivity of the web could be fixed with yet another JavaScript framework — perhaps. But I don’t think security can. I’ll look at the non-bold items in later parts.

### App identity and linking

To get the deployment and sandboxing benefits of the web, some sort of app browser will be required. We’d also like the ability to treat parts of an app as if it were a hypertext document so we can link to things. That’s a key part of the web’s success — an Amazon search results page is sort of an app, but because we can link to it, it’s also sort of a document.

But our app browser shouldn’t physically resemble a web browser. Look at it with fresh eyes: web browser UI is not excellent.

![](https://miro.medium.com/v2/resize:fit:1000/1*mdqGiMeX8-LDOHihp44QUQ.png)
The URL is a key part of the web’s design but it does make a mess sometimes!

The first problem is that URLs leak all over the UI. The URL bar routinely holds random bits of browser memory encoded in a form that is difficult to understand for both humans and machines, leading to a parade of exploits. If a desktop app dumped random internal variables into the title bar we would rightly regard that as brand-destroying bugginess, so why do we tolerate it here?

The second problem is that URLs are hard to read for machines too ([mind blowing exploits available here](https://www.blackhat.com/docs/us-17/thursday/us-17-Tsai-A-New-Era-Of-SSRF-Exploiting-URL-Parser-In-Trending-Programming-Languages.pdf)). Even ignoring clever encoding tricks like those, the web has various ways to establish the identity of an app. Browsers need some notion of app identity for separating cookie jars and tracking granted permissions. This is an “origin”. The concept of a web origin evolved over time and is essentially impossible to describe. [RFC 6454 tries](https://tools.ietf.org/html/rfc6454#section-3), but as it admits:

> Over time, a number of technologies have converged on the web origin concept as a convenient unit of isolation. However, many technologies in use today, such as cookies [RFC6265], pre-date the modern web origin concept. **These technologies often have different isolation units, leading to vulnerabilities.**
> 
> 

For example, ponder what stops your server setting a cookie on the .com domain. Now what about kamagaya.chiba.jp? (it’s not a website, it’s [a section of the hierarchy](https://publicsuffix.org/list/) like .com is!)

Being able to unexpectedly link to parts of an app that don’t benefit from it and aren’t expecting it is one of the sources of the “dangerous link” problem. Heavily URL-oriented design makes *all* of your app subject to data injection by malicious third parties, a design constraint that’s proven near impossible to handle well, yet is blithely encouraged by the web’s architecture.

Still, being able to link to the middle of an app from documents and vice-versa is pretty nice when it works and is expected by developers. Let’s take a couple of requirements:

**Requirement: app identity (isolation) must be simple and predictable.**

**Requirement: we must be able to deep link into an app, but not always.**

To their credit, the Android designers recognised these requirements and came up with solutions. We can start there:

* App identity is defined by a public key. All apps signed by the same key share the same isolation domain. String parsing is not used to determine isolation.
* Apps can opt-in to receiving strongly typed packets of data that that ask them to open themselves with some state. Android calls this concept “intents”. Intents aren’t actually *very* strongly typed in Android, but they could have been. They can be used for internal navigation as well, but to allow linkage from some other app the intent must be published in the app’s manifest. By default, the app cannot be invoked via links.
* Where to find an app to download? A domain name is a good enough starting point. Whilst we could support HTTP[S] downloads too, ideally we’ll find a NewWeb server listening on a well known port and we can fetch the code from there. So a domain name becomes a starting point for getting the app, but is otherwise quite unimportant.

To distinguish URLs and intents from our own take, let’s call them *link packets*.

Requiring developers to opt-in to external linkage will lower the linkability of NewWeb in order to improve its security. Perhaps that is a poor tradeoff and will be fatal. But many links people can theoretically create on the web today are invisibly useless anyway due to authentication requirements, or because they don’t contain any useful state to begin with (many SPAs). And such apps would minimize their surface area for attack. The malicious link that is the starting point of so many exploits would immediately become less severe. That seems valuable.

There are other benefits too — the importance of the domain name for webapps makes it tempting for domain providers to [seize domains their executives personally disagree with](http://www.telegraph.co.uk/technology/2017/08/29/worlds-oldest-neo-nazi-website-stormfront-shut/). Whilst simple websites can be relocated without too much pain, more complex sites that may have established email reputations, OAuth tokens etc are more badly harmed. Private keys can be lost or stolen, but so can domain names. If you lose your private key at least it’s your own fault.

As for the rest of the browser UI, it can probably be scrapped. Tabs could be useful, but reload should never be necessary and the back button can be provided by the app itself when it makes sense to do so, *a la* iOS.

How to build the app browser? Even though NewWeb is pretty different to the web, the basic UI of fullscreen apps is OK and users will want to switch back and forth. So why not fork Chromium and add a new tab mode?

(*edit: some people are interpreting this as “use Chrome for everything that isn’t listed here” which isn’t what I meant. I meant re-using its tabbed UI implementation so you can have NewWeb and OldWeb apps open next to each other easily. But tabbed UI is not difficult and so there’s no requirement to involve Chromium, the app browser could be a from-scratch dedicated app too)*

### A unified data representation

Maybe the link packet concept sounded a bit vague. What *is* this thing? To define it more precisely we need data structures.

The web has a variety of ways to do this but they’re all text based. To stress a point from my last article, text based protocols (not just JSON) have the fundamental flaw that buffer signalling is all ‘in band’, that is, to figure out where a piece of data ends you must read all of it looking for a particular character sequence. Those sequences may be legitimately a part of the data, so that means you also need an escaping mechanism. And then because these protocols are meant to be sorta human readable, or at least nerd readable, there are often odd corner cases to do with whitespace handling or Unicode canonicalisation that end up leading to exploits, like HTTP header splitting attacks.

In the early days this might have helped the web catch on with developers. View source has definitely helped me out more times than I can count. But the average web *page* [is now over 2 megabytes in size](https://www.soasta.com/blog/page-bloat-average-web-page-2-mb/), let alone web *app*. Even boring web pages of static text frequently involve masses of minified JavaScript that you need machine help to even begin to read. The benefits seem less than they once did.

A primitive decompiler
In fairness, the web has in recent years been slowly moving away from text based protocols. HTTP/2 is binary. The much touted “WebAssembly” is a binary way to express code, although it doesn’t actually fix any of the problems I’ve been discussing. But still.

**Requirement: data serialisation should be automatic, typed, binary and consistent from data store to front end.**

Serialisation code is not only tedious to write but also a major attack surface. A good platform would handle it for you. Let’s define a toy syntax for expressing data structures:

The web equivalent of this would be an amazon.com URL. It defines a immutable typed data structure to represent a request to open the app in some state.

That data structure is marked with `@PermazenType`. What’s this all about?

If we’re serious about being length prefixed, strongly typed and injection-free throughout the entire stack then we have to do something about SQL. The Structured Query Language is a friendly and well understood way to express sophisticated queries to a variety of stonkingly powerful database engines, so this is a pity. But, SQL is a text based API. Despite SQL injection being one of the easiest exploit types to understand and fix, it’s also one of the most common bugs in real websites. That’s no surprise: the most obvious way to use SQL in a web server will work fine but silently opens you up to being hacked. Parameterised queries help but are a bolt-on fix, which can’t be used in every situation. The tech stack creates a well disguised bear trap for us to fall into and our goal is to minimize the feature:bear ratio.

SQL has some other issues. You quickly hit the object-relational mapping problem. The results of a SQL query can’t be natively sent over an HTTP connection or embedded into a web page, so there’s always some required transformation into another format. SQL hides the performance of the underlying queries from you. The backends are hard to scale. Schema changes often involve taking table locks, making them effectively un-deployable without triggering unacceptable downtime.

NoSQL engines are not convincingly better: they typically fix one or two of these problems but at the cost of throwing out SQL’s solutions to the rest, meaning you’re often stuck with a solution that is different but not necessarily better. As a consequence the biggest companies like Google and Bloomberg have spent a lot of time researching how to make SQL databases scale ([F1](https://research.google.com/pubs/pub41344.html), [ComDB2](https://github.com/bloomberg/comdb2)).

[**Permazen**](https://github.com/permazen/permazen) is a new take on data storage that I find quite fascinating at the moment. Quoting the website:

> Permazen is a completely different way of looking at persistence programming. Instead of starting from the storage technology side, it starts from the programming language side, asking the simple question, “What are the issues that are inherent to persistence programming, regardless of programming language or database storage technology, and how can they be addressed at the language level in the simplest, most correct, and most language-natural way?”
> 
> 

Permazen is a Java library, however, its design and techniques could be used in any platform or language. It requires a sorted key/value store of the kind that many cloud providers can supply (it can also use an RDBMS as a K/V store), but it does everything else inside the library. It doesn’t have tables or SQL. Instead it:

* Uses the collection operations of the host language to do unions and intersections (joins).
* Schemas are defined directly by classes, it does not require an object/relational mapping to be defined.
* Can do incremental schema evolution ‘just in time’, avoiding the need for table locks to change how data is represented in the data store.
* Transactions are precisely defined and copying data out of a transaction or back in is clearly controllable by the developer.
* Change monitoring, such that you can get callbacks when the underlying data changes, including across processes (if the underlying K/V store supports this, which they often do).
* There’s a CLI that lets you query and work with the data store (e.g. to force data migrations if you don’t want just-in-time migration), and a GUI.

Really, you should just read the excellent [white paper](https://cdn.rawgit.com/permazen/permazen/master/permazen-language-driven.pdf) or [read the slides](https://s3.amazonaws.com/archie-public/jsimpledb/JSimpleDB-BJUG-Slides2016-05-05.pdf) (the slides use the old name for the library but it’s the same software). Permazen isn’t well known, but it’s the cleverest and slickest approach to integrating data storage tightly with an object oriented language that I’ve yet seen.

One of the interesting things about Permazen is that you can use ‘snapshot transactions’ to serialise and deserialise an object graph. That snapshot even includes indexes, meaning you can stream data into local storage if memory is too small and do indexed queries over it. It should be clear how this can provide a unified form of data storage with offline sync support. Resolving conflicts can be done transactionally as all Permazen operations can be done inside a serialisable transaction (if you want a Google Docs like conflict-free experience that will require an operational transform library).

NewWeb doesn’t *have* to use this sort of approach. You could use something a bit more traditional like protobufs, but then you need to use a special IDL which is equally inconvenient in all languages, and also tag your fields. You could use CBOR or ASN1. In my current project, [Corda](https://www.corda.net/), we have built our own object serialisation engine that builds on AMQP/1.0 and in which messages are by default self describing, so given a binary packet you can always get back the schema — as such it can do a View Source feature, kinda like XML or JSON. AMQP is nice because it’s an open standard and the base type system is pretty good (e.g. it knows about dates and annotations).

The point is that receiving data from potentially malicious sources is risky, so you want as few degrees of freedom in the format as possible and as many integrity checks as possible. All buffers should be length prefixed, so users can’t inject malicious data into a field to try and terminate it early. Data needs to be thoroughly checked as it’s loaded, so the best place to do that is in the type system and constructors of the data structures themselves — that way you can’t forget. Schemas help understand what the structure should be, making it harder for attackers to create type confusions.

Despite the fully binary nature of the proposed scheme, a one-way transformation to text can be defined for debugging and learning purposes. In fact, Permazen does that too.

### Simplicity and learning curve

One problem with type systems is that they add complexity; they can feel like a lot of pointless bureaucracy to developers who aren’t used to them. And without tooling, binary protocols can be harder to learn and debug.

**Requirement: a shallow learning curve**

I’m convinced that a big part of the web’s success is because it’s essentially untyped — everything is just strings. Bad for security, productivity and maintainability but *really* good for the learning curve.

One way this is being addressed in the web world is with gradually typed versions of JavaScript. That’s good and valuable research, but those dialects aren’t widely used and most new languages are at least somewhat strongly typed (Rust, Swift, Go, Kotlin, Ceylon, Idris …).

Another way to address this might be with a clever IDE: if you let developers start out with untyped data structures (everything is defined as `Any`) the runtime can figure out what the underlying types seem to be, and feed them back into the tool. It can then offer to swap in better type annotations. If the developer seems to be encountering typecast errors as they work, the IDE can offer to relax the constraint back again.

Of course, this gives less safety and maintainability than forcing the developer to really think about things up front, but even fairly weak heuristically determined types that only trigger errors at runtime can protect against a lot of exploits. Runtimes like the JVM and V8 already collect this sort of type information. In Java 9 there’s a new API that lets you control the VM at a low level (the JVMCI) which exposes this sort of profiling data — it’d be a lot easier to experiment with this type of tooling now.

### Languages and VMs

Talking of which, what language should NewWeb use? Trick question of course: the platform shouldn’t be fussy.

There have been many attempts to introduce non-JavaScript languages to the web over the years, but they all died trying to get consensus from browser vendors (mostly Mozilla). In fact the web has gone *backwards* over time in this regard — you used to be able to run Java, ActionScript, VBScript and more, but as browser vendors systematically erased plugins from the platform everything that wasn’t JavaScript got scrubbed out. That’s a shame. It could certainly use competition. It’s a sad indictment of the web platform that WebAssembly is the only effort to add a new web language, and that language is C … which I hope you don’t want to write web apps in! XSS is bad enough without adding double frees on top.

It’s always easier to agree on problems than solutions, but never mind — it’s time to get (more) controversial. The core of my personal NewWeb design would be the JVM. This will not surprise those who know me. Why the JVM?

* HotSpot can run the largest variety of languages of any virtual machine, and the principle of cheapness dictates that we want to be able to use as much open source code as possible.   
Yes, that means I want to be able to make apps that include [JavaScript](https://www.youtube.com/watch?v=OUo3BFMwQFo) (run at high speed), along with [Ruby](http://chrisseaton.com/rubytruffle/), [Python](http://www.jython.org/), [Haskell](http://eta-lang.org/), and oh yes — [C, C++ and Rust should come along for the ride too](https://llvm.org/devmtg/2016-01/slides/Sulong.pdf). All this is possible because of two really cool projects called Graal & Truffle, [which I’ve written about extensively before](https://blog.plan99.net/graal-truffle-134d8f28fb69). These projects let the JVM run even languages you wouldn’t expect (like Rust) in a virtualized, fast and interoperable way. I don’t know of any other VM that can blend so many languages together, so seamlessly, and with such good performance.
* The OpenJDK sandbox has been attacked relentlessly for years and has learned the same painful lessons that web browser makers have. The last zero day exploit was in 2015, and the one before that was in 2013. Just two sandbox 0-days in 5 years is good enough for me, especially because lessons have been learned from each exploit found and fundamental security improvements have been made over time. No sandbox has a perfect track record, so really this boils down to a personal judgement call — what level of security is *good enough*?
* There’s huge quantities of high quality libraries that solve key parts of the puzzle, like Permazen and JavaFX.
* I happen to know it quite well and I like Kotlin, which has a JVM backend.

I expect lots of people to disagree with that choice. No problem — the same design ideas can be implemented on top of Python or V8 or Go or Haskell, or whatever else floats your boat. Best if you can pick something that has an open spec with competing implementations though (the JVM does).

The politics of Oracle don’t concern me much because Java is open source. There aren’t a lot of high quality, high performance open source runtimes out there to choose from. The ones that do exist are all built by giant software firms who have all made their own questionable decisions in the past as well. The web has at various times been heavily influenced or outright controlled by Microsoft, Google, Mozilla and Apple. All of them have done things I found objectionable: that’s the nature of large companies. You won’t agree with their actions all the time. Oracle won’t win any popularity contests, but the nice thing about open source code is that it doesn’t really have to.

There are a few things I’d want to reuse from Google Chrome specifically. My app browser should support an equivalent to WebGL (i.e. eGL), and Chromium’s [ANGLE project](https://chromium.googlesource.com/angle/angle/+/master/src/) would be useful for that. The app browser should silently auto update like Chrome does, and Google’s auto update engines are open source too. Finally, whilst the Pack200 format compresses bytecode by a lot, throwing in nice codecs like [Zopfli](https://en.wikipedia.org/wiki/Zopfli) wouldn’t hurt.

### RPC

Binary data structures aren’t enough. We need binary protocols too. A standard way to link client with server is RPC.

One of the coolest British startups right now is [Improbable](https://improbable.io/company/about-us), who have published [a great blog post on how they’re moving away from REST+JSON and towards gRPC+protobufs from the browser to the server](https://improbable.io/games/blog/grpc-web-moving-past-restjson-towards-type-safe-web-apis). Improbable describe the result as a “type safe nirvana”. Browsers don’t make this as easy as HTTP+XML or JSON, but with enough JavaScript libraries you can bolt it on top. It’s a good idea. Back in the real world where we’re all writing web apps, you should do this.

RPC protocols take some experience to design. My dream RPC supports:

* Returning futures (promises) and [ReactiveX Observables](http://reactivex.io/), so you can stream “push” events naturally.
* Returning large or infinitely sized byte streams (e.g. for live video).
* Flow control for high vs low priority transfers.
* Versioning, interface evolution, type checking.

Again, in the Corda project we’ve tackled designing [an RPC stack](https://docs.corda.net/clientrpc.html) with these properties. It isn’t currently released as a standalone library but perhaps in future we’ll do that.

HTTP/2 is one of the most recent and most designed parts of the web, and it’s a decent enough framing and transport protocol. But it inherits a lot of cruft from HTTP. [Consider the hacks opened up by obscure HTTP methods that are never used](https://www.owasp.org/index.php/Test_HTTP_Methods_(OTG-CONFIG-006)). HTTP’s odd approach to state doesn’t help. HTTP itself is stateless, but apps need stateful sessions, so app devs have to add their own on top using a mishmash of easily stealable cookies, tokens and so on. This leads to problems like [session fixation attacks](https://en.wikipedia.org/wiki/Session_fixation).

It’d be better to split things more clearly into layers:

* Encryption, authentication and session management. TLS does a decent job of this. We’d just reuse it.
* Transport and flow control. HTTP/2 does this, but messaging protocols like AMQP also do it, without the legacy.
* RPC

The RPC stack is obviously integrated with the data structure framework, so all transfers are typed. Developers never need to do any parsing by hand. RPC *naming* is just a simple string comparison. There are no path traversal vulnerabilities as a consequence.

### User authentication and sessions

NewWeb would not use cookies. It’s better to use a public/private keypair to identify a session. [The web is heading in this direction](https://www.ietf.org/proceedings/90/slides/slides-90-uta-0.pdf), but to be compatible with existing web apps there has to be a complicated “binding” step where bearer tokens like cookies are connected to the underlying cryptographic session and that’s just needless complexity in a security sensitive component — it can be tossed in a clean redesign. Sessions are only ever identified on the server side by a public key.

User authentication is one of the hardest bits of a web app to get right. [I wrote down some thoughts on that previously](https://blog.plan99.net/building-account-systems-f790bf5fdbe0) so I won’t go deep on that again. Suffice it to say that linking a session to an email address is a feature best handled by the base platform and not constantly re-invented at the app layer. A client side TLS certificate is sufficient to implement a basic single sign-on system, with the basic ‘send email and sign certificate if received’ flow being so cheap that Lets Encrypt style free providers are not unthinkable.

This approach builds on tech that already exists, but should make a serious dent in phishing, password database breaches, password guessing and many other related issues.

### Conclusion

A platform that wants to compete with the web should have a strong focus on security, as that’s the primary justification and competitive advantage. It’s the main thing you can’t easily fix about the web. That means: binary, type safe data structures and APIs, RPC, cryptographic sessions and user identification.

It also means code that’s sandboxed and streaming, UI that has great layout management yet is also responsive and styleable, some way to blend documents and apps together as well as the web does, and a very productive development environment. We also need to consider the politics of the thing.

I’ll take a look at these other aspects [in the last and final post in this series](https://blog.plan99.net/more-design-principles-for-the-newweb-b88582dcb4e0).

![](https://miro.medium.com/v2/da:true/resize:fit:0/5c50caa54067fd622d2f0fac18392213bf92f6e2fae89b691e62bceb40885e74)
