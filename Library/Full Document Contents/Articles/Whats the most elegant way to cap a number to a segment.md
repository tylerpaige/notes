# Whats the most elegant way to cap a number to a segment?

![rw-book-cover](https://cdn.sstatic.net/Sites/stackoverflow/Img/apple-touch-icon@2.png?v=73d79a89bded)

## Metadata
- Author: [[Samuel Rossille]]
- Full Title: Whats the most elegant way to cap a number to a segment?
- Category: #articles
- Publication date: 2012-07-10
- URL: https://stackoverflow.com/questions/11409895/whats-the-most-elegant-way-to-cap-a-number-to-a-segment
- Summary: Also, depending on what perf tooling or browser you use you get various outcomes of whether the Math based implementation or ternary based implementation is faster.

# Notes

[[Samuel Rossille - Whats the most elegant way to cap a number to a segment?]]

***

### [Samuel Rossille](https://stackoverflow.com/users/1060205/samuel-rossille)

 (2012-07-10 08:58:53)

Let's say `x`, `a` and `b` are numbers. I need to limit `x` to the bounds of the segment `[a, b]`.

In other words, I need a [clamp function](https://math.stackexchange.com/q/1336636):

```
clamp(x) = max( a, min(x, b) )

```

Can anybody come up with a more readable version of this?

#### accepted answer from [Otto Allmendinger](https://stackoverflow.com/users/92493/otto-allmendinger)

 (2012-07-10 09:02:07)

The way you do it is pretty standard. You can define a utility `clamp` function: 

```
/**
 * Returns a number whose value is limited to the given range.
 *
 * Example: limit the output of this computation to between 0 and 255
 * (x * 255).clamp(0, 255)
 *
 * @param {Number} min The lower boundary of the output range
 * @param {Number} max The upper boundary of the output range
 * @returns A number in the range [min, max]
 * @type Number
 */
Number.prototype.clamp = function(min, max) {
  return Math.min(Math.max(this, min), max);
};

```

(Although extending language built-ins is generally frowned upon)

#### [dweeves](https://stackoverflow.com/users/109814/dweeves)

 (2012-07-10 09:10:27)

a less "Math" oriented approach, but should also work, this way, the `<` / `>` test is exposed (maybe more understandable than minimaxing) but it really depends on what you mean by "readable"

```
function clamp(num, min, max) {
  return num <= min 
    ? min 
    : num >= max 
      ? max 
      : num
}

```

#### [Tomas Nikodym](https://stackoverflow.com/users/6498192/tomas-nikodym)

 (2016-09-13 19:50:41)

There's a proposal to add an addition to the built-in `Math` object to do this:

```
Math.clamp(x, lower, upper)

```

But note that as of today, it's a Stage 2 [proposal](https://rwaldron.github.io/proposal-math-extensions/#sec-math.clamp). Until it gets widely supported (which is not guaranteed), you can use a [polyfill](https://www.npmjs.com/package/ecma-proposal-math-extensions).

#### [CAFxX](https://stackoverflow.com/users/414813/cafxx)

 (2012-07-10 09:03:43)

A simple way would be to use

```
Math.max(min, Math.min(number, max));

```

and you can obviously define a function that wraps this:

```
function clamp(number, min, max) {
  return Math.max(min, Math.min(number, max));
}

```

---

Originally this answer also added the function above to the global `Math` object, but that's a relic from a bygone era so it has been removed (thanks @Aurelio for the suggestion)

#### [Aurelio](https://stackoverflow.com/users/1446845/aurelio)

 (2016-11-03 17:36:10)

This does not want to be a "just-use-a-library" answer but just in case you're using Lodash you can use [`.clamp`](https://lodash.com/docs/4.16.6#clamp):

```
_.clamp(yourInput, lowerBound, upperBound);

```

So that:

```
_.clamp(22, -10, 10); // => 10

```

Here is its implementation, taken from Lodash [source](https://github.com/lodash/lodash/commit/946560c99847b2f600ff204fe01e9e776f05d2ec):

```
/**
 * The base implementation of `_.clamp` which doesn't coerce arguments.
 *
 * @private
 * @param {number} number The number to clamp.
 * @param {number} [lower] The lower bound.
 * @param {number} upper The upper bound.
 * @returns {number} Returns the clamped number.
 */
function baseClamp(number, lower, upper) {
  if (number === number) {
    if (upper !== undefined) {
      number = number <= upper ? number : upper;
    }
    if (lower !== undefined) {
      number = number >= lower ? number : lower;
    }
  }
  return number;
}

```

Also, it's worth noting that Lodash makes single methods available as standalone modules, so in case you need only this method, you can install it without the rest of the library:

```
npm i --save lodash.clamp

```

#### [bingles](https://stackoverflow.com/users/20489/bingles)

 (2017-11-02 20:46:15)

If you are able to use es6 arrow functions, you could also use a partial application approach:

```
const clamp = (min, max) => (value) =>
    value < min ? min : value > max ? max : value;

clamp(2, 9)(8); // 8
clamp(2, 9)(1); // 2
clamp(2, 9)(10); // 9

or

const clamp2to9 = clamp(2, 9);
clamp2to9(8); // 8
clamp2to9(1); // 2
clamp2to9(10); // 9

```

#### [Ricardo](https://stackoverflow.com/users/6209133/ricardo)

 (2018-02-15 17:44:32)

If you don’t want to define any function, writing it like `Math.min(Math.max(x, a), b)` isn’t that bad.

#### [Michael Stramel](https://stackoverflow.com/users/979387/michael-stramel)

 (2020-07-21 04:54:23)

This expands the ternary option into if/else which minified is equivalent to the ternary option but doesn't sacrifice readability.

```
const clamp = (value, min, max) => {
  if (value < min) return min;
  if (value > max) return max;
  return value;
}

```

Minifies to 35b (or 43b if using `function`):

```
const clamp=(c,a,l)=>c<a?a:c>l?l:c;

```

Also, depending on what perf tooling or browser you use you get various outcomes of whether the Math based implementation or ternary based implementation is faster. In the case of roughly the same performance, I would opt for readability.

#### [stoke motor](https://stackoverflow.com/users/7904413/stoke-motor)

 (2017-04-26 18:15:19)

In the spirit of arrow sexiness, you could create a micro
clamp/constrain/gate/&c. function using rest parameters

```
var clamp = (...v) => v.sort((a,b) => a-b)[1];

```

Then just pass in three values

```
clamp(100,-3,someVar);

```

That is, again, if by sexy, you mean 'short'

#### [user947856](https://stackoverflow.com/users/947856/user947856)

 (2016-03-04 19:15:06)

My favorite:

```
[min,x,max].sort()[1]

```
