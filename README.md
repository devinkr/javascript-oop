# Object-Oriented Programming in JavaScript

## Learning Objectives

-   Use a constructor function to produce objects of a particular type.
-   Attach attributes to a new object using the constructor.
-   Understand the cost of defining methods inside a constructor function.
-   Define methods on custom objects by attaching them to the prototype.
-   Refactor prototypes using ES6 class syntax
-   Use the `new` keyword to create instances of a class or prototype
-   Define and put inheritance into practice

## Objects in Review

In programming, _objects_ give us the ability to encapsulate logic and data about things in the real world and represent them in our programs. 


In JavaScript, **objects are collections of properties(key-value pairs)**. We can add, remove, or change these properties as we please. The simplest way to create an object is by using **object literal notation**.

```js
const car = {
  make: 'Honda',
  model: 'Civic',
  year: 1997, // Generally, there's no comma after the last pair!
};
```

In the above example, the variable `car` points to an object literal. This particular object has 3 properties: `make`, `model` and `year`.

What advantages to using an object to store this information versus an array?

### Create

We already saved a sample object to a `car` variable. We did this using **object literal notation**.

```js
const car = {
  make: 'Honda',
  model: 'Civic',
  year: 1997,
  'tire-type': 'Goodyear',
  // NOTE: Keys with a "-" or space in their name must be surrounded by quotation marks.
  // NOTE: You should use camelCase in JS, but sometimes you need to work with hyphens in JSON objects, if they were generated from a non-JavaScript API.
};
```

### Read

To access object properties, we use either dot `.property` or bracket `['property']` notation.

```js
console.log( car.make )
console.log( car['make'] )

// What happens when we try to access a property yet to be defined?
console.log( car.owner )

// NOTE: When accessing properties whose keys have a "-" or space in them, you must use bracket notation.
console.log( car['tire-type'] )

// NOTE: When accessing properties with a variable, you must also use bracket notation
function(carProp) {
  car[carProp]
}
```

Dot notation is much more common than bracket notation.

### Update

Call on the object property just like we did when reading it, and use the assignment operator `=` followed by its new value.

```js
car.year = 2003;
// or
car['year'] = 2003;
```

We can also create brand new properties for an object after it's initialized using this method.

```js
// Now our car has a smell property equal to "Leathery Boot". We did not initially declare this property.
car.smell = 'Leathery Boot';

console.log(car);
```

### Delete

If you want to delete an object property entirely, use the `delete` keyword.

- This deletes the whole key-value pair, not just the value.
- You won't do this often.

```js
delete car.smell;
```

### Methods

Methods are functions that are attached to some object.

```js
// Our car now has a drive method...
const car = {
  make: 'Honda',
  model: 'Civic',
  color: 'red',
  drive: function () {
    console.log('vroom vroom');
  },
  gears: ['Reverse', 'Neutral', '1', '2', '3', '4'],
  engine: {
    horsepower: '6 horses',
    pistons: 12,
    fast: true,
    furious: false,
  },
  // Methods can take arguments
  gps: function (location) {
    console.log(`Beep boop, driving to ${location}`);
  },
};

// We can run the car's two methods like so...
car.drive();
car.gps('neverland');
```

Checkout our awesome souped-up car! With methods as part of our Javascript toolbox, we now have an interface with which we can interact with our objects.

> We've only scratched the surface for objects. We'll dive a bit deeper into them later.  If you're looking for more on the power of objects and functions, we recommend reading [The Secret Life of JS Objects](http://eloquentjavascript.net/06_object.html) chapter in Eloquent JS

## Context

In Javascript, context tells us where functions are invoked.  In short, the **context is the object that a function is attached to**. 

Every time a Javascript function is called, a context is determined / set. That context is always an object, and can be referenced in the function definition using a special keyword in JS, `this`.

We use `this` similar to the way we use pronouns in natural languages like English and French. Say we write:

```
John bites an apple. The apple tastes good
```

We can also say it another way:

```
John bites an apple. This tastes good
```

What does `this` refer to?` The apple.

In a similar manner, we use the `this` keyword as a replacement for the subject in question.

### `this` in an Object

Here's an example of the most common way context is determined for a function. When a method is called on an object, that object becomes the context...

```js
const user = {
  fullName: 'James Reichard',
  sayName: function () {
    console.log(`My name is ${this.fullName}.`);
  },
};
user.sayName();
```

<details>
  <summary><strong>What does <code>this</code> represent here?</strong></summary>

> Here the object that the method is being called on is `user`

</details>

### Default Context

When a function is called, but it's not a method on an object, and no context is otherwise assigned, then the context is set to the default context. In a browser, the default context is the `window` object.

```js
function revealThis() {
  console.log(this);
}

revealThis();
```

### A Rule of Thumb

In general, `this` is probably the **parent** or enclosing item (item being function or object)

Some exceptions are:
- In an event listener function, `this` is the thing where the event originated (such as the button that was clicked).
- In another callback function, in which case `this` is probably the `window`.
- When `.bind()` is used to change the context manually.

When in doubt, log it out...

```js
console.log(this);
```
Now that we've had a refresher on Objects and `this` in JavaScript, we've got a foundation to talk about Object Oriented Programming!

## Object Oriented Programming

Object oriented programming (OOP) isn't a language or a tool. OOP is a style of programming — what we call a **programming paradigm**.  The four pillars of object oriented programming are:

<details>
  <summary><strong>1. Encapsulation</strong></summary>
  <p>Encapsulation is one method that we use to try to make complex systems easier to use.  Encapsulation is defined as <i>the action of enclosing something in, or as if in, a capsule</i>.  In programming, the <i>capsule</i> is an object.  This makes our code clearer and cleaner because all of the related parts are grouped together!</p>
  <p>We also use encapsulation to hide all of the really complex parts of our code, while providing simple ways to access the essential parts from the outside only when necessary.  This means we can isolate the impact of changes in the internal, hidden parts has on the overall system.</p>
</details>
<details>
  <summary><strong>2. Abstraction</strong></summary>
  <p>Abstraction is a concept that is closely related to encapsulation.  It's also a way to remove complexity.  Think of your phone.  It's got a pretty simple user <em><b>interface</b></em>: maybe it has a screen and one button (maybe not even a button!), but the internal logic board of the phone is super complicated.  As a user, we don't need to know anything about how the phone's logic board works in order to use it. This is an example of abstraction in the real world.</p>
  <p>Both abstraction and encapsulation aim to reduce complexity in our code.  Encapsulation refers to the things we do to reduce the complexity in our implementation or how our code is actually written.   Abstraction refers to how we design or architect our code.  Thus abstraction happens when we plan and encapsulation happens when we execute the plan.</p>
</details>
<details>
  <summary><strong>3. Inheritance</strong></summary>
  <p>One of the chief problems with encapsulating all of our code into self contained objects is that there's a strong possibility that we'll have lots of duplicated code among objects of a similar type.  Inheritance helps us solve that problem.</p>
  <p>Let's put this in terms of a real life example too.  Imagine you've got a program with different types of users.  Some users are administrators who can do lots more in our app than customers can. Even though they are different they share a lot in common.  They both have emails, usernames, passwords, profile pictures and much more.</p>
  <p>Using inheritance we can put all of things that users have in common inside of one object called <b>User</b> and then create separate objects for an Admin and a Customer.  Both of the Admin and the Customer will <em><b>inherit</b></em> the properties and behaviors that they share in common from the User.  This helps make our code DRYer.</p>
</details>
<details>
  <summary><strong>4. Polymorphism</strong></summary>
  <p>Poly means <i>many</i> and morph means <i>form</i>, so polymorphism is many forms.  Lets imagine that you have a program with animals (it  could totally happen :smile:). All of the animals have the same method called <b>move</b>.  This method causes the animals to walk to a specific location on the screen.  It works great for some of our animals, but not for the fish or birds in our program.  They need a different type of implementation for moving, they need to swim or fly, not walk.  So the method move can take <em><b>many forms</b></em>, depending on the animal type that uses it!</p>
  <p>Polymorphism makes our code easier to understand and work with because it's way less complicated to remember that every animal has a move method, than to remember that the method for a dog is called walk and the one for the catfish is swim, or the one for the pigeon is fly.  It's also clearer to us if each type of animal is responsible for it's own implementation of move than to have a single method called move that uses a gigantic conditional statement to determine how that one method should be applied to different types of animals.</p>
</details>

### You Do: 

In breakout rooms, come up with real-world examples of these four pillars of OOP! 

## JavaScript and OOP

OOP is not a language, it's just a collection of principles that guide how we write and organize our code.  Some languages are considered to be Object Oriented Programming languages though.  These languages treat everything like an object and use classes as a way to define those objects (think of classes like a blueprint for the object).  Languages like Ruby and Python fall into this category.  Some languages, like C-base languages (e.g., C#, C++, Java), are also considered to be object-oriented programming languages even though they don't strictly treat everything like an object.

Then there's JavaScript... this one is a little controversal.  JavaScript doesn't treat everything as an object nor is based on classes.  JavaScript uses prototypes instead of classes.  So is it an object oriented programming language?  Well, that's a question for others to fight over.  What we can say about JavaScript is that we can absolutely write our code in an object-oriented style following the principles of the OOP paradigm.  In fact, this is one of the most popular ways that you'll see JavaScript used in the wild.

## Prototypes and Prototypal Inheritance

Now that we're using objects to solve problems, it might make sense to have a
way to make multiple objects with the same kind of format - an 'object factory',
designed to construct objects of a particular type.

Suppose we had the following object describing a favorite comic book hero:

```js
const batman = {
  name: 'Bruce Wayne',
  alias: 'The Bat-man',

  usePower: function () {
    return 'Spend money and hit people'
  }
}
```

And now we want another object describing a different hero:

```js
const wonderWoman = {
  name: 'Diana Prince',
  alias: 'Wonder Woman',

  usePower: function () {
    return 'Deflect bullets with bracelets'
  }
}
```

Why is this not a good answer?

One reason is that *code duplication can be a major source of errors in software
development*, and it makes it harder to track down and fix those errors when
they occur.  Another important reason is that it makes our code less efficient and
therefore less performant.  Because of this, writing
[DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)
code is considered a best practice.

### I Do: Constructors

Using **constructor functions** helps us to write code that adheres to the principles of DRY.

Constructor functions:

- Always start with a capital letter (convention).
- Are always used with the `new` keyword (self-enforced).

Bad things happen when you break these rules.

Let's make a Hero constructor function. We'll make use of the function to
reduce duplication in our objects, while allowing the difference to vary by
only defining the differences when we *construct* the new object.

```js
const usePower = function () {
  return this._power
}

const Hero = function (name, alias, power) {
  this.name = name
  this.alias = alias
  this._power = power
  this.usePower = usePower
}
```

It is conventional to use a leading underscore (`_`) on a property name to
indicate to future developers that the property is **not** intended for direct
access or assignment. Nothing in JavaScript enforces this convention, but
developers should consider any property with a leading underscore *private* to
the object (not accessible from the outside).

`const` is just like `let`, except `const` will not let you re-assign a value
to the same name.

```js
const foo = 'bar'
foo = 'baz' // explode!
```

What does my choice of `const` tell you about my expectations for constructor
functions?

We defined a method inside the `Hero` constructor, but doing that is a bad
idea. JavaScript allows it, but **don't do it**. We'll see the right way to
achieve a near identical and preferred result shortly.

Now, let's create `wonderWoman` using the constructor function instead of an
object literal:

```js
const wonderWoman = new Hero('Diana Prince', 'Wonder Woman', 'Deflect bullets with bracelets')
//=> undefined

wonderWoman
/* => { 
  name: 'Diana Prince',
  alias: 'Wonder Woman',
  power: 'Deflects bullets with bracelets',
  usePower: [Function] 
}
*/
```

### Your turn: Create two more instances of the `Hero` class! Log them out to make sure they work.

The `new` keyword in JavaScript does the following, in order:

1. Creates an empty object (`{}`).
1. Attaches the constructor function to the object as a property.
1. Invokes the constructor function as a method on the object.
1. Returns the object.

If we forget to use the `new` keyword what happens is that the context of `this` is not set to the new object!  That means that we'll wind up creating all of our `members` (the properties and methods) on the global object.  In the browser, that means we'll be adding them to the `window` object.

A new object created this way is sometimes called an 'instance' of type `Hero`.

### We Do: Refactor Object Literals Using Constructors

You're working for a car rental company called CarMeisters.  Your code contains a whole bunch of objects like this:

```js

const fusion = {
  body: 'sedan',
  make: 'Ford',
  model: 'Fusion',
  available: true,
  mileage: 0,
  bookReservation: function() {
    this.available = false
  },
  returnVehicle: function() {
    this.available = true
  },
  drive: function(milesDriven) {
    this.mileage += milesDriven
  }
}

const jetta = {
  body: 'sedan',
  make: 'Volkswagen',
  model: 'Jetta',
  available: true,
  mileage: 0,
  bookReservation: function() {
    this.available = false
  },
  returnVehicle: function() {
    this.available = true
  },
  drive: function(milesDriven) {
    this.mileage += milesDriven
  }
}
```

Use what you've learned to create a constructor to allow you to produce many different car objects.


## Prototypes

In the previous section, we saw how to use constructors to deduplicate effort
in creating new objects that share attributes. We learned that we shouldn't
define a method inside a constructor function because we will end up with a
copy of that function inside every instance. So how should we get behavior in
our custom objects?

### We Do: Add Methods to the Prototype

1. Create `usePower` and attach it to the constructor function's prototype.

```js
const usePower = function () {
  return this._power;
};

// Constructor function
const Hero = function (name, alias, power) {
  this.name = name;
  this.alias = alias;
  this._power = power;
};

Hero.prototype.usePower = usePower;

const wonderWoman = new Hero(
  'Diana Prince',
  'Wonder Woman',
  'Deflects bullets with bracelets'
);

const batMan = new Hero('Bruce Wayne', 'The Bat Man', 'Has cool toys');

console.log(wonderWoman, batMan);
// Notice that the Hero instances' `usePower` method now lives in the Prototype, instead of a separate copy of the function being attached to each Hero instance! 😎
```

### You Try: 

1. Create a method to say the hero's name and alias for the code snippet above. Attach it to the prototype.
1. Create new instances of Heroes. Call the method just attached on each Hero.
1. Observe that this method isn't part of objects created using the constructor
   function! 


## Classes in JavaScript

_you_: Wait, you said that there weren't classes in JavaScript...

_me_: There aren't **actual** classes in JavaScript.  Classes in JavaScript are just _**syntactic sugar**_.  Syntactic sugar is syntax within a programming language that is designed to make things easier to read or to express. It makes the language "sweeter" for human use.

## Classes in ES6

The syntax to define a class in JavaScript looks like this:

![Class Syntax](assets/js-class-syntax.png)

> The above figure shows how to define a simple class using JavaScript. The
> Class is defined using the `class` keyword and given a name (in this case
> `Car`). The `constructor` function accepts three parameters (`make`, `model`,
> and `color`) and sets these as attributes. The class also contains a `drive`
> method.

Notice the use of `this` and the fact that we're not returning from the class?

When we want to generate instances of this class, we'll use the `new` keyword:

```js
const corolla = new Car('Toyota', 'Corolla', 'Silver')
const outback = new Car('Subaru', 'Outback', 'Forest Green')
```

The `new` keyword will automatically:

1. Creates an empty object (`{}`).
1. Attaches the constructor function to the object as a property.
1. Invokes the constructor function as a method on the object.
1. Returns the object.

How is this different from the way that we saw it used with constructors before?

> One nice thing about the class syntax is that you cannot forget to use the `new` keyword because you'll get an error immediately in the console if you do.

### You Do: Define an Animal Class

Define a class for your favorite animal (like a dog, cat, or snake). Give your class 3 attributes and an eat method. After you've defined your class, create 3 instances.

## Inheritance

One of the core concepts of OOP we need to implement is inheritance.

### Inheritance in JavaScript

In JavaScript, we can inherit from a class by *extending* it with the `extend`
keyword. This will let us create a subclass:

```js
class Car {
  constructor(make, color) {
    this.make = make
    this.color = color
  }
}

class Toyota extends Car {
  drive() {
    console.log('vroom vroom')
  }
}
```

The above `Toyota` class will include all of the properties defined in the `Car`
class, in addition to the `drive` method.

If we have properties that we want to add to our subclass, we still need to take
in the properties of our parent class, and pass them up to our parent class with
`super`:

```js
class Car {
  constructor(model, color) {
    this.model = model
    this.color = color
  }
}

class Toyota extends Car {
  constructor(model, color) {
    super(model, color)

    this.make = 'Toyota'
  }
  drive() {
    console.log('vroom vroom')
  }
}
```

The `super` method invokes the `constructor` method of the parent (or extended)
class. So in our `Toyota` class, the `super` method will call the `constructor`
method of our `Car` class.

## You Do: Extend an Animal Class

Define an Animal class with the following properties and methods:

-  group (Invertebrates, Fish, Amphibians, Reptiles, Birds, and Mammals)
-  eat: log "yum yum" to the console
-  sleep: log "zzzzz" to the console

Modify your animal from the previous prompt so that it extends your new Animal class.

Create an instance of your animal class (the one that extends the Animal class).

## You Do: Game of Cards

Define a Card class with the following properties:

- suit (hearts, spades, clubs, diamonds)
- rank (Ace, 2, 3, 4, .. Jack, Queen, King)
- score (1, 2, 3, 4, ... 11, 12, 13)

Define a Deck class with the following properties and methods:

-  length (the number of cards - should start at 52)
-  cards (an array of cards in the deck)
-  draw: return a random card from the cards array

When you create an instance of your Deck class (i.e. in your constructor),
fill in the cards array with 52 instances of your Card class. You can do
this with a nested for loop - first loop through an array of all possible
suits, then loop through an array of all possible ranks. Inside your inner
loop, create an instance of your Card class and push it into the Deck's cards
array.

Instantiate an instance of your Deck and start drawing random cards!
