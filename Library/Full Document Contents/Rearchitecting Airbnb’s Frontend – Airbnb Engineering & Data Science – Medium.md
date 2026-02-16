---
Author: Adam Neary
Full Title: Rearchitecting Airbnb’s Frontend
Category:
Publication date: 2017-05-16
URL: https://medium.com/airbnb-engineering/rearchitecting-airbnbs-frontend-5e213efc24d2
Summary: Airbnb moved from Rails-served pages to a React stack that server-renders and routes in the browser. They use code-splitting, lazy loading, async components, and Redux for globals while keeping local state for fast inputs. This cut bundle size, removed legacy libraries, and made pages much faster and more mobile-friendly.
Publication: Airbnb Engineering & Data Science
---

![rw-book-cover](https://miro.medium.com/freeze/max/1200/1*VMRwDmHVeYC3YnJhhtKn4Q.gif)

# Notes

[[Adam Neary - Rearchitecting Airbnb’s Frontend – Airbnb Engineering & Data Science – Medium]]

***

Overview: We recently rethought the architecture for the JavaScript side of our codebase at Airbnb. This post will look at (1) the product drivers that precipitated the changes, (2) the steps we took to move away from our legacy Rails solutions, and (3) some of the key pillars of the new stack. *Bonus: We’ll talk about what’s next!*

Airbnb sees more than 75 million searches each day, which makes the search page our highest traffic page. For nearly ten years, engineers have evolved, enhanced, and optimized the way that Rails delivers the page.

Recently, we moved into verticals beyond Homes, [introducing Experiences and Places](https://www.airbnb.com/new). As a part of bringing these new products to web, we took the time to rethink the search experience itself.

![](https://miro.medium.com/max/700/1*VMRwDmHVeYC3YnJhhtKn4Q.gif)Transitioning between routes for a broad search
Rather than navigating from our landing page at [www.airbnb.com](http://www.airbnb.com) (1) to a search results page (2) to a single listing (3) to the booking flow (4)— *each page delivered standalone via Rails* — we want the user experience to be fluid, adjusting what the user is experiencing as they explore and narrow their search.

![](https://miro.medium.com/max/700/1*epBwi0kxrcW5a6Wv-T4rSg.gif)Designs exploring search from three states: New User, Returning User, and Marketing Marquee
Navigating across tabs and interacting with listings should feel luxurious and effortless. In fact, today there is nothing stopping us from delivering an experience on par with native applications on small and medium screens.

![](https://miro.medium.com/max/700/1*y_gKoEDVvBvJpGq7hfcr_g.gif)Future concept for navigating between tabs, considering async-loaded of content
To tee up this type of experience, we needed to break free of the legacy page-by-page approach that got us here, and in the end we wound up with a fundamental rearchitecting of our Frontend code. [Leland Richardson](https://medium.com/u/41a8b1601c59?source=post_page-----5e213efc24d2--------------------------------) [recently spoke at React Conf about React Native in the “brownfield” of an existing, high traffic native application](https://www.youtube.com/watch?v=tWitQoPgs8w). This article will examine how we undertook a dramatic upgrade with similar constraints, but on the web. Hopefully you find it useful if you find yourself in a similar place!

### Breaking Free from Rails

Before firing up the barbecue for all the fun [Progressive Web App](https://developers.google.com/web/progressive-web-apps/) work on our roadmap, we needed to separate from Rails (or at least the way we use Rails at Airbnb in delivering standalone pages).

Unfortunately, only a matter of months ago, our search page contained some very old code…like, *Lord of the Rings*, touch-that-at-your-peril old. Fun fact: I once replaced a small [Handlebars](http://handlebarsjs.com/) template backed by a Rails presenter with a simple React component, and suddenly things were breaking in entirely separate parts of the page — even in our API response! Turns out, the presenter was mutating the backing Rails model, which had been impacting all downstream data for years, even when the UI wasn’t being rendered.

In short, we were in this project like Indiana Jones swapping the idol for a bag of sand, and immediately the temple starts collapsing, and we’re running from a boulder.

#### Step 1: Aligning on API Data

When Rails is server-rendering your page, you can get away with throwing data at your server-rendered React components any way you like. Controllers, helpers, and presenters can produce data of any shape, and even as you migrate sections of the page to React, each component can consume whatever data it requires.

But once you endeavor to render the route client-side, you need to be able to request the data you need dynamically and in a predetermined shape. In the future, we may crack this problem with something like [GraphQL](http://graphql.org/), but let’s set that aside for now, as it wasn’t an option when this refactor took place. Rather, we chose to align on a “v2” of our API, and we needed all our components to begin consuming that canonical data shape.

If you find yourself in similar waters with a large application, you might find as we did that planning for the migration of existing server-side data plumbing was the easy part. Simply step through any place Rails is rendering a React component, and ensure that data inputs are API shapes. You can further validate compliance with API V2 shapes used as React PropTypes on the client.

The tricky bit for us was working with all the teams who interact with the guest booking flow: our Business Travel, Growth, and Vacation Rentals teams; our China and India market-specific teams, Disaster Recovery…the list goes on, and we needed to reeducate all these folks that even though it was technically possible to pass data directly to the component being rendered (“yes, I understand it’s just an experiment, but…”), *all data* needs to go through the API.

#### Step 2: Non-API Data: Config, Experiments, Phrases, L10n, I18n…

There is a separate class of data from what we would think of as API data, and it includes application config, user-specific experiment assignment, internationalization, localization, and similar concerns. Over the years, Airbnb has built up some incredible tooling to support all these functions, but the mechanisms for delivering them to the Frontend were a bit under-baked (or possibly fully-baked when built, before the ground began shifting under foot!).

We use [Hypernova](https://www.npmjs.com/package/hypernova) to server-render React, but before we went deep on this refactor, it was a bit nebulous whether experiment delivery in a React component would blow up during server-rendering or if string translations available on the client would all be reliably available on the server. Critically, if the server and client output don’t match to the bit, the page not only flashes the diff but also re-renders the entire page after load, which is terrible for performance.

Worse yet, we had some magical Rails functions written long ago, for instance `add_bootstrap_data(key, value)`, which could ostensibly be called anywhere in Rails to make data available on the client globally via `BootstrapData.get(key)`(though, again, not necessarily for Hypernova). What began as a helpful utility for a small team became a source of untraceable witchcraft for a large application and team. The “data laundering” crimes became increasingly tricky to unwind, as each team owns a different page or feature, and therefore each team cultivated a different mechanism for loading config, each suiting their unique needs.

Clearly, this was already breaking down, so we converged on a canonical mechanism for bootstrapping non-API data, and we began migrating all apps/pages to this handoff between Rails and React/Hypernova.

Some content could not be imported from the original document. [View content ↗](https://medium.com/airbnb-engineering/rearchitecting-airbnbs-frontend-5e213efc24d2) 

This higher order component does two very important things:

1. It receives a canonical shape of bootstrap data as a Plain Old JavaScript Object, and initializes all the supporting tooling correctly both for server-rendering and client-rendering identically.
2. It swallows everything except `bootstrapData`, another simple object which we expect `<App>`to load into Redux to be used by children as needed (in place of `BootstrapData.get`).

In a single shot, we eliminated `add_bootstrap_data` and prevented engineers from passing arbitrary keys through to top level React components. Order was restored to the shire, and before long we were navigating to routes dynamically in the client and rendering content of material complexity without Rails to prop it up (pun intended).

### Super-Charging the Frontend

Server rework in hand, we now turn our gaze to the client.

#### The Lazy-Loaded Single Page App

Gone are the days, friends, of the monster Single Page App (SPA) with a gruesome loading spinner on initialization. This dreaded loading spinner was the objection many folks raised when we pitched the idea of client-side routing with React Router.

![](https://miro.medium.com/max/700/1*O2fK16vfyWaDT-IR61drPw.png)Lazy loading of route bundles in the Chrome Timeline
But if you look above, you’ll see the impact of [code-splitting](https://webpack.github.io/docs/code-splitting.html) and [lazy-loading](https://webpack.js.org/guides/lazy-load-react/) bundles by route. In essence, we server render the page and deliver just the bare minimum JavaScript required to make it interactive in the browser, then we begin proactively downloading the rest when the browser is idle.

On the Rails side, we have one controller for all routes delivered via the SPA. Each action is simply responsible for (1) making whatever API request the client would have made on client-side navigation, then (2) bootstrapping that data to Hypernova along with config. We went from thousands of lines of Ruby code per action (between the controller, helpers, and presenters) down to ~20–30 lines. Yahtzee.

But it’s not just code that is noticeably different…

![](https://miro.medium.com/max/700/1*EpKNHdS4Xzl9fRdGekUgEA.gif)Side-by-side comparison fetching Homes for Tokyo: Legacy page load vs client-side routing (4–5x difference)
…now transitions between routes are smooth as butter and a step change (~5x) faster, and we can break ground on the animations featured at the beginning of this post.

#### AsyncComponent

Prior to React, we would render an entire page at a time, and this practice carried over into our early React days. But we use an AsyncComponent similar to [this](https://medium.com/@thejameskyle/react-loadable-2674c59de178) as a way to load sections of the component hierarchy after mount.

Some content could not be imported from the original document. [View content ↗](https://medium.com/airbnb-engineering/rearchitecting-airbnbs-frontend-5e213efc24d2) 

This is particularly useful for heavy elements that aren’t initially visible, like Modals and Panels. Our explicit goal is to ship precisely the JavaScript required to initially render the visible portion of the page and make it interactive, not one line more. This has also meant that if, for example, teams want to use D3 for a chart in a modal on a page that doesn’t otherwise use D3, they can weigh the “cost” of downloading that library as part of their modal code in isolation from the rest of the page.

Best of all, it is this simple to use anywhere it is needed:

Some content could not be imported from the original document. [View content ↗](https://medium.com/airbnb-engineering/rearchitecting-airbnbs-frontend-5e213efc24d2) 

Here we can simply swap out the synchronous version of our map for an async version, which is particularly useful on small breakpoint, where the map is displayed via user interaction with a button. Since most of these users are on phones, getting them to interactive before worrying about Google Maps comes with a tasty boost in page load time.

Also, note the `scheduleAsyncLoad()` utility, which requests the bundle in advance of user interaction. Since the map is so frequently used, we don’t need to wait for user interaction to request it. Instead, we can enqueue it when you get to the Homes Search route. If the user does request it prior to download, they see a reasonable `<Loader />` until the component is available. No sweat.

The final benefit of this approach is that `HomesSearch_Map` becomes a named bundle that the browser can cache. As we disaggregate larger route-based bundles, the slowly-changing sections of the app remain untouched across updates, further saving JavaScript download time.

#### Building Accessibility into our Design Language

Doubtless it warrants a dedicated post, but we have begun building our internal component library with Accessibility enforced as a hard constraint. In the coming months, we will have replaced all UI across the guest flow that is compatible with screen readers.

Some content could not be imported from the original document. [View content ↗](https://medium.com/airbnb-engineering/rearchitecting-airbnbs-frontend-5e213efc24d2) 

The UI is rich enough that we want to associate a CheckBox not only with a title, but also a subtitle using `aria-describedby`. To achieve this requires a unique identifier in the DOM, which means enforcing a required ID as a prop that any calling parents need to provide. These are the types of hard constraints the UI can impose to ensure that if a component is used in the product, it is delivered with accessibility built in.

The code above also demonstrates our responsive utilities HideAt and ShowAt, which allow us to dramatically alter what the user experiences at different screen sizes without having to hide and show using CSS. This leads to much leaner pages.

#### Getting Surgical and Philosophical about State

No Frontend post would be complete without touching on the debate about how to handle app state.

We use Redux for all API data and “globals” like authentication state and experiment configurations. Personally, I like [redux-pack](https://github.com/lelandrichardson/redux-pack) for async. Your mileage may vary.

However, with all the complexity on the page—particularly around Search—it doesn’t work to use Redux for low-level user interactions like form elements. We found that no matter how we optimized, the Redux loop was going to make typing in inputs feel inadequately responsive.

![](https://miro.medium.com/max/669/1*12LgecpKz8HA2e2evkYacw.png)Our Room Type Filter (code featured above)
So we use component local state for everything the user does up until it triggers a route change or network interaction, and we haven’t had any problems.

At the same time, I like the feel of a Redux container component, and we found that even with local state, we could build Higher Order Components that could be shared. A great example is with our filters. Search for [homes in Detroit](https://www.airbnb.com/s/Detroit--MI--United-States/homes), and you’ll find a few different panels on the page, each operating independently, that can modify your search. Across various breakpoints, there are actually dozens of components that need to know the currently-applied search filters and how to update them, both temporarily during user interaction and officially once accepted by the user.

Some content could not be imported from the original document. [View content ↗](https://medium.com/airbnb-engineering/rearchitecting-airbnbs-frontend-5e213efc24d2) 

Here we have a neat trick. Every component that needs to interact with filters can be wrapped with this HOC, and you’re done. It even comes with prop types. Each component wires into the *responseFilters* (those associated with the currently-displayed results) from Redux but keeps a local stagedFilters object available for modification.

By tackling state this way, interacting with our Price Slider has no impact on the rest of the page, so performance is great. But all filters panels are implemented with the same function signatures, so development is simple.

### What’s Next?

Now that the grizzly legwork of catching the Frontend up with the present is largely in hand, we can turn our attention to the future.

* [AMP](https://www.ampproject.org/) versions of all pages in the core booking flow will lead to sub-second (in some cases) *Time To Interactive* from Google search on mobile web, and many of the the changes required to get there will drive dramatic improvements in P50/P90/P95 cold load times across mobile web and desktop web alike.
* [PWA](https://developers.google.com/web/progressive-web-apps/) functionality will lead to sub-second (in some cases) *Time To Interactive* for returning visitors and will open the door to offline-first functionality so very critical to users with flaky connections.
* Dropping the final hammer on legacy tech/frameworks will cut bundle sizes in half. It’s not flashy work, but finally ripping out jQuery, Alt, Bootstrap, Underscore, and all external CSS requests (they block rendering, and 97% of the rules are unused!) will streamline not only the code we ship, but also the footprint of what new hires need to learn as they ramp up.
* Finally, the yeoman’s work of manually bird-dogging rendering bottlenecks, async-loading code not visible at initial render, avoiding unnecessary re-renders, and reducing the cost of re-renders. These improvements are the difference between a clunky feeling app and a well-oiled machine.

Tune in next time as we chase down these opportunities. Since so many of the wins will have immediate quantitative impact, we will try to capture some of the specific wins in subsequent posts.

*Naturally, if you enjoyed reading this and thought this was an interesting challenge, we are always looking for talented, curious people to* [*join the team*](https://www.airbnb.com/careers/departments/engineering)*. Or, if you just want to talk shop, hit me up on twitter any time* [*@adamrneary*](https://twitter.com/AdamRNeary)

Finally, huge props to [Salih Abdul-Karim](https://twitter.com/therealsalih) and [Hugo Ahlberg](https://twitter.com/hugoahlberg), the experience designers behind the face-melting animations I still can’t stop ogling. The list of many engineers deserving kudos for their role in this effort is indescribably long but most certainly includes Nick Sorrentino, [Joe Lencioni](https://medium.com/u/e52389684329?source=post_page-----5e213efc24d2--------------------------------) , [Michael Landau](https://medium.com/u/5fe7db9fe60f?source=post_page-----5e213efc24d2--------------------------------) , Jack Zhang, Walker Henderson, and Nico Moschopoulos.
