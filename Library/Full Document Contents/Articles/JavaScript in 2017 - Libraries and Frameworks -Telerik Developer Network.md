# JavaScript in 2017 - Libraries and Frameworks -Telerik Developer Network

![rw-book-cover](https://d585tldpucybw.cloudfront.net/sfimages/default-source/blogs/older-content/tdn/future_js_2017_header7c87182ea68748fca9cfc75310553a56.jpg?sfvrsn=cc64ff5a_1)

## Metadata
- Author: [[Burke Holland]]
- Full Title: JavaScript in 2017 - Libraries and Frameworks -Telerik Developer Network
- Category: #articles
- Publication date: 2017-02-14
- URL: http://developer.telerik.com/topics/web-development/javascript-2017-libraries-frameworks/
- Summary: JavaScript in 2017 - Libraries and Frameworks is out. Stay connected to Telerik Blogs for .NET, JavaScript, cross-platform app development (and beyond) news and tutorials.

# Notes

[[Burke Holland - JavaScript in 2017 - Libraries and Frameworks -Telerik Developer Network]]

***

![future_js_2017_header](https://d585tldpucybw.cloudfront.net/sfimages/default-source/blogs/older-content/tdn/future_js_2017_header7c87182ea68748fca9cfc75310553a56.jpg?sfvrsn=cc64ff5a_1)JavaScript in 2017 - Libraries and Frameworks\_future\_js\_2017\_header
![Angular](https://d585tldpucybw.cloudfront.net/sfimages/default-source/blogs/older-content/tdn/angular-banner.jpg?sfvrsn=b8885dff_1)
![React](https://d585tldpucybw.cloudfront.net/sfimages/default-source/blogs/older-content/tdn/react-banner.jpg?sfvrsn=c8fc8b1c_1)
![Vue](https://d585tldpucybw.cloudfront.net/sfimages/default-source/blogs/older-content/tdn/vue-banner.jpg?sfvrsn=2da9dcb8_1)
![Ember](https://d585tldpucybw.cloudfront.net/sfimages/default-source/blogs/older-content/tdn/ember-banner.jpg?sfvrsn=77e800f1_1)
![](https://d585tldpucybw.cloudfront.net/sfimages/default-source/blogs/older-content/tdn/aurelia-banner.jpg?sfvrsn=c24b68dd_1)
![Aurelia](https://d585tldpucybw.cloudfront.net/sfimages/default-source/blogs/older-content/tdn/aurelia-interest.jpg?sfvrsn=efcb1b00_1)
![](https://d585tldpucybw.cloudfront.net/sfimages/default-source/blogs/older-content/tdn/web-components-support.jpg?sfvrsn=3b6bdf53_1)
"Angular will be the dominant framework..." what is this based on?

Angular 1.x was successful by the standards of when it was released, whereas Angular 2+ is an entirely separate library which just happens to share the same name, and which has not had anything close to the same impact. The "throw away and start again" approach with 2.x immediately converted all the community investment into legacy.

Meanwhile React exploded into dominance by being based around some very simple core concepts, which by their simplicity are able to remain stable. There is now a whole ecosystem of React clones, which our projects can switch between fairly painlessly to compare performance etc. Some claim greater speed, or smaller downloads. It's entirely possible that React will be supplanted by one of its clones.

It's also possible that Angular will include just such a clone in a future version and recommend it as the preferred UI layer.

That much is risky speculation. But what is more solid fact is that "Angular" could only appear dominant if you combine together the historical abandoned version 1 with the struggling version 2+. If you separate them out, and then (unavoidably) discard version 1 as a dead end, you are left with React as the runaway winner.

Another big influence you miss here is static typing. Angular 2 being written in TypeScript is a big positive. But here React has achieved quite a coup: TypeScript has built-in support for JSX. This means that UI templates are also statically typed. It actually means that TypeScript provides more explicit support for React (and its many clones) than for any other pattern.

I suspect this will become a sticking point for Vue. It is frankly a blast from the past - similar in feel to Knockout.JS and Angular 1. The view is defined in HTML extended with its own vocabulary of directives. So I suspect it is attracting Angular 1 refugees. It seems unlikely to pursade many React users however, because of the huge advantage React derives from its "everything is JavaScript" approach. No need for HTML directives for looping/logic. And templating integrates with modern JavaScript's modular name spacing and TypeScript's static checking and intelligence. React's approach creates a virtuous circle of mutually supportive elements.

The one fly in React's ointment is the dubious philosophical directions it was originally associated with: the nebulous "Flux Architecture", which should never have seen the light of day, and evaporated into nothing almost immediately. What remains is Redux, which is a boilerplate-heavy challenge for beginners. Fortunately with the appearance of MobX (reminiscent of Knockout's data modelling, and also Kendo's 'observable' stuff, but a much more perfect implementation!), we now have a more sane set of options, and of course React itself is not tied to any of these.

My workplace recently purchased the full Telerik UI suite that covers all platforms, and are using Kendo a little bit as part of that. But we are doing so within React, wrapping components using the low-level API defined in your .d.ts file.
