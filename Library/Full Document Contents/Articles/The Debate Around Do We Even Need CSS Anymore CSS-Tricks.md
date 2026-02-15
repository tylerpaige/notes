# The Debate Around "Do We Even Need CSS Anymore?" | CSS-Tricks

![rw-book-cover](https://css-tricks.com/wp-json/social-image-generator/v1/image/204067)

## Metadata
- Author: [[Chris Coyier]]
- Full Title: The Debate Around "Do We Even Need CSS Anymore?" | CSS-Tricks
- Category: #articles
- Publication date: 2015-06-26
- URL: https://css-tricks.com/the-debate-around-do-we-even-need-css-anymore/
- Summary: The debate about whether we still need CSS is growing, with some developers opting for inline styles managed through JavaScript instead. Critics argue that CSS offers better separation of concerns and is simpler to use, while others appreciate the dynamic capabilities of inline styles. Despite the shift towards using JavaScript for styling, many still believe that CSS remains essential for effective web design.

# Notes

[[Chris Coyier - The Debate Around "Do We Even Need CSS Anymore?" | CSS-Tricks]]

***

This has become quite the hot topic lately. It’s been talked about at a number of conferences and meetups I’ve been at personally lately. I’ve seen slide decks on it. I know people literally not shipping any CSS in production. Pretty wild, eh?

I thought we could have a little campfire here and talk about it as rationally as we can, covering all the relevant points.

##### We obviously still need to style things

Nobody is saying we don’t need styles. We still need to style things, what’s being talked about is how and where we do that. I was just on a panel at [BrooklynJS](http://brooklynjs.com/) and [Jed Schmidt](https://twitter.com/jedschmidt) said:

> The worst things about CSS are the “Cascading” and the “Sheets”
> 
> 

##### What does anyone have against CSS?

These are the main arguments against CSS:

* **Everything is global.** Selectors are matched against everything in the DOM. You need *naming strategies* to combat against this and keep things efficient (which are hard to enforce and easy to break).
* **CSS grows over time.** Smart people on great teams cede to the fact that they are afraid of their own CSS. You can’t just delete things as it’s so hard to know if it’s absolutely safe to do that. So, they don’t, they only add. I’ve seen a graph charting the size of production CSS over five years show that size grow steadily, despite the company’s focus on performance.
* **You can be more dynamic with styles in a programming language.** The argument goes something like “we’re already juicing up CSS with preprocessors anyway, might as well kick it up a notch.” You could for instance (if you *really* wanted to make this controversial) base styles off a User-Agent string or a module’s current width.

##### What is the alternative to CSS then?

The alternative is inline styles. So instead of:

We’re talking:

I haven’t heard anyone yet argue you should apply these styles directly to HTML you author. The idea is you apply styles to elements through JavaScript.

##### React is driving a lot of these thoughts

[React](http://facebook.github.io/react/) is a JavaScript library that helps with *view* concerns in websites. It’s developed mainly by Facebook, extremely popular, and gaining momentum. It’s had it’s [own conference](http://conf.reactjs.com/) and is even growing into a framework for [building native apps](https://facebook.github.io/react-native/).

One of it’s core concepts is the “Virtual DOM”. You build the HTML you intend to use right in the JavaScript. Seemingly quite weird at first, but this coupling between HTML and JavaScript is always there and it appeals to people to just write it together from the get-go. I quoted Keith J Grant recently, and I [will again](http://keithjgrant.com/posts/against-css-in-js.html):

> This coupling is real, and it is unavoidable. We must bind event listeners to elements on the page. We must update elements on the page from our JavaScript. Our code must interact bidirectionally and in real-time with the elements of the DOM.
> 
>  … the mantra of React is to stop pretending the DOM and the JavaScript that controls it are separate concerns.
> 
> 

React has the ability to manage inline styles built right in. They call them what they are: [inline styles](https://facebook.github.io/react/tips/inline-styles.html). Here’s a basic example:

See the Pen [Inline Styles with React](http://codepen.io/chriscoyier/pen/PqOpEX/) by Chris Coyier ([@chriscoyier](http://codepen.io/chriscoyier)) on [CodePen](http://codepen.io).

The virtual DOM thing that React does is also important because of its speed. DOM manipulation stuff is generally regarded as slow in JavaScript, and thus managing styles through DOM manipulation would also be slow. But React has the magic dust that makes manipulation fast, so people don’t worry about the slowness issues when working with React.

[Here’s another example](http://codepen.io/chrisnager/pen/MwOjqe?editors=001) by Chris Nager.

##### The style authoring is still abstracted

CSS is the abstraction of style away from anything else. Literally files you open and work on to manage styles. You likely aren’t giving that up when moving to a JavaScript-based inline-style setup. You’d just have, probably, `style.js` instead of `style.css`. You’d still be writing key/value pairs and smooshing files together in a build process.

It will be different, but the authoring abstraction is still there.

##### What do you get out of inlining styles?

###### Cascade-less

The scary “global” nature of CSS is neutered. The cascade, tapered. I don’t think you could say the cascade is entirely gone, because some styles are inherited so styles can still be passed down to child elements and that’s one definition of cascade. But the module-ish nature of this style of development likely leads to less overlapping style concerns. A module over here is styled like this, a module over there is styled like that – probably no conflicts in sight.

###### All JavaScript

One sense I get is that some people just like and prefer working in all JavaScript. You could certainly attribute some of the success of Node.js to that fact.

###### Dynamic Styles

“State” is largely a JavaScript concern. If you want/need style to change based on dynamic conditions (states) on your site, it may make sense to handle the styling related to the state change along with everything else.

In [a recent talk at CSS Conf](https://www.youtube.com/watch?v=NoaxsCi13yQ) ([slides](https://docs.google.com/presentation/d/1pL8e2OC8iDUWCvGkixYB18bRXjiVRmSgwiWoxiQN3vQ/edit#slide=id.ga1b311e93_1_22)), [Colin Megill](https://twitter.com/colinmegill) used the example of the Twitter new tweet input textarea as a dynamic place that changes the state of other elements.

![](https://i0.wp.com/css-tricks.com/wp-content/uploads/2015/06/state-changes.gif)
##### Who’s actually doing this?

I heard Colin Megill say they are shipping literally zero CSS on “big stuff” and not seeing performance problems. I’ll update this with URL’s if I get them. I hear one big project will be live within a month.

I know Jed Schmidt works on [the mobile version of UNIQLO](https://m.uniqlo.com/us/), and you can see the inline styles at work there:

![](https://i0.wp.com/css-tricks.com/wp-content/uploads/2015/06/inline-styles-on-uniqlo.jpg)
**Update from Jed:** This version of the site is all Sass and the inline styles you see there are from JavaScript animations.

[Christopher Chedeau](http://blog.vjeux.com/) [has been talking about this](https://speakerdeck.com/vjeux/react-css-in-js) and is literally a Facebook engineer, so maybe Facebook a little?

##### Can this concept be combined with, you know, CSS?

Even if you bought into the concept of inline styles, can it live in harmony with some (do I have to say it) *regular* CSS? Is page layout appropriate as inline styles? Doesn’t base typography still make sense to do globally? I’m not sure if we’re far along enough in this world to see a best practice emerge.

In the m.uniqlo.com example above, they are shipping a 57k CSS file as well, and you can see evidence of state-based class in the DOM as well (e.g. “is-open”).

##### A lot of people really don’t like this

Surprise! There are more arguments against this kind of thing than for it. As I was collecting opinions about this, I told Lea Verou “Some people really like this idea!” to which she told me:

> You can find people in the world who like eating excrement it doesn’t mean it’s a good idea.
> 
> 

Let’s run through other arguments:

###### Styling is what CSS is for

This is the “religious” angle that probably isn’t going to take us very far.

###### The separation of concerns is inherent to CSS

Separation of concerns is a very useful concept when building things as complex as websites are. You get seperation of concerns automatically when writing CSS: it’s a file just for styling.

###### Inline styles are at the top of the specificity spectrum

Keeping specificity low in CSS means that when you do need to rely on specificity to win a styling war, you have it available as a tool. When you’re already at the top, you don’t have that wiggle room luxury.

The `!important` declaration can still win a specific property/value styling war over an inline style, but that’s a slightly different concept and an even grosser war to fight.

###### Some simple states are much easier in CSS

How do you do :hover/:focus/:active in inline styles? You don’t. You fake it. And what about media queries?

###### Adding/removing classes is a perfect tool for state changes already

JavaScript is really good at adding/removing/changing classes on elements. And classes are a perfect way to handle state in CSS.

```
.is-open {
 display: block;
}
```

Plus you can change state on parent elements (by changing a class) to affect the state of lots of elements within:

```
.tweet-too-long {
 .tweet-button {
 opacity: 0.5;
 }
 .warning-message {
 display: inline-block;
 }
}
```

###### Browsers aren’t made to deal with styling in this way

For instance, inline styles are literally data kept in an attribute right on the DOM element. DOM weight is a thing (it can cause browsers to be slow or crash). That styling information isn’t only just kept in the style attribute though, it’s also represented in the DOM in the element’s style properties. Is that the same thing, or is this kinda double-weighted styling?

Are there speed differences between…

```
var thing = document.getElementById("thing");
thing.style.width = "100px";
thing.style.height = "100px";
thing.style.backgroundColor = "red";

var thing2 = document.getElementById("thing-2");
thing2.setAttribute("style", "width: 100px; height: 100px; background-color: red;");
```

Has that been figured out to discover what’s best cross-browser? If this stuff takes off, will browsers need to evolve to handle things in a different way?

The browser has this whole concept of the CSSOM. Can we use that somehow more intelligently through JavaScript rather than inline styles?

###### CSS is successful because of it’s simplicity

CSS is a fairly easy to jump into. A lot of people know it. You can hire for it. It’s portable.

###### Some of these “dynamic” styling concerns can be solved with regular CSS

* There are demos that include measuring widths and subtracting fixed values from them. CSS can do that with `calc()`.
* There are demos that include setting `font-size` or `line-height` that depends on the browser width or height. CSS can do that with viewport units. Using JavaScript for this kind of thing is overtooling.
* There are demos that dynamically change colors on many different elements. CSS will be able to do that with native variables.

###### We tried this in 1996 and it was a bad idea then

[Get off my lawn.](https://en.wikipedia.org/wiki/JavaScript_Style_Sheets)

###### CSS is cacheable

The network is still the bottleneck. CSS files can be cached so the network doesn’t even come into play and that is smoking fast.

###### You can still use React

React is pretty awesome. Here’s an article by [David Khourshid on Styling React Components in Sass](http://kittygiraudel.com/2015/06/18/styling-react-components-in-sass/). Mark Dalgleish [doesn’t like the global nature of CSS](https://medium.com/seek-ui-engineering/the-end-of-global-css-90d2a4a06284), and has working concepts to localize selectors. Glen Maddern expounds upon this in [Interoperable CSS](http://glenmaddern.com/articles/interoperable-css).

###### Doesn’t anyone care about progressive enhancement anymore?

This is a wider conversation is perhaps out-of-scope here. Because sites (including React based sites using inline styles) can be rendered entirely server-side, it means they can be done with progressive enhancement in mind. Although “can be” and “what it actually encourages” are different things.
