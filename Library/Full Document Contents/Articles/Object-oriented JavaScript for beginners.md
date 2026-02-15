# Object-oriented JavaScript for beginners

![rw-book-cover](https://developer.mozilla.org/mdn-social-share.cd6c4a5a.png)

## Metadata
- Author: [[mozilla.org]]
- Full Title: Object-oriented JavaScript for beginners
- Category: #articles
- URL: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object-oriented_JS
- Summary: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object-oriented_JS

# Notes

[[mozilla.org - Object-oriented JavaScript for beginners]]

***

In [the last article](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object-oriented_programming), we introduced some basic concepts of object-oriented programming (OOP), and discussed an example where we used OOP principles to model professors and students in a school.

We also talked about how it's possible to use [prototypes](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes) and [constructors](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Basics#introducing_constructors) to implement a model like this, and that JavaScript also provides features that map more closely to classical OOP concepts.

In this article, we'll go through these features. It's worth keeping in mind that the features described here are not a new way of combining objects: under the hood, they still use prototypes. They're just a way to make it easier to set up a prototype chain.

|  |  |
| --- | --- |
| Prerequisites: |  Basic computer literacy, a basic understanding of HTML and CSS, familiarity with JavaScript basics (see [First steps](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps) and [Building blocks](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks)) and OOJS basics (see [Introduction to objects](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Basics), [Object prototypes](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes), and [Object-oriented programming](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object-oriented_programming)).  |
| Objective: | To understand how to use the features JavaScript provides to implement "classical" object-oriented programs. |

You can declare a class using the [`class`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/class) keyword. Here's a class declaration for our `Person` from the previous article:

```
class Person {

  name;

  constructor(name) {
    this.name = name;
  }

  introduceSelf() {
    console.log(`Hi! I'm ${this.name}`);
  }

}

```

This declares a class called `Person`, with:

* a `name` property.
* a constructor that takes a `name` parameter that is used to initialize the new object's `name` property
* an `introduceSelf()` method that can refer to the object's properties using `this`.

The `name;` declaration is optional: you could omit it, and the line `this.name = name;` in the constructor will create the `name` property before initializing it. However, listing properties explicitly in the class declaration might make it easier for people reading your code to see which properties are part of this class.

You could also initialize the property to a default value when you declare it, with a line like `name = '';`.

The constructor is defined using the [`constructor`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/constructor) keyword. Just like a [constructor outside a class definition](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Basics#introducing_constructors), it will:

* create a new object
* bind `this` to the new object, so you can refer to `this` in your constructor code
* run the code in the constructor
* return the new object.

Given the class declaration code above, you can create and use a new `Person` instance like this:

Note that we call the constructor using the name of the class, `Person` in this example.

If you don't need to do any special initialization, you can omit the constructor, and a default constructor will be generated for you:

```
class Animal {

  sleep() {
    console.log('zzzzzzz');
  }

}

const spot = new Animal();

spot.sleep(); // 'zzzzzzz'

```

Given our `Person` class above, let's define the `Professor` subclass.

```
class Professor extends Person {

  teaches;

  constructor(name, teaches) {
    super(name);
    this.teaches = teaches;
  }

  introduceSelf() {
    console.log(`My name is ${this.name}, and I will be your ${this.teaches} professor.`);
  }

  grade(paper) {
    const grade = Math.floor(Math.random() * (5 - 1) + 1);
    console.log(grade);
  }

}

```

We use the [`extends`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/extends) keyword to say that this class inherits from another class.

The `Professor` class adds a new property `teaches`, so we declare that.

Since we want to set `teaches` when a new `Professor` is created, we define a constructor, which takes the `name` and `teaches` as arguments. The first thing this constructor does is call the superclass constructor using [`super()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/super), passing up the `name` parameter. The superclass constructor takes care of setting `name`. After that, the `Professor` constructor sets the `teaches` property.

**Note:** If a subclass has any of its own initialization to do, it **must** first call the superclass constructor using `super()`, passing up any parameters that the superclass constructor is expecting.

We've also overridden the `introduceSelf()` method from the superclass, and added a new method `grade()`, to grade a paper (our professor isn't very good, and just assigns random grades to papers).

With this declaration we can now create and use professors:

```
const walsh = new Professor('Walsh', 'Psychology');
walsh.introduceSelf();  // 'My name is Walsh, and I will be your Psychology professor'

walsh.grade('my paper'); // some random grade

```

Finally, let's see how to implement encapsulation in JavaScript. In the last article we discussed how we would like to make the `year` property of `Student` private, so we could change the rules about archery classes without breaking any code that uses the `Student` class.

Here's a declaration of the `Student` class that does just that:

```
class Student extends Person {

  #year;

  constructor(name, year) {
    super(name);
    this.#year = year;
  }

  introduceSelf() {
    console.log(`Hi! I'm ${this.name}, and I'm in year ${this.#year}.`);
  }

  canStudyArchery() {
    return this.#year > 1;
  }

}

```

In this class declaration, `#year` is a [private data property](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_class_fields). We can construct a `Student` object, and it can use `#year` internally, but if code outside the object tries to access `#year` the browser throws an error:

```
const summers = new Student('Summers', 2);

summers.introduceSelf(); // Hi! I'm Summers, and I'm in year 2.
summers.canStudyArchery(); // true

summers.#year; // SyntaxError

```

Private data properties must be declared in the class declaration, and their names start with `#`.

You can have private methods as well as private data properties. Just like private data properties, their names start with `#`, and they can only be called by the object's own methods:

```
class Example {
  somePublicMethod() {
    this.#somePrivateMethod();
  }

  #somePrivateMethod() {
    console.log('You called me?');
  }
}

const myExample = new Example();

myExample.somePublicMethod(); // 'You called me?'

myExample.#somePrivateMethod(); // SyntaxError

```

You've reached the end of this article, but can you remember the most important information? You can find some further tests to verify that you've retained this information before you move on — see [Test your skills: Object-oriented JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Test_your_skills:_Object-oriented_JavaScript).

#### [Summary](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Classes_in_JavaScript#summary)

* **Classes in JavaScript**
