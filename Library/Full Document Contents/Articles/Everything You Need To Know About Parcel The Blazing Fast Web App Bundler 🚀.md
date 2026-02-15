# Everything You Need To Know About Parcel: The Blazing Fast Web App Bundler 🚀

![rw-book-cover](https://cdn-media-1.freecodecamp.org/images/1*QrpgyDDba-3XhpRIDiPx-Q.png)

## Metadata
- Author: [[Michael Ozoemena]]
- Full Title: Everything You Need To Know About Parcel: The Blazing Fast Web App Bundler 🚀
- Category: #articles
- Publication date: 2018-07-31
- URL: https://medium.freecodecamp.org/all-you-need-to-know-about-parcel-dbe151b70082
- Summary: Parcel is a fast web app bundler that works without any setup. This article shows how to use Parcel to bundle a simple React app from scratch. Parcel also has features like a dev-server and hot module replacement for easy development.

# Notes

[[Michael Ozoemena - Everything You Need To Know About Parcel: The Blazing Fast Web App Bundler 🚀]]

***

###### **What’s Parcel?**

[Parcel](https://parceljs.org/) is a web application bundler which offers a blazingly fast performance utilizing multicore processing and requires zero configuration.

So like Webpack? Yes, like Webpack, but lighter and with no configuration required.

###### **What this article offers.**

In this article, I’ll show you how you can make use of Parcel to bundle your basic React.js app built with JavaScript (ES6), HTML, and CSS. We will be creating a React.js app from “scratch” without using tools like `create-react-app` or anything like that.

##### **Getting started.**

The first thing we need to do is set up our project. I have created some starter files on GitHub, and you can see them [here](https://github.com/THEozmic/getting-started-parcel). When you have the project cloned on your computer, run `git checkout beginning` and `npm install` to put things in a “starter” position (note that at this point, the project doesn’t really work because we don’t have any bundled files yet).

##### **Bundling the files.**

Now, we have a simple `express` server set up to serve files from the `dist/` folder. The reason you see `cannot GET /` when you run `npm start` and go to `localhost:5000/` is because no build has happened yet. As such, the `dist/` folder is empty/non-existent.

In order to start bundling our files and have something show up when you go to `localhost:5000/`, we need to do a few things:

1. Install Parcel by running `npm install parcel-bundler --save`.
2. Create build scripts.
3. Run the build scripts and start our server.
4. Load up `localhost:5000/` in the browser.

##### **Creating build scripts and bundling files.**

Before we move into actually creating the build scripts and adding it to our `package.json` file, let’s learn a bit more about bundling files.

**Note** that the `parcel` command will not work if you only have `parcel` installed in your project’s `node_modules` folder and not globally using the `-g` flag.

A nice feature that comes with Parcel (aside from other amazing stuff) is the in-built `dev-server` with [hot module replacement](https://parceljs.org/hmr.html). You can simply make use of this `dev-server` by running `parcel index.html` where `index.html` is your entry HTML file.

Unfortunately, we won’t be utilizing the `dev-server` feature in our demo project, because we’ve built our own little `express` server, but this doesn’t mean we won’t still have `hot module replacement`. Feel free to give the `dev-server` a spin on your own time.

The commands we want to use instead are:

* `parcel watch index.html` to build our files into `dist/` folder and to “watch” for changes to our files during development mode, and
* `parcel build index.html` to just build our files and dump them into `dist/` folder (useful for production mode).

If we had run `npm install parcel-bundler -g` instead of `npm install parcel-bundler --save`, then the commands in the previous paragraphs will run smoothly — but we didn’t. We installed Parcel into our local `node_modules` folder, so instead of running `parcel index.html`, we’ll run `./node_modules/.bin/parcel index.html` to get our files bundled.

Now that we’ve learned all that, we can proceed to editing our `package.json` file and adding our build scripts into it.

```
"scripts": {
    "parcel:dev": "./node_modules/.bin/parcel index.html",
    "parcel:watch": "./node_modules/.bin/parcel watch index.html",
    "parcel:build": "./node_modules/.bin/parcel build index.html"
  }
```

As you can see, I have created three `npm scripts`. Now, when we run `npm run parcel:watch` we will have our files built into the `dist/` folder. We’ll also have Parcel watching out for the changes we make as we edit our CSS, HTML, and JavaScript files so it’ll hot-reload the page for us.

##### **Viewing the results.**

In order to view our simple React.js app in the browser, we can run `npm start` (an already existing script) to start our `express` server, which should then be listening to `localhost:5000/`.

###### **Key things to take away.**

1. You can get up and running with Parcel with absolutely zero configurations. All you have to do is install it and run the commands.
2. Parcel is suitable for both development and production modes.
3. Parcel has an in-built `dev-server` and `hot module replacement` to help you quickly get moving.
4. There’s more to Parcel than what’s in this article, so be sure to look at the [documentation](https://parceljs.org/) to get more in-depth.

I hope you enjoyed it. If you did, don’t forget to leave a comment and a few claps.

If this article was helpful, .

Learn to code for free. freeCodeCamp's open source curriculum has helped more than 40,000 people get jobs as developers. [Get started](https://www.freecodecamp.org/learn/)
