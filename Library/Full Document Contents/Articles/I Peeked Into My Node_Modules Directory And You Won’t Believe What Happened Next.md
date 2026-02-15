# I Peeked Into My Node_Modules Directory And You Won’t Believe What Happened Next

![rw-book-cover](https://miro.medium.com/focal/1200/632/47/45/1*ppvsoAlJoTunobAhbep9zA.jpeg)

## Metadata
- Author: [[Jordan Scales]]
- Full Title: I Peeked Into My Node_Modules Directory And You Won’t Believe What Happened Next
- Category: #articles
- Publication date: 2020-06-07
- URL: https://medium.com/@jdan/i-peeked-into-my-node-modules-directory-and-you-wont-believe-what-happened-next-b89f63d21558
- Summary: https://medium.com/@jdan/i-peeked-into-my-node-modules-directory-and-you-wont-believe-what-happened-next-b89f63d21558

# Notes

[[Jordan Scales - I Peeked Into My Node_Modules Directory And You Won’t Believe What Happened Next]]

***

#### Just absolute madness

![](https://miro.medium.com/max/700/1*ppvsoAlJoTunobAhbep9zA.jpeg)Photo: Mint Images/Getty Images
The left-pad fiasco shook the JavaScript community to its core when a rouge developer removed a popular module from npm, causing tens of projects to go dark.

While code bloat continues to slow down our websites, drain our batteries, and make “npm install” slow for a few seconds, many developers like myself have decided to carefully audit the dependencies we bring into our projects. It’s time we as a community stand up and say enough is enough, this community belongs to all of us, not just a handful of JavaScript developers with great hair.

Am I being paranoid? Maybe. Am I overestimating the hard work that goes into running an open source project? Most likely. Was I kicked off my ZogSports team because I “make sports less fun for everyone involved”? Yes.

I decided to document my experiences in auditing my projects’ dependencies, and I hope you find the following information useful.

### Express

Behind the fastest, leanest JavaScript web framework is a heaping pile of dependencies, each with their own heaping pile of dependencies. In fact, a simple “npm install express” leads to 291 installed modules.

```
$ tree node_modules/ | count  
zsh: command not found: count  
$ tree node_modules/ | lines  
zsh: command not found: lines  
$ tree node_modules/ | wc -lines  
wc: illegal option — i  
usage: wc [-clmw] [file …]  
$ tree node_modules/ | wc -countlines  
wc: illegal option — o  
usage: wc [-clmw] [file …]  
$ man wc  
$ tree node_modules/ | wc -l  
  292
```

Imagine if the apple you were eating for breakfast had 291 ingredients, or if the car you drove to work had 291 parts. You’d be worried, wouldn’t you? Yet, for some reason, we’re totally fine installing 291 individual modules just to power an enterprise-grade web server capable of handling thousands of incoming requests per second.

So, what’s in these dependencies anyway? Many are self-explanatory: “range-parser” parses ranges, “escape-html” escapes html, and “negotiator” makes great deals.

However, one dependency — “yummy” — caught my attention.

```
├── yummy  
│ ├── LICENSE  
│ ├── README.md  
│ ├── like-tweet.js  
│ ├── index.js  
│ └── package.json
```

Interesting. Besides the normal “index.js” and “package.json,” we find a suspicious “like-tweet.js.” I decided to take a closer look.

```
var http = require(“http”)http.request({  
  method: “POST”,  
  hostname: “api.twitter.com”,  
  path: “/hotpockets/status/501511389320470528”,  
})
```

Hold on a second. “like-tweet.js,” which runs whenever your application loads up the popular express library, makes a POST request to the twitter API. Why? I went ahead and loaded up this route myself.

Some content could not be imported from the original document. [View content ↗](https://medium.com/@jdan/i-peeked-into-my-node-modules-directory-and-you-wont-believe-what-happened-next-b89f63d21558) 

Sure enough, it’s a tweet from Hot Pockets, and I had already favorited it. In fact, every time you download express, you favorite this exact tweet from Hot Pockets: introducing their new signature hickory ham sandwich pastries filled with real ham, real cheese, and a variety of chef-inspired sauces.

What sort of monster brokered an advertising deal like this? I pass sensitive customer data through express, and they go ahead and sell my Twitter favorites to Hot Pockets? Needless to say, I likely won’t be using express again.

### Ember

Ember.js is a JavaScript web framework that specializes in quickly rendering to-do lists. It packs quite the punch at only 112 kilobytes minified and gzipped, but what most people don’t know is how much of that 112 kilobytes is spent on nothing.

If we peek into Ember’s dependencies, we see a library called “Glimmer.” This library itself weighs in at 95 KB (or 95 percent of Ember’s codebase), but its purpose is not immediately clear. (It is [not even mentioned](https://github.com/emberjs/ember.js/search?utf8=%E2%9C%93&q=glitter) on Ember’s internet homepage!)

![](https://miro.medium.com/max/700/1*zvDxVb7XhGfQFk0Lhzc6Ew.png)Screenshot from Github
Additionally, I was unable to find [any Google results for Glimmer](https://www.bing.com/search?q=glitter&qs=n&form=QBRE&pq=glitter&sc=8-6&sp=-1&sk=&cvid=0D14DF9331B348988DDAF04DDB346DE0) that had to do with JavaScript. So what’s going on here?

Well, looking at Glimmer’s dependencies, I found something quite curious:

```
├─┬ glimmer@1.1.5  
│ ├─┬ brittanica@13.0.2  
│ │ ├── brittanica-a@0.0.1  
│ │ ├── brittanica-b@0.1.2  
│ │ ├── brittanica-c@0.0.3  
│ │ ...
```

“brittanica” — a module using up 93 KB (or 93 percent) of Glimmer’s code size, must be doing all the heavy lifting. It itself consists of many dependencies — each one a module also named “brittanica,” suffixed with a letter of the English alphabet (26 in total).

Poking further, I found that each module consists of a single terms.json file. I opened a selection of these in Atom, then opened them in Sublime after Atom froze. The following gibberish appeared:

```
{  
    "g": {  
        "page": 1018,  
        "description": "The seventh letter of the US English..."  
    },  
    "ga": {  
        "page": 1021,  
        "description": "a Kwa language of Ghana, spoken in Acc..."  
    },    ...    "glimmer": {  
        "page": 1172,  
        "description": "A faint or wavering light, used pri..."  
    },    ...  
}
```

A total 17,648 lines describing various words and phrases that begin with the letter G. What on earth could possibly need these? Well, you’re in for quite the surprise. Meet glimmer/help.js.

```
module.exports = function help() {  
    var descriptino = require("brittanica-g").glimmer;  
    console.log("glimmer (n). " + descriptino);  
    console.log("");  
    console.log("Copyright (c) 2016 Tilde Inc");  
    console.log("");  
    ...  
}
```

In case the above code snippet is unclear, allow me to summarize:

* Ember prides itself on using Glimmer: a small, lightning-fast rendering library.
* Glimmer brings in the entirety of Encyclopedia Brittanica, just to display the definition for the word “glimmer” in its help menu.
* Our oceans are dying at an alarming rate, and we’re all too busy staring at our phones playing Pokémon to have a conversation about it.

Just absolute madness.

### Babel

Babel is a compiler for next-generation JavaScript that beat its competition when someone at Facebook said they liked it better.

Though loved by many, Babel has had its fair share of critics. Many complain about its confusing plugin system, overly complicated config files, and unclear error messages. Curiously, not many seem to notice the incredible amount of dependencies Babel requires. Until now.

I started my investigation by installing the “babel-preset-es2015” package. This package allows twentysomething web developers to write a newer, worse version of JavaScript that no one else on their team knows. I then counted the number of dependencies brought in by this package, followed by the total code size, using two commands I absolutely didn’t just look up on StackOverflow.

```
$ ls node_modules | wc -l  
    90  
$ du -sh node_modules  
    17M node_modules
```

A whopping 90 dependencies totaling 17 megabytes. Let that soak in.

If the entire recorded history of humanity could fit in a single megabyte, then Babel alone would consist of 17 times the entire recorded history of humanity. Just so that we can avoid writing JavaScript.

So I started wondering, what on earth is causing Babel’s code to be so large?

One of the biggest offenders, a package called “babel-core” was suspiciously large, coming in at 13 megabytes on its own. I opened up babel-core in vim, then turned off my computer because Ctrl-C wasn’t exiting, then opened babel-core in Sublime Text 2.

```
module.exports = require("./lib/api/node.js");
```

About two hours later, I successfully found the referenced “/lib/api/node.js” and discovered the crux of the issue. A coding slip-up so unforgivable that I nearly threw out my MacBook and swore off web development forever.

![](https://miro.medium.com/max/700/1*4UF6rEG73xHxcjDn5Abb3A.png)Screenshot by the writer
It’s true. Each installation of Babel includes a picture of Guy Fieri, and there is nothing you can do about it.

I have no idea if this picture was supposed to be stripped out before pushing to npm, or how this mistake passed code review. Either way, it’s there, and it’s taking up precious space on millions and millions of 15-inch Retina MacBook Pro hard drives across the world.

### Conclusion

To be clear, I don’t think that extra dependencies are a sign of the end of the world. I just think it’s important that we as a community start to value the work of maintaining and cleaning up code as much as we value cool features and great project logos.

We can do better. Dead code takes up space, eats up bandwidth, and can even kill your startup.

I encourage you to take a look inside your node\_modules/ directory next time you have a couple of free hours and a computer with at least 16 gigabytes of RAM. The results may just surprise you.
