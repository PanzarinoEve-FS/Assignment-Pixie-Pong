The key learning objectives this week are:
1. Learn to chain asynchronous JavaScript code effectively using Promises.

2. Utilize a JavaScript graphics library (PixiJS) to create and manipulate visual canvas assets on the display.

Debugger
We were provided with debugger.html.zip
A debugger allows you to set breakpoints and inspect values, enables faster debugging with more accuracy and tools to diagnose the issue.

Asynchronous Logic & Concurrency

Synchronous code executes one line at a time
Asynchronous Code uses promises
Asynchronous Code allows multiple tasks to run simultaneously.
A promise represents a value that might not be available yet, but will be in the future.
Promises can be in one of three states: pending, fulfilled (resolved), or rejected.
Once the promise is fulfilled, you can chain then() or catch() methods to handle success or failure.

Promise Chaining
Avoid callback functions.
Make a promise Javascript

const promise = new Promise()
Function checks age -
if 18 or older resolves the promise
if not 18 or older rejects promise
using .then to run if resolved
using .catch to run if rejected

const checkEligibility = (age) => {
  return new Promise((resolve, reject) => {
    if (age >= 18) {
      resolve("You are eligible to vote!");
    } else {
      reject("Sorry, you are too young to vote.");
    }
  });
};
checkEligibility(16) // <- Below 18 years old
  .then((message) => {
    console.log(message); // This runs if the promise is resolved
  })
  .catch((error) => {
    console.error(error); // This runs if the promise is rejected
  });
And we learned to work with PixiJS, a webgl visual library to make 2d games.

I am excited about learning about making games this week. I have a project that is using three.js to render 3d, so this week's topic is of a lot of interest to me.
My game is much more complex than this so it probably will be very simple to me, but it's still good seeing the proper way to do things and seeing if there are different ways of thinking about how to approach rendering data.