# All You Need To Know About Parcel 🚀 – freeCodeCamp

![rw-book-cover](https://miro.medium.com/max/1200/1*Gjhk6qvPM5zAy1iPPS1ttg.png)

## Metadata
- Author: [[Trevor-Indrek Lasn]]
- Full Title: All You Need To Know About Parcel 🚀 – freeCodeCamp
- Category: #articles
- Publication date: 2019-07-31
- URL: https://medium.com/@wesharehoodies/all-you-need-to-know-about-parcel-dbe151b70082
- Summary: https://medium.com/@wesharehoodies/all-you-need-to-know-about-parcel-dbe151b70082

# Notes

[[Trevor-Indrek Lasn - All You Need To Know About Parcel 🚀 – freeCodeCamp]]

***

#### Evolution and innovation with zero configuration

![](https://miro.medium.com/max/680/1*-Tcq85crClCEu_gYzn06gg.gif)
Yet other bundler/build tool? *Really?* Yep — you betcha, evolution and innovation combined brings you [Parcel](https://parceljs.org/)!

![](https://miro.medium.com/max/700/1*Gjhk6qvPM5zAy1iPPS1ttg.png)
### **What’s So Special About Parcel and Why Should I Care?**

While webpack brings a lot of configurability with the cost of complexity — **Parcel brings simplicity**. Parcel brands itself as “zero configuration”.

*Unraveling the above* — Parcel has a development server built-in, out of the box. The development server will automatically rebuild your app as you change files and supports [hot module replacement](https://parceljs.org/hmr.html) for fast development.

### **What Are the Benefits of Parcel?**

* Fast bundle times — Parcel is faster than Webpack, Rollup and Browserify.

![](https://miro.medium.com/max/700/1*jovxixL_dfSEnp9f6r8eEA.png)Parcel benchmarks
Something to consider: Webpack is still awesome and sometimes can be faster.

![](https://miro.medium.com/max/700/1*e9ZNxTRvxQSgAHFIegC-6w.png)[Source](https://github.com/TheLarkInn/bundler-performance-benchmark/blob/master/README.md)
* Parcel has out of the box support for JS, CSS, HTML, file assets, and more — **no plugins needed — More user-friendly.**
* Zero configuration required. Out of the box code splitting, hot module reloading, CSS preprocessors, development server, caching, and many more!
* Friendlier error logs.

![](https://miro.medium.com/max/910/1*miFAZZhZpaloYs1fj3jB0A.png)
![](https://miro.medium.com/max/452/1*2MnJM2-lQHND-icGggt4Ug.png)Parcel error handling
### All Right — So When Should I Use Parcel, Webpack or Rollup?

It’s completely up to you but I personally would use each bundler in the following situations:

***Parcel*** — Small to medium sized projects (<15k lines of code)

***Webpack*** — Large to enterprise-sized projects.

***Rollup*** — For NPM packages.

*Let’s give Parcel a shot!*

### Installation is Fairly Straight-Forward

We installed the [parcel-bundler npm package](https://www.npmjs.com/package/parcel-bundler) locally. Now we need to initialize a node project:

![](https://miro.medium.com/max/700/1*ncsWSVcZ9H2GvCryk1bjbw.png)
Next, create `index.html` and `index.js` file.

![](https://miro.medium.com/max/700/1*42o-xydISJg7RFPJEV8vXQ.png)
Let’s connect our `index.html` and `index.js`

![](https://miro.medium.com/max/1252/1*mnvGwOAj77U0ukki4s4LZQ.png)
![](https://miro.medium.com/max/760/1*0SsOP82bxYkYIt-H9XL8Zw.png)
Finally, add parcel script to our `package.json`:

![](https://miro.medium.com/max/666/1*n3Al1gXiv4tNNGo3pWc-ug.png)
That’s all there is to configuration — amazing time saver!

Next up — let’s start our server.

![](https://miro.medium.com/max/634/1*Yq8tQPP6Qv80xwV3N-1lIw.gif)
![](https://miro.medium.com/max/1120/1*tWzj5lTbPm2rEZKndCgKhQ.png)
Work like a charm! Notice the build times.

![](https://miro.medium.com/max/700/1*6PKBaYyEQrK889opDE72Vg.png)
***15ms?!*** Wow — that’s blazing fast indeed!

How’s the HMR?

![](https://miro.medium.com/max/700/1*KHATEDXNqL5fshf3S0B5Zw.gif)
Also feels very fast.

### SCSS

![](https://miro.medium.com/max/427/1*dMNikHR10Nfw1Z0PtmITXA.png)
All we need is the `node-sass` package and we’re good to go!

Next up, add some styling and import the `styles.scss` to `index.js`:

![](https://miro.medium.com/max/836/1*lhF1lxmw4RQNyTpI1Y1Hdw.png)
![](https://miro.medium.com/max/868/1*SSv27gQ34310LyJBHqo8ZQ.png)
![](https://miro.medium.com/max/700/1*r8zgxebzyd-KV7LU63qfww.png)
### Production Build

All we need is to add a `build` script to our `package.json`:

![](https://miro.medium.com/max/700/1*BbfYCV5-PaFwDX_Y68oXgw.png)
Running our build script:

![](https://miro.medium.com/max/420/1*bPzZxDj7qAwfMFkPBy44Ow.gif)
See how easy Parcel makes our lives?

![](https://miro.medium.com/max/420/1*TVPM_3Zm60KkLxnhdDVMOQ.png)
You can specify a specific build path like so:

```
parcel build index.js -d build/output
```

### React

![](https://miro.medium.com/max/512/1*6kK9j74vyOmXYm1gN6ARhQ.png)
Setting up react is really simple, all we need to do is install our dependencies and setup our `.babelrc`:

```
npm install *--save react react-dom babel-preset-env babel-preset-react && touch .babelrc*
```

![](https://miro.medium.com/max/678/1*8LV0jtqGPIRN-Z05nZjWZQ.png)
All right, let’s bring out the big guns! Try writing our initial react component yourself before scrolling…

![](https://miro.medium.com/max/1192/1*w6prJQoCeWWClTIGe-2eCg.png)
![](https://miro.medium.com/max/1320/1*JcIe-GLpc9yiNnWauvobrQ.png)
![](https://miro.medium.com/max/700/1*7ME5571Q3BlWNAgFwSGxHg.png)
### Vue

![](https://miro.medium.com/max/225/1*lJPS840gMBZYhHeZ6aop_g.png)
**As requested, here’s the Vue example.**

Start by installing `vue` and `parcel-plugin-vue` — the latter for `.vue` components support.

```
$ npm i --save vue parcel-plugin-vue
```

We need to add our root element, import the vue index file and initialize Vue.

Start by making a vue directory and let’s also create`index.js` and `app.vue`

```
$ mkdir vue && cd vue && touch index.js app.vue
```

Now let's hook our `index.js` and `index.html`:

![](https://miro.medium.com/max/700/1*PJ7L4G15cDpvreu6NkdXLQ.png)
Finally, let’s initialize vue and write our first vue component!

![](https://miro.medium.com/max/1090/1*EHKOgp5Yc69NBCImVJUJcg.png)
![](https://miro.medium.com/max/1150/1*TCyx5wWr5GK1O9E6bKllUA.png)
![](https://miro.medium.com/max/700/1*XDZ71d55e8vGY8QoVeJGlw.png)
Voila! — we have Vue installed with `.vue` support!

### TypeScript

![](https://miro.medium.com/max/496/1*SwI4JNcok6yj8b6a0Mykvg.png)
This one is extremely easy — just install TypeScript and we’re good to go!

```
npm i --save typescript
```

Make a file called `index.ts` and insert it into the `index.html`:

![](https://miro.medium.com/max/1024/1*zp1272l6v1XxLmX8QSndkA.png)
![](https://miro.medium.com/max/1340/1*mR0wngPbI4UfHtMZxletxQ.png)
![](https://miro.medium.com/max/700/1*QpIDy402yydKokM1bO5l7A.png)
#### Good to go!

If you want to take your JavaScript abilities to the next level, I’d recommend reading the “[*You Don’t Know JS*](https://amzn.to/2LSDpG6?source=post_page---------------------------)” book series.

#### Heres the [Source Code](https://github.com/wesharehoodies/parcel-examples-vue-react-ts)!

Thanks for reading! ❤
