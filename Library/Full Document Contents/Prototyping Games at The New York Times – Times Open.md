---
Author: The NYT Open Team
Full Title: Prototyping Games at The New York Times
Category:
Publication date: 2021-01-21
URL: https://open.nytimes.com/prototyping-games-at-the-new-york-times-82092495a924
Summary: Live Fast. Die Hard. Document.
Publication: NYT Open
---

![rw-book-cover](https://miro.medium.com/v2/resize:fit:1080/0*TYU_j0O7QyXKG8pr.)

# Notes

[[The NYT Open Team - Prototyping Games at The New York Times – Times Open]]

***

#### Live Fast. Die Hard. Document.

**By** [**JEAN KIM**](https://twitter.com/_jeanyoungkim)**,** [**ROBERT VINLUAN**](https://twitter.com/RobertVinluan) **AND** [**SAM VON EHREN**](https://twitter.com/samvonehren)

This summer, The New York Times Games Team set out to explore new audiences and opportunities by rapidly prototyping and user testing games. We built and tested three different prototypes: Spelling Bee, KenKen and Word Maze, and learned something new with each one. Creating a process for building prototypes that were released to our large audience (which is about a million people) was quite a journey. We’d like to talk through our methodology and share some of the things we’ve learned along the way.

### Who We Are

The Games Team is mostly responsible for the digital Crossword. We have a smaller subset of the team responsible for what we call Games Expansion: Jean Kim (Technology), Robert Vinluan (Product Design) and Sam Von Ehren (Game Design). Our primary objective is to test the viability of new games as quickly as possible.

While we each have our specialities, we’re all multidisciplinary: Robert and Jean have design and engineering backgrounds, and Sam has an engineering and game design background. These mixed skillsets allow us to have a freeform team structure so we can work on projects independently or help each other as needed. At any given time, our team can work on up to three separate prototypes at once.

### How We Work

Our team philosophy is “Live fast, die hard, document.” Which is to say, we race toward an MVP as quickly as possible, rigorously test for viability and document every step of the way.

Documentation may seem like the enemy of speed, but as a small team within a much larger organization, we often have to coordinate with other teams, codify our objectives and explain our processes. On top of that, documentation provides a detailed history of our successes and failures, and records our work for posterity.

### Live Fast

We’ve made an explicit choice to use any and all libraries, APIs, SDKs and hacks available to us in our prototyping process. Our goal is to assess the viability of games, not build production-ready products, so we operate under the assumption that any code we write will eventually be thrown out.

![](https://miro.medium.com/v2/resize:fit:700/0*TYU_j0O7QyXKG8pr.)
To give you a sense of how this works in practice, let’s look at some of the technical decisions that went into building Spelling Bee:

* Zero frameworks were harmed in the making of this game. The entire game was built with three files: index.html, main.js and styles.css.
* It was built specifically for mobile devices.
* We chose to only support iOS 9 and above, and the latest versions of mobile browsers.
* Puzzle data came from a static JSON file and game progress was saved in localStorage.
* main.js was written in es6 and manually run through Babel (read: copied and pasted into <https://babeljs.io/repl/>) to support older browsers.

This approach allowed us to build the Spelling Bee prototype in three days(!!). The takeaway from all of this is that you shouldn’t be too precious — or precious at all — with your code while prototyping.

> Building an interactive prototype is a means to an end when testing for viability, and you should never let technical decisions slow you down.
> 
> 

### Die Young

To verify and improve our prototypes, we employ an array of different testing strategies depending on what stage of development we are in and what we want to learn (There are a lot, but that’s a topic for another post). One strategy is “informal in-person testing:” make a basic version of your game, put it in front of someone and watch them play. This gives you a sense of what’s working and where your game is failing.

![](https://miro.medium.com/v2/resize:fit:700/0*VhEWYKrOJLngHT-j.)
Our KenKen prototype included an interactive tutorial. To test it, we actually walked around the Times Building asking people with no KenKen experience to play the game while we watched. In doing this, we were able to identify specific points where people struggled or succeeded. After each session we’d adjust the prototype for the next tester, developing a rapid cadence of testing and iterating that would completely transform the tutorial over the course of just a few hours.

### Document

For each game, we created a slew of documents covering everything from Game Design, to outlining necessary features, to test plans that specified exactly what metrics we wanted to track.

One of our most important documentation strategies is frequent post mortems — in software development parlance, a post mortem is a type of meeting where developers analyze and discuss how a project went. While this can feel like a lot of administrative overhead, it serves two vital functions:

* Within the actual post mortem, impediments to the process can be identified and we can collaborate on finding a solution.
* Within the day-to-day process, we can cut as many corners as possible (live fast), knowing we’ll reflect on the decision and assess its impact in a judgement free zone.

![](https://miro.medium.com/v2/resize:fit:700/0*RQlFGMuJfrZKchL9.)
This was most important with our third prototype, Word Maze. The development and testing process had an unusually high number of issues. We worked off year-old code, changed the visuals late in the process and engaged in a series of dubious hot fixes. Immediately having a post mortem allowed us to discuss and solidify preventative measures. In this case: a process for scrapping code, visual design convention guides and hard limitations on hot fixes.

### Teamwork makes the dream work

At the end of the day, our “live fast, die hard” mentality has been crucial to figuring out both our approach to building new products and our team’s process. We firmly believe in the cycle of getting feedback and iterating accordingly, and our development process will always value completion over perfection.

Product development tends to focus on lengthy planning sessions and steady progress towards a “perfect” end result. While there is certainly a time and place for this approach, it exponentially increases the cost of failure during the discovery phase of a product’s lifecycle. As we’ve tried to figure out the future of the Games Team’s portfolio, it’s been liberating and instructive to embrace the possibility of failure in pursuit of new directions.

We’re continuing to improve the way we approach rapid prototyping and are excited to see where it takes us. Stay tuned for more updates!

[*Jean Kim*](https://twitter.com/_jeanyoungkim) *was most recently a web developer on the Games Team at The New York Times. On October 16th, she’ll join* [*Maven*](https://www.mavenclinic.com) *as a front-end developer.*

[*Robert Vinluan*](https://twitter.com/RobertVinluan) *is a product designer on the Games Team, working on Crosswords and new prototypes. Play board games with me.*

[*Sam Von Ehren*](https://twitter.com/samvonehren) *is a game designer on the Games Team, and a retired fighting game player.*
