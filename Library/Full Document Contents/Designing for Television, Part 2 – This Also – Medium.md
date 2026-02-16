---
Author: Molly Lafferty
Full Title: Designing for Television, Part 2
Category:
Publication date: 2016-09-01
URL: https://medium.com/this-also/designing-for-television-part-2-f31793e7d2e9#.q7av36xxl
Summary: Prototyping the user interface
---

![rw-book-cover](https://miro.medium.com/v2/resize:fit:1200/1*Q1_NbXfSFpYQoTBUAHkvvw.png)

# Notes

[[Molly Lafferty - Designing for Television, Part 2 – This Also – Medium]]

***

#### Prototyping the user interface

***With*** [***Samir Zahran***](https://medium.com/@SamIAre) ***and*** [***Michelle Cruz***](https://medium.com/@mscruz)

![](https://miro.medium.com/v2/resize:fit:2000/1*Q1_NbXfSFpYQoTBUAHkvvw.png)
In [Part 1](https://medium.com/this-also/designing-for-television-part-1-54508432830f#.v7wuukcav), we dug into the basic ingredients of a TV UI. Now that we have [our mockup](https://www.dropbox.com/s/s5cjvi6z5d6w1qb/TV_UI_Template.psd?dl=0), we’re ready to start the most important part of the product design process: prototyping.

A successful product is measured by its use, so the sooner we can *feel* the experience, the more we can refine the final product. This is especially true for television interfaces. We’ll need to test navigation patterns, ensure everything on screen is legible, refine the cursor movement, and make the entire experience feel natural and responsive.

This article will give you a high-level overview of the core functions of our prototype so that you’ll be able to start testing and customizing code right away. We’ll also discuss some strategies and approaches to advanced topics, like cursor behavior and motion design.

### Getting Started

*What you’ll need to get started*

The best prototypes are fast and cheap. Developing a native application for any connected television device is costly, both in time and money. With just some basic web technology and a few pieces of hardware, we can quickly emulate a 10-foot television experience and start making progress.

#### **Grab a browser**

We used Chrome, but Firefox and Edge are also compatible.

#### Grab a controller

For this interface, you’ll need a game controller. Make sure it’s compatible with one of the [core libraries](https://github.com/samiare/Controller.js/wiki/Controller%20Layouts#layouts), and keep in mind that some controllers may need drivers installed.

#### Connect to an HDTV

Connect your computer or laptop to an HDTV using an HDMI cable. Try to set up a 10-ft viewing distance, since that’ll be the benchmark for testing a good user experience.

#### Download the prototype

[Download your own copy of the project directory](https://www.dropbox.com/sh/v0ssndg41gktslp/AACOEiQZIRUHLxSNUdR_ET5Pa?dl=0). To view the prototype, open the *prototype/index.html* file in a browser.

![](https://miro.medium.com/v2/resize:fit:700/1*CYoY03kqglRSI9atIx_A3g.png)
This folder will look familiar if you have experience with web development. Here are the key javascript files:

* **js/app.js**: The prototype application logic
* **js/data.js**: The data that’s injected into the UI
* **js/libraries/Controller.min.js**: Our custom Gamepad API library
* **js/libraries/Controller.layouts.min.js**: An add-on for compatibility with more gamepads

#### Test the controller

Lastly, you’ll want to confirm that your controller is working with one of the core libraries, [Controller.js](https://github.com/samiare/Controller.js). To test it, first go [here](https://samiare.github.io/Controller.js/). Then, plug your controller into your computer’s USB port.

![](https://miro.medium.com/v2/resize:fit:700/1*Tbcdi43h-QuqjlmcwcuL-g.gif)
Try pressing some buttons and moving the analog sticks to see the values change. If you aren’t seeing any information, or the values aren’t changing, check out the [compatibility information](https://github.com/samiare/Controller.js) on GitHub.

### Basic Framework

*Let’s get organized and build just enough to test & tweak*

Our prototype has been structured like an app, organized into several files that each have a specific purpose. We’ve avoided using major frameworks or sophisticated architectural patterns to make a prototype that’s lightweight, flexible, and accessible to a larger audience. If we keep it quick and scrappy, there’s a lot less pain in redesigning and rebuilding our ideas.

#### app.js

Most of the prototype’s logic is located in *app.js.* The most important part of this file is our main object, *Example*, which provides a context for all of the different functions and variables that we’ll need to build the interface. After we declare all of our key functions inside this object, we’ll call *Example.load()* to begin the execution of our code.

```
var Example = {  
    data: prototypeCarousel,  
    container: document.getElementById('prototype'),  
    column: document.getElementById('prototype-column'),  
    ...  
}Example.load();
```

#### data.js

It’s good practice to separate your data from your app’s logic. This makes it a lot easier to test, tweak, and calibrate your interface as you use it. We’ve stored the metadata for our design’s content, the carousel, in *data.js*:

```
var prototypeRow = {  
  title: 'Row',  
  content: [  
    {  
      eyebrow: '18PX eyebrow 1',  
      title: 'Show Title 92px 1',  
      summary: 'Lorem ipsum dolor sit'  
    },  
    {  
      eyebrow: '...',  
      title: '...',  
      summary: '...'  
    },  
    {...} // etc.  
  ]  
}
```

### Mapping Button Presses

*Organizing all of our buttons and events in one place*

The navigation handler is a function that we use to make a clear, organized list of what buttons we’d like to map on the game controller. The switch/case structure used here makes it easy to update and test different configurations on the fly.

```
navigationHandler: function(event) {  
    var button = event.detail.button;    switch(button) {  
        case 'DPAD_RIGHT':  
            Example.dpadHandler(button);  
            break;  
        ...  
    }  
}
```

Ultimately, the navigation handler is an organizational tool that will allow us to identify which button was pressed, call the desired function, and pass along the name of the button that was pressed.

One thing you can experiment with on your own is how to map the analog sticks. Unlike input from the direction pad, analog sticks have a wide range of motion, are pressure sensitive, *and* support a click event. This makes for lot of interesting possibilities for interaction design—after all, game controllers are made to enable diversity in game mechanics.

In this example, we’ve kept the analog sticks as simple as possible. A feature in Controller.js allows us to read analog stick events as if they were the same input from a direction pad. A *threshold* value can be set to control how far the user should push the analog stick before it fires a direction event.

### Handling Direction Events

*A simple example that handles interactions with the direction pad*

All we’ve done so far in our prototype is map buttons to functions. What we really need to test our design is a function that moves our user’s cursor on the screen. Initially, this sounds pretty easy, but in reality, there are a lot of important details we want to build and test.

We’ll use the *dpadHandler* function to organize the overall series of events that should take place when the user presses the d-pad. First, it will be tracking the highlighted screen element with the use of the variable *index*. It has to support cases where our user’s cursor is on the first item in the grid, some arbitrary item in the middle, or at the end.

The *if* statements will handle the logic that enables the carousel, and the *changeCursor* function will visibly move our user’s cursor.

```
dpadHandler: function(button) {  
    var index = Example.column.getAttribute('data-selection');  
    var selection = document.getElementById('selected');    if (button === 'DPAD_RIGHT') {  
        if (index < Example.data.content.length - 1) {  
            **index++;**  
            **Example.scroll('next');**  
            **Example.changeCursor('next', index);**  
        } else {  
            Example.scrollOverflow(Example.container, 'post');  
        }  
    } else { ... }  
}
```

One of the big design details you can experiment with is the *scroll* function. If you have paid close attention to television apps, it can become frustrating to press a direction button each and every time you’d like to move the cursor from one element to the next.

The solution in many products is to enable a long press on the direction pad. Unlike mobile, the long press is a very natural and commonly used interaction for televisions. To build a long press event, we have to track the time a user first pressed a directional button and routinely check to see if the user has held the button long enough to pass a fixed threshold.

You can look at how we’ve implemented the long press in the *scroll* method. This function implements all of these design details and gives you a good starting place to experiment. To stop the cursor after a long press, we map the release event of the direction pad to *dpadDirectionHold*.

### Strategies for Cursor Behavior

*Building a logical system for the cursor to jump around*

Television interfaces have a unique interaction model where selections are statically locked to visible elements on the screen. The user sees this as their cursor, which is generally an outline or highlight around their selected item. Building cursor functionality can become quite challenging. Here are a few approaches for different designs.

#### One Dimensional List

In our example, we have a 1-dimensional layout: the selection can only move horizontally. Because of this, **list traversal** was the easiest method. Every item has an index, and you simply keep track of the current index and increment or decrement the index when the user moves the cursor.

#### Two Dimensional Grid

If your interface is a static grid of uniformly sized items, you can use a method similar to list traversal, but modify it to store two indexes: one for the row and another for the column. Remember, keep it scrappy.

#### Advanced Layouts

Sophisticated interfaces won’t always come in the form of a list or grid. For example, designing a radial menu on televisions is a reasonable concept given that analog sticks naturally map to a radial layout. A good strategy for enabling this type of advanced cursor movement is to perform **collision detection**.

The idea behind collision detection is simple. If you have an interface with several interactive elements on the screen, you can take the desired direction of movement from the user’s controller and *search* in that direction for a selectable interface element. If you’ve ever played [Asteroids](http://www.freeasteroids.org/), you’ve seen collision detection in action.

The most robust method for collision detection that we’ve used on projects is **ray casting**. With ray casting, you start with the current selection’s position and draw a line in the user’s desired direction of movement until the ray collides with an interface element. Then you move the cursor to that item. Check out this [example](http://jsfiddle.net/nLMTW/) from JSFiddle for more.

#### Other Considerations

It’s easy to get lost in the technical complexities of list traversal or ray casting, but keep in mind a few more cursor behaviors that users will still need the interface to account for:

* When and how will you move the cursor to scroll the screen? Do you keep the cursor in the center of the screen, or wait for it to leave the bounds of the interface before scrolling more content into view?
* What happens if there is nothing selectable in the direction the user presses? Does the selection wrap to the beginning, or the next row?
* Is there a need for advanced shortcuts to cut down on repetitive button mashing? For game controllers, will you use the triggers or bumpers?

### Creating natural movement

*Animating interaction on large format interfaces*

With the logic of the cursor movement figured out, it’s time to add animation to create a natural and responsive interface. Like type and color, it’s essential to study and finesse animations on a TV. Fun, lively animations that look great on a phone or laptop can become overwhelming and distracting on a large display.

And while it’s often argued that motion adds personality and polish (see Apple TV’s [satisfying parallax motion](https://www.youtube.com/watch?v=o2eYaVoseto)), it’s better to err on the side of “less is more.” Subtle, well-timed animations will make a more polished experience than fancy, unnecessary flourishes.

While our cursor could simply jump from one item to the next, animating the cursor in and out as we move between items creates a single, cohesive movement that aligns with the user’s single button press.

![](https://miro.medium.com/v2/resize:fit:700/1*ktdltEw1IIqrVOD5JDesdg.gif)
Easing curves are essential when prototyping interface motion. Very rarely do objects in real life move at a constant speed from start to stop. To define our motion experience, we began testing several [standard bezier curves](http://easings.net/). We slowly adjusted these curves and the animation’s duration until cursor movement felt fast, fluid, and responsive.

Generally, your [UI animations should be between 200ms to 500ms long](http://valhead.com/2016/05/05/how-fast-should-your-ui-animations-be/). However, even if the animation duration is short, applying the wrong curve can exaggerate the length of the transition and make controller input feel slow and laggy. Many users of gaming consoles expect the fast, instant response of a controller they regularly experience in video games.

Motion can also provide users with hints about where they can and can’t navigate. At the end of an ordered list, a slight bump can indicate that a user has reached the last item and can no longer navigate further to the right.

![](https://miro.medium.com/v2/resize:fit:700/1*r923OhqX-BjTd4PZn9e62A.gif)
You can read more about interface motion in the [tvOS User Interaction Guidelines](https://developer.apple.com/tvos/human-interface-guidelines/user-interaction/#focus-and-selection) and [Google’s excellent Material Motion guide](https://material.google.com/motion/material-motion.html).

### Get Ready for Part Three

*Developing our custom Gamepad API library*

You may have noticed that [Controller.js](https://github.com/samiare/Controller.js) is just a library built on top of the [Gamepad API](https://developer.mozilla.org/en-US/docs/Web/API/Gamepad_API/Using_the_Gamepad_API). So why not just use that API? While it’s a great tool that got us using a controller with our designs quickly, it was built for gaming and didn’t quite have the functionality or ease of use we wanted for building interactivity in UIs. In Part 3, we’ll explain more about the process of building and using the library.
