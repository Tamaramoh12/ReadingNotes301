# Concepts of Functional Programming in Javascript

```
Complexity is anything that makes software hard to understand or to modify.
```

Doing some research, I found **functional programming** concepts like **immutability** and **pure function**. Those concepts are big advantages to build side-effect-free functions, so it is easier to maintain systems — with some other benefits.

**Functional programming** is a programming paradigm — a style of building the structure and elements of computer programs — that treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data.

### Pure functions

1- **It returns the same result if given the same arguments (it is also referred as deterministic).**

- Imagine we want to implement a function that calculates the area of a circle. An impure function would receive radius as the parameter, and then calculate radius * radius * PI.

Ex:
```
let PI = 3.14;

const calculateArea = (radius) => radius * radius * PI;

calculateArea(10); // returns 314.0
```
Why is this an impure function? Simply because it uses a global object that was not passed as a parameter to the function.

Now imagine some mathematicians argue that the PI value is actually 42and change the value of the global object.
Our impure function will now result in 10 * 10 * 42 = 4200. For the same parameter (radius = 10), we have a different result. Let's fix it!

```
let PI = 3.14;

const calculateArea = (radius, pi) => radius * radius * pi;

calculateArea(10, PI); // returns 314.0
```

Now we’ll always pass thePI value as a parameter to the function. So now we are just accessing parameters passed to the function. No external object.

For the parameters radius = 10 & PI = 3.14, we will always have the same the result: 314.0

For the parameters radius = 10 & PI = 42, we will always have the same the result: 4200

- Any function that relies on a random number generator cannot be pure.

```
function yearEndEvaluation() {
  if (Math.random() > 0.5) {
    return "You get a raise!";
  } else {
    return "Better luck next year!";
  }
}
```

2- **It does not cause any observable side effects.**

Examples of observable side effects include modifying a global object or a parameter passed by reference.
Now we want to implement a function to receive an integer value and return the value increased by 1.

```
let counter = 1;

function increaseCounter(value) {
  counter = value + 1;
}

increaseCounter(counter);
console.log(counter); // 2
```

We have the counter value. Our impure function receives that value and re-assigns the counter with the value increased by 1.

Observation: mutability is discouraged in functional programming.

We are modifying the global object. But how would we make it pure? Just return the value increased by 1. Simple as that.

```
let counter = 1;

const increaseCounter = (value) => value + 1;

increaseCounter(counter); // 2
console.log(counter); // 1
```

See that our pure function increaseCounter returns 2, but the counter value is still the same. The function returns the incremented value without altering the value of the variable.

If we follow these two simple rules, it gets easier to understand our programs. Now every function is isolated and unable to impact other parts of our system.
Pure functions are stable, consistent, and predictable. 

Given the same parameters, pure functions will always return the same result. We don’t need to think of situations when the same parameter has different results — because it will never happen.

### Immutability

When data is immutable, its state cannot change after it’s created. If you want to change an immutable object, you can’t. Instead, you create a new object with the new value.

In Javascript we commonly use the for loop. This next for statement has some mutable variables.

```
var values = [1, 2, 3, 4, 5];
var sumOfValues = 0;

for (var i = 0; i < values.length; i++) {
  sumOfValues += values[i];
}

sumOfValues // 15
```

For each iteration, we are changing the i and the sumOfValue state. But how do we handle mutability in iteration? Recursion!

```
let list = [1, 2, 3, 4, 5];
let accumulator = 0;

function sum(list, accumulator) {
  if (list.length == 0) {
    return accumulator;
  }

  return sum(list.slice(1), accumulator + list[0]);
}

sum(list, accumulator); // 15
list; // [1, 2, 3, 4, 5]
accumulator; // 0
```

# Refactoring JavaScript for Performance and Readability 

Recently, I wrote an article about how to write very fast JavaScript. Some of the examples took it to the extreme and became very quick at the cost of being totally unmaintainable. There's a middle ground between speed and comprehension and that's where good code lives.

Example:

We're an URL-shortening website, like TinyURL. We accept a long URL and return a short URL that forwards visitors to the long URL. We have two functions.

```
// Unrefactored code

const URLstore = [];

function makeShort(URL) {
  const rndName = Math.random().toString(36).substring(2);
  URLstore.push({[rndName]: URL});
  return rndName;
}

function getLong(shortURL) {
  for (let i = 0; i < URLstore.length; i++) {
    if (URLstore[i].hasOwnProperty(shortURL) !== false) {
      return URLstore[i][shortURL];
    }
  }
}
```

Problem: what happens if getLong is called with a short URL that isn't in the store? Nothing is explicitly returned so undefined will be returned. Since we're not sure how we're going to handle that, let's be explicit and throw an error so that problems can be spotted during development.

Performance-wise, be careful if you're iterating through a flat array very often, especially if it's a core piece of your program. The refactor here is to change the data-structure of URLstore.

Currently, each URL object is stored in an array. We'll visualize this as a row of buckets. When we want to convert short to long, on average we need to check half of those buckets before we find the correct short URL. What if we have thousands of buckets, and we perform this hundreds of times a second?

The answer is to use some form of hash function, which Maps and Sets use under the surface. A hash function is used to map a given key to a location in the hash table. Below, this happens when we place our short URL into the store in makeShort and when we get it back out in getLong. Depending on how you're measuring run time, the result is that on average we only need to check one bucket — no matter how many total buckets there are!

```
// Refactored code

const URLstore = new Map(); // Change this to a Map

function makeShort(URL) {
  const rndName = Math.random().toString(36).substring(2);
  // Place the short URL into the Map as the key with the long URL as the value
  URLstore.set(rndName, URL);
  return rndName;
}

function getLong(shortURL) {
  // Leave the function early to avoid an unnecessary else statement
  if (URLstore.has(shortURL) === false) {
    throw 'Not in URLstore!';
  }
  return URLstore.get(shortURL); // Get the long URL out of the Map
}
```

For those examples, we assumed that the random function wouldn't clash. 'Cloning TinyURL' is a common system design question and a very interesting one at that. What if the random function does clash? It's easy to tack on addendums about scaling and redundancy.











