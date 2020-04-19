## Chapter 02: First Class Functions

### A Quick Review

When we say functions are "first class", we mean we can treat functions like any other data type and there is nothing particularly special about them - they may be stored in arrays, passed around as function parameters, assigned to variables etc.

- Example:

```js
const hi = name => `Hi ${name}`;
const greeting = name => hi(name);
```

The function wrapper around `hi` in `greeting` is completely redundant.
Because functions are callable in JavaScript.
When `hi` has the `()` at the end it will run and return a value.
When it does not, it simply returns the function stored in the variable.

```js
hi; // name => `Hi ${name}`
hi("jonas"); // "Hi jonas"
```

Since greeting is merely in turn calling hi with the very same argument, we could simply write:

```js
const greeting = hi;
greeting("times"); // "Hi times"
```

In other words, `hi` is already a function that expects one argument, why place another function around it that simply calls hi with the same bloody argument?

The world is littered with ajax code exactly like this. Here is the reason both are equivalent:

```js
// this line
ajaxCall(json => callback(json));

// is the same as this line
ajaxCall(callback);

// so refactor getServerStuff
const getServerStuff = callback => ajaxCall(callback);

// ...which is equivalent to this
const getServerStuff = ajaxCall; // <-- look mum, no ()'s
```

### Why Favor First Class?

Okay, let's get down to the reasons to favor first class functions.
It's easy to add layers of indirection that provide no added value and only increase the amount of redundant code to maintain and search through.
In addition, if such a needlessly wrapped function must be changed, we must also need to change our wrapper function as well.

```js
httpGet("/post/2", json => renderPost(json));
```

If `httpGet` were to change to send a possible `err`, we would need to go back and change the "glue".

```js
// go back to every httpGet call in the application and explicitly pass err along.
httpGet("/post/2", (json, err) => renderPost(json, err));
```

Had we written it as a first class function, much less would need to change:

```js
// renderPost is called from within httpGet with however many arguments it wants
httpGet("/post/2", renderPost);
```

Besides the removal of unnecessary functions, we must name and reference arguments.

Having multiple names for the same concept is a common source of confusion in projects. There is also the issue of generic code.

For instance, these two functions do exactly the same thing, but one feels infinitely more general and reusable:

```js
// specific to our current blog
const validArticles = articles =>
  articles.filter(article => article !== null && article !== undefined),

// vastly more relevant for future projects
const compact = xs => xs.filter(x => x !== null && x !== undefined);
```

By using specific naming, we've seemingly tied ourselves to specific data (in this case articles). This happens quite a bit and is a source of much reinvention.

---

## Chapter 03: Pure Happiness with Pure Functions

### Oh to Be Pure Again

One thing we need to get straight is the idea of a pure function.

> A pure function is a function that, given the same input, will always return the same output and does not have any observable side effect.

Take `slice` and `splice`. They are two functions that do the exact same thing - in a vastly different way, mind you, but the same thing nonetheless.

`slice` is pure because it returns the same output per input every time, guaranteed.
`splice` will chew up its array and spit it back out forever changed which is an observable effect.

```js
const xs = [1, 2, 3, 4, 5];

// pure
xs.slice(0, 3); // [1,2,3]
xs.slice(0, 3); // [1,2,3]
xs.slice(0, 3); // [1,2,3]

// impure
xs.splice(0, 3); // [1,2,3]
xs.splice(0, 3); // [4,5]
xs.splice(0, 3); // []
```

In functional programming, we dislike functions like `splice`, that mutate data.

This will never do as we're striving for reliable functions that return the same result every time, not functions that leave a mess in their wake like splice.

Let's look at another example:

```js
// impure
let minimum = 21;
const checkAge = age => age >= minimum;

// pure
const checkAge = age => {
  const minimum = 21;
  return age >= minimum;
};
```

In the impure portion, `checkAge` depends on the mutable variable minimum to determine the result. In other words, it depends on system state which is disappointing because it increases the cognitive load by introducing an external environment.

It might not seem like a lot in this example, but this reliance upon state is one of the largest contributors to system complexity. This `checkAge` may return different results depending on factors external to input, which not only disqualifies it from being pure, but also puts our minds through the ringer each time we're reasoning about the software.

Its pure form, on the other hand, is completely self sufficient. We can also make minimum immutable, which preserves the purity as the state will never change. To do this, we must create an object to freeze.

`const immutableState = Object.freeze({ minimum: 21 });`

### Side Effects May Include...

Let's look more at these "side effects" to improve our intuition.

We'll be referring to effect as anything that occurs in our computation other than the calculation of a result.

There's nothing intrinsically bad about effects and we'll be using them all over the place in the chapters to come. It's that side part that bears the negative connotation. Water alone is not an inherent larvae incubator, it's the stagnant part that yields the swarms, and I assure you, side effects are a similar breeding ground in your own programs.

> A side effect is a change of system state or observable interaction with the outside world that occurs during the calculation of a result.

Side effects may include, but are not limited to:

- changing the file system
- inserting a record into a database
- making an http call
- mutations
- printing to the screen / logging
- obtaining user input
- querying the DOM
- accessing system state

And the list goes on and on. Any interaction with the world outside of a function is a side effect, which is a fact that may prompt you to suspect the practicality of programming without them. The philosophy of functional programming postulates that side effects are a primary cause of incorrect behavior.

It is not that we're forbidden to use them, rather we want to contain them and run them in a controlled way.

We'll learn how to do this when we get to functors and monads in later chapters, but for now, let's try to keep these insidious functions separate from our pure ones.

Side effects disqualify a function from being pure. And it makes sense: pure functions, by definition, must always return the same output given the same input, which is not possible to guarantee when dealing with matters outside our local function.

Let's take a closer look at why we insist on the same output per input.

### 8th Grade Math

> A function is a special relationship between values: Each of its input values gives back exactly one output value.

In other words, it's just a relation between two values: the input and the output. Though each input has exactly one output, that output doesn't necessarily have to be unique per input.

Functions can be described as a set of pairs with the position (input, output): `[(1,2), (3,6), (5,10)]` (It appears this function doubles its input).

Or perhaps a table. Or even as a graph with x as the input and y as the output.

There's no need for implementation details if the input dictates the output. Since functions are simply mappings of input to output, one could simply jot down object literals and run them with `[]` instead of `()`.

```js
const toLowerCase = {
  A: "a",
  B: "b",
  C: "c",
  D: "d",
  E: "e",
  F: "f"
};
toLowerCase["C"]; // 'c'
```

Of course, you might want to calculate instead of hand writing things out, but this illustrates a different way to think about functions. (You may be thinking "what about functions with multiple arguments?". Indeed, that presents a bit of an inconvenience when thinking in terms of mathematics. For now, we can bundle them up in an array or just think of the arguments object as the input. When we learn about currying, we'll see how we can directly model the mathematical definition of a function.)

Here comes the dramatic reveal: Pure functions are mathematical functions and they're what functional programming is all about. Programming with these little angels can provide huge benefits. Let's look at some reasons why we're willing to go to great lengths to preserve purity.

### The Case for Purity

#### Cacheable

For starters, pure functions can always be cached by input. This is typically done using a technique called memoization:

```js
const squareNumber = memoize(x => x * x);

squareNumber(4); // 16
squareNumber(4); // 16, returns cache for input 4

squareNumber(5); // 25
squareNumber(5); // 25, returns cache for input 5
```

Here is a simplified implementation, though there are plenty of more robust versions available.

```js
const memoize = f => {
  const cache = {};

  return (...args) => {
    const argStr = JSON.stringify(args);
    cache[argStr] = cache[argStr] || f(...args);
    return cache[argStr];
  };
};
```

The takeaway is that we can cache every function no matter how destructive they seem.

#### Portable / Self-documenting

Pure functions are completely self contained. Everything the function needs is handed to it on a silver platter.

For starters, a function's dependencies are explicit and therefore easier to see and understand - no funny business going on under the hood.

```js
// impure
const signUp = attrs => {
  const user = saveUser(attrs);
  welcomeUser(user);
};

// pure
const signUp = (Db, Email, attrs) => () => {
  const user = saveUser(Db, attrs);
  welcomeUser(Email, user);
};
```

The example here demonstrates that the pure function must be honest about its dependencies and, as such, tell us exactly what it's up to. Just from its signature, we know that it will use a Db, Email, and attrs which should be telling to say the least.

We'll learn how to make functions like this pure without merely deferring evaluation, but the point should be clear that the pure form is much more informative than its sneaky impure counterpart which is up to who knows what.

Something else to notice is that we're forced to "inject" dependencies, or pass them in as arguments, which makes our app much more flexible because we've parameterized our database or mail client or what have you. Should we choose to use a different Db we need only to call our function with it. Should we find ourselves writing a new application in which we'd like to reuse this reliable function, we simply give this function whatever Db and Email we have at the time.

In a JavaScript setting, portability could mean serializing and sending functions over a socket. It could mean running all our app code in web workers. Portability is a powerful trait.

Contrary to "typical" methods and procedures in imperative programming rooted deep in their environment via state, dependencies, and available effects, pure functions can be run anywhere our hearts desire.

When was the last time you copied a method into a new app? One of my favorite quotes comes from Erlang creator, Joe Armstrong: "The problem with object-oriented languages is they’ve got all this implicit environment that they carry around with them. You wanted a banana but what you got was a gorilla holding the banana... and the entire jungle".

#### Testable

Next, we come to realize pure functions make testing much easier.

We don't have to mock a "real" payment gateway or setup and assert the state of the world after each test. We simply give the function input and assert output.

In fact, we find the functional community pioneering new test tools that can blast our functions with generated input and assert that properties hold on the output. It's beyond the scope of this book, but I strongly encourage you to search for and try Quickcheck - a testing tool that is tailored for a purely functional environment.

#### Reasonable

Many believe the biggest win when working with pure functions is referential transparency.

A spot of code is referentially transparent when it can be substituted for its evaluated value without changing the behavior of the program.

Since pure functions don't have side effects, they can only influence the behavior of a program through their output values. Furthermore, since their output values can reliably be calculated using only their input values, pure functions will always preserve referential transparency. Let's see an example.

```js
const { Map } = require("immutable");

// Aliases: p = player, a = attacker, t = target
const jobe = Map({ name: "Jobe", hp: 20, team: "red" });
const michael = Map({ name: "Michael", hp: 20, team: "green" });
const decrementHP = p => p.set("hp", p.get("hp") - 1);
const isSameTeam = (p1, p2) => p1.get("team") === p2.get("team");
const punch = (a, t) => (isSameTeam(a, t) ? t : decrementHP(t));

punch(jobe, michael); // Map({name:'Michael', hp:19, team: 'green'})
```

`decrementHP`, `isSameTeam` and `punch` are all pure and therefore referentially transparent.

We can use a technique called equational reasoning wherein one substitutes "equals for equals" to reason about code.

It's a bit like manually evaluating the code without taking into account the quirks of programmatic evaluation. Using referential transparency, let's play with this code a bit.

First we'll inline the function `isSameTeam`.

`const punch = (a, t) => (a.get('team') === t.get('team') ? t : decrementHP(t));`

Since our data is immutable, we can simply replace the teams with their actual value

`const punch = (a, t) => ('red' === 'green' ? t : decrementHP(t));`

We see that it is false in this case so we can remove the entire if branch

`const punch = (a, t) => decrementHP(t);`

And if we inline decrementHP, we see that, in this case, punch becomes a call to decrement the hp by 1.

`const punch = (a, t) => t.set('hp', t.get('hp') - 1);`

This ability to reason about code is terrific for refactoring and understanding code in general.

Indeed, we'll be using these techniques throughout the book.

#### Parallel Code

Finally, we can run any pure function in parallel since it does not need access to shared memory and it cannot, by definition, have a race condition due to some side effect.

This is very much possible in a server side js environment with threads as well as in the browser with web workers though current culture seems to avoid it due to complexity when dealing with impure functions.

### In Summary

We've seen what pure functions are and why we, as functional programmers, believe they are the cat's evening wear.

From this point on, we'll strive to write all our functions in a pure way.

We'll require some extra tools to help us do so, but in the meantime, we'll try to separate the impure functions from the rest of the pure code.

Writing programs with pure functions is a tad laborious without some extra tools in our belt. We have to juggle data by passing arguments all over the place, we're forbidden to use state, not to mention effects.

How does one go about writing these masochistic programs?

Let's acquire a new tool called curry.

---

## Chapter 04: Currying

### Can't Live If Livin' Is without You

My Dad once explained how there are certain things one can live without until one acquires them. A microwave is one such thing. Smart phones, another. The older folks among us will remember a fulfilling life sans internet. For me, currying is on this list.

The concept is simple: You can call a function with fewer arguments than it expects. It returns a function that takes the remaining arguments.

You can choose to call it all at once or simply feed in each argument piecemeal.

```js
const add = x => y => x + y;
const increment = add(1);
const addTen = add(10);

increment(2); // 3
addTen(2); // 12
```

Here we've made a function add that takes one argument and returns a function. By calling it, the returned function remembers the first argument from then on via the closure. Calling it with both arguments all at once is a bit of a pain, however, so we can use a special helper function called curry to make defining and calling functions like this easier.

### More Than a Pun / Special Sauce

Currying is useful for many things. We can make new functions just by giving our base functions some arguments.

We also have the ability to transform any function that works on single elements into a function that works on arrays simply by wrapping it.

Giving a function fewer arguments than it expects is typically called partial application. Partially applying a function can remove a lot of boiler plate code.

We typically don't define functions that work on arrays, because we can just call `map()` inline. Same with sort, filter, and other higher order functions (a higher order function is a function that takes or returns a function).

When we spoke about pure functions, we said they take 1 input to 1 output. Currying does exactly this: each single argument returns a new function expecting the remaining arguments. That is 1 input to 1 output.

No matter if the output is another function - it qualifies as pure. We do allow more than one argument at a time, but this is seen as merely removing the extra `()`'s for convenience.

### In Summary

Currying is handy and I very much enjoy working with curried functions on a daily basis. It is a tool for the belt that makes functional programming less verbose and tedious.

We can make new, useful functions on the fly simply by passing in a few arguments and as a bonus, we've retained the mathematical function definition despite multiple arguments.

Let's acquire another essential tool called compose.

## Chapter 05: Coding by Composing

### Functional Husbandry

Here's compose:

```js
const compose = (...fns) => (...args) =>
  fns.reduceRight((res, fn) => [fn.call(null, ...res)], args)[0];
```

Don't be scared! This is the level-9000-super-Saiyan-form of compose. For the sake of reasoning, let's drop the variadic implementation and consider a simpler form that can compose two functions together. Once you get your head around that, you can push the abstraction further and consider it simply works for any number of functions (we could even prove that)! Here's a more friendly compose for you my dear readers:

```js
const compose2 = (f, g) => x => f(g(x));
```

f and g are functions and x is the value being "piped" through them.

Composition feels like function husbandry. You, breeder of functions, select two with traits you'd like to combine and mash them together to spawn a brand new one. Usage is as follows:

```js
const toUpperCase = x => x.toUpperCase();
const exclaim = x => `${x}!`;
const shout = compose(
  exclaim,
  toUpperCase
);

shout("send in the clowns"); // "SEND IN THE CLOWNS!"
```

The composition of two functions returns a new function. This makes perfect sense: composing two units of some type (in this case function) should yield a new unit of that very type. You don't plug two legos together and get a lincoln log. There is a theory here, some underlying law that we will discover in due time.

In our definition of compose, the g will run before the f, creating a right to left flow of data. This is much more readable than nesting a bunch of function calls. Without compose, the above would read:

```js
const shout = x => exclaim(toUpperCase(x));
```

Composition is straight from the math books. In fact, perhaps it's time to look at a property that holds for any composition.

```js
// associativity
compose(
  f,
  compose(
    g,
    h
  )
) ===
  compose(
    compose(
      f,
      g
    ),
    h
  );
```

```js
const head = x => x[0];
const reverse = reduce((acc, x) => [x].concat(acc), []);
const last = compose(
  head,
  reverse
);

last(["jumpkick", "roundhouse", "uppercut"]); // 'uppercut'
```

Composition is associative, meaning it doesn't matter how you group two of them. So, should we choose to uppercase the string, we can write:

```js
compose(
  toUpperCase,
  compose(
    head,
    reverse
  )
);
// or
compose(
  compose(
    toUpperCase,
    head
  ),
  reverse
);
```

Since it doesn't matter how we group our calls to compose, the result will be the same.

Applying the associative property gives us this flexibility and peace of mind that the result will be equivalent.

There's no right or wrong answers - we're just plugging our legos together in whatever way we please.

### Pointfree

Pointfree style means never having to say your data. It means functions that never mention the data upon which they operate. First class functions, currying, and composition all play well together to create this style.

```js
// not pointfree because we mention the data: word
const snakeCase = word => word.toLowerCase().replace(/\s+/gi, "_");

// pointfree
const snakeCase = compose(
  replace(/\s+/gi, "_"),
  toLowerCase
);
```

See how we partially applied replace? What we're doing is piping our data through each function of 1 argument. Currying allows us to prepare each function to just take its data, operate on it, and pass it along. Something else to notice is how we don't need the data to construct our function in the pointfree version, whereas in the pointful one, we must have our word available before anything else.
