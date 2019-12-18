# Object-Oriented Programming in JavaScript

## Learning Objectives

-   Use a constructor function to produce objects of a particular type.
-   Attach attributes to a new object using the constructor.
-   Recall the cost of defining methods inside a constructor function.
-   Define methods on custom objects by attaching them to the prototype.
-   Refactor prototypes using ES6 class syntax
-   Use the `new` keyword to create instances of a class or prototype
-   Define and put inheritance into practice

## Object Oriented Programming

Object oriented programming (OOP) isn't a language or a tool. OOP is a style of programming — what we call a **programming paradigm**.  The four pillars of object oriented programming are:

1.   **Encapsulation**
2.   **Abstraction**
3.   **Inheritance**
4.   **Polymorphism**

### Encapsulation

Encapsulation is one method that we use to try to make complex systems easier to use.  Encapsulation is defined as _the action of enclosing something in, or as if in, a capsule_.  In programming, the _capsule_ is an object.  This makes our code clearer and cleaner because all of the related parts are grouped together!

We also use encapsulation to hide all of the really complex parts of our code, while providing simple ways to access the essential parts from the outside only when necessary.  This means we can isolate the impact of changes in the internal, hidden parts has on the overall system.

### Abstraction

Abstraction is a concept that is closely related to encapsulation.  It's also a way to remove complexity.  Think of your phone.  It's got a pretty simple user _**interface**_: maybe it has a screen and one button (maybe not even a button!), but the internal logic board of the phone is super complicated.  As a user, we don't need to know anything about how the phone's logic board works in order to use it. This is an example of abstraction in the real world.

Both abstraction and encapsulation aim to reduce complexity in our code.  Encapsulation refers to the things we do to reduce the complexity in our implementation or how our code is actually written.   Abstraction refers to how we design or architect our code.  Thus abstraction happens when we plan and encapsulation happens when we execute the plan.

### Inheritance

One of the chief problems with encapsulating all of our code into self contained objects is that there's a strong possibility that we'll have lots of duplicated code among objects of a similar type.  Inheritance helps us solve that problem.

Let's put this in terms of a real life example too.  Imagine you've got a program with different types of users.  Some users are administrators who can do lots more in our app than customers can. Even though they are different they share a lot in common.  They both have emails, usernames, passwords, profile pictures and much more.

Using inheritance we can put all of things that users have in common inside of one object called **User** and then create separate objects for an Admin and a Customer.  Both of the Admin and the Customer will _**inherit**_ the properties and behaviors that they share in common from the User.  This helps make our code DRYer.

### Polymorphism

Poly means _many_ and morph means _form_, so polymorphism is many forms.  Lets imagine that you have a program with animals (it  could totally happen :smile:). All of the animals have the same method called **move**.  This method causes the animals to walk to a specific location on the screen.  It works great for some of our animals, but not for the fish or birds in our program.  They need a different type of implementation for moving, they need to swim or fly, not walk.  So the method move can take _**many forms**_, depending on the animal type that uses it!

Polymorphism makes our code easier to understand and work with because it's way less complicated to remember that every animal has a move method, than to remember that the method for a dog is called walk and the one for the catfish is swim, or the one for the pigeon is fly.  It's also clearer to us if each type of animal is responsible for it's own implementation of move than to have a single method called move that uses a gigantic conditional statement to determine how that one method should be applied to different types of animals.

### OOP Lab

Break up into groups and come up with a way to describe each of these concepts to a 10 year old child.  Put each in terms of a real life example or object like a washing machine or car.

## JavaScript and OOP

As we've seen, OOP is not a language, it's just a collection of principles that guide how we write and organize our code.  Some languages are considered to be Object Oriented Programming languages though.  These languages treat everything like an object and use classes as a way to define those objects (think of classes like a blueprint for the object).  Languages like Ruby and Python fall into this category.  Some languages, like C-base languages (e.g., C#, C++, Java), are also considered to be object-oriented programming languages even though they don't strictly treat everything like an object.

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
const wonderWoman = new Hero('Diana Prince',
                           'Wonder Woman',
                           'Deflect bullets with bracelets')
//=> undefined

wonderWoman
/* => { name: 'Diana Prince',
  alias: 'Wonder Woman',
  power: 'Deflects bullets with bracelets',
  usePower: [Function] }
  */
```

The `new` keyword in JavaScript does the following, in order:

1. Creates an empty object (`{}`).
1. Attaches the constructor function to the object as a property.
1. Invokes the constructor function as a method on the object.
1. Returns the object.

If we forget to use the `new` keyword what happens is that the context of `this` is not set to the new object!  That means that we'll wind up creating all of our `members` (the properties and methods) on the global object.  In the browser, that means we'll be adding them to the `window` object.

A new object created this way is sometimes called an 'instance' of type `Hero`.

### You Do: Refactor Object Literals Using Constructors

You're working for a company called CarMeisters.  Your code contains a whole bunch of objects like this:

```js

const fusion = {
  type: 'full-size',
  body: 'sedan',
  make: 'Ford',
  model: 'Fusion',
  doors: '4-door',
  available: true,
  bookReservation: function() {
    this.available = false
  }
}

const jetta = {
  type: 'standard',
  body: 'sedan',
  make: 'Volkswagen',
  model: 'Jetta',
  doors: '4-door',
  available: true,
  bookReservation: function() {
    this.available = false
  }
}
```

Use what you've learned to create a constructor to allow you to produce many different car objects.


## Prototypes

In the previous section, we saw how to use constructors to deduplicate effort
in creating new objects that share attributes. We learned that we should never
define a method inside a constructor function, because we will end up with a
copy of that function inside every instance. So how should we get behavior in
our custom objects?

### We Do: Add Methods to the Prototype

1. Create `usePower` and attach it to the constructor function's prototype.
1. Create a method to say the hero's name and alias. Attach it to the
   prototype.
1. Create `batman` and `wonderWoman`. Call the method just attached.
1. Observe that this method isn't part of objects created using the constructor
   function.

### You Do: Add Methods to the Prototype

How can you refactor your `Car` using a protoype so that we have a more efficient system?  Add a protoype and move the methods there that all of the cars share!

## Classes in JavaScript

_you_: Wait, you said that there weren't classes in JavaScript?

_me_: There aren't **real** classes in JavaScript.  Classes in JavaScript are just _**syntactic sugar**_.  Syntactic sugar is syntax within a programming language that is designed to make things easier to read or to express. It makes the language "sweeter" for human use.

## Classes in ES6

> Follow along with Prompt #1 in [JS OOP
> Practice](https://git.generalassemb.ly/dc-wdi-fundamentals/js-oop-practice)

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
const carolla = new Car('Toyota', 'Carolla', 'Grey')
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

> 10 min to work, 5 min to share with the class

Work through Prompt #2 in [JS OOP
Practice](https://git.generalassemb.ly/dc-wdi-fundamentals/js-oop-practice)

## Inheritance

One of the core concepts of OOP we need to implement is inheritance.

> Follow along with Prompt #3 in [JS OOP
> Practice](https://git.generalassemb.ly/dc-wdi-fundamentals/js-oop-practice)

### Turn & Talk

Turn to your neighbor or the people in your row and discuss the following
questions:

* What is inheritance in this case?
* What problem does inheritance solve?

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

Work through Prompt #4 in [JS OOP
Practice](https://git.generalassemb.ly/dc-wdi-fundamentals/js-oop-practice)

## You Do: Game of Cards

Work through Prompt #5 in [JS OOP
Practice](https://git.generalassemb.ly/dc-wdi-fundamentals/js-oop-practice)
