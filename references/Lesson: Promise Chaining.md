A Promise in JavaScript is an object that represents the eventual completion or failure of an asynchronous operation. It acts as a placeholder for a value that might not yet be available but will be at some point in the future. Promises have three main states: pending, fulfilled (resolved), and rejected. When a promise is in the "pending" state, it means the operation has started but is not yet complete. If it successfully completes, the promise moves to the "fulfilled" state, resolving with a value. If the operation encounters an error, the promise moves to the "rejected" state, typically providing an error message.

Promises offer a clean and modern way to handle asynchronous tasks compared to older callback-based techniques. By using methods like .then() and .catch(), you can manage what happens after a promise is fulfilled or rejected, respectively. For example, if you’re fetching data from an API, you can use a promise to wait for the data to be available, then proceed with processing it once it's ready. This way, instead of blocking the execution of other code while waiting for the data, the rest of the program can continue running, improving overall efficiency and responsiveness.

Life Before JavaScript Promises

In a life long, far away from this time before there were such things as JavaScript Promises we would have to nest our JavaScript code many levels deep. Forming what frontend developers referred to as "Callback Hell." Yup, it actually had a name. It's almost the same issue as "divitous" when developers nest too many "div" tags, but the JavaScript equivalent. I figured what better way to learn the importance of using promises in JavaScript without first being exposed directly to the issue it fixed.
Javascript
// This function gets called after our 4 async calls
function onUserUpdate() {
}
$.get(`/users/1`, function(user) {
  ...
  $.get(`/users/1/settings`, function(settings) {
    ...
    $.post(`/users/1/setttings`, function(results) {
      ...
      $.post(`/users/1`, function(results) {
        ...
        onUserUpdate();
      })
    })
  })
})
Look at that code! Go on, don’t look away!! Nope! Keep staring at it! 🤢🤮 Take a good, long gander! You can’t call yourself a developer until you’ve experienced the pure agony of wanting to gouge your eyes out at the sight of Callback Hell! I am truly sorry, but it had to be done. You will be a better developer for having gone through this experience, I promise (ha! pun intended).

Now We Got That Over With



Phew! Now that you've witnessed some of the awful asynchronous JavaScript code that used to plague the internet, it’s time to dive into how Promises resolve this issue (pun intended). In the code below, we can declare a promise simply by creating a new instance of the Promise class that JavaScript provides by default.


Javascript
const promise = new Promise()
Using Resolve & Reject Arguments



Okay, so the previous code example was a bit minimal (I know). Let’s take it a step further in the next example. Anytime we create a new instance of a Promise, we should pass two main arguments into the constructor function of the Promise class. The first is called resolve, and the second is called reject.



Argument 
Type
Description
resolve                         	
function( ...args )                                    	
This is equivalent to a callable function. This function should be called only after the asynchronous code has finished and has not encountered any errors.

...args – This represents all the data you want to return to your callable function. You can specify as many arguments as you want to pass down the chain.

reject	function( ...args )	
This is equivalent to a callable function. This function should be called only if the asynchronous code fails.

...args – This represents all the data you want to return to your callable function. You can specify as many arguments as you want to pass down the chain.
Okay, so we have the definition of both our resolve and reject arguments. Now let’s put it all together in the following code example. We’ll start with some normal synchronous (non-async) code. Technically, our promise will be resolved immediately after we run it.

 
Javascript
const message = new Promise((resolve, reject) => {
  resolve(`Foo bar!`)
})
console.log(message)
// Expected Output -> Promise { `Foo bar!` }
So notice how we got back:

   Promise {  `Foo bar!`  }
Instead of getting a string back, we got the Promise object instance with the resolved string "Foo bar!". This happens because we never explicitly asked for the value contained inside the promise. To extract this value from our promise, we need to call a method on the Promise class called .then().

When .then() is called on a promise, it accepts a single function. This function is the same one that gets called when we run resolve(). It’s important to understand that resolve() and the function passed into .then() are linked. This allows us to build what’s called a Promise chain. Inside our asynchronous code, we can always call resolve(), which in turn triggers the function passed into .then() on the Promise instance.

In a way, a Promise always maintains a reference back to the main synchronous process. Going back to our grocery store analogy, it’s like you and your friend splitting up during checkout but still having a cell phone to stay in contact. The asynchronous code always has a way to pass data back to the main synchronous code.

Now, let’s grab the value from our Promise using the .then() method, which takes a single function as an argument. This function will be called when the Promise executes resolve().
Javascript
const message = new Promise((resolve, reject) => {
  resolve(`Foo bar!`)
})
message.then((messgae) => {
  console.log(message)
  // Expected Output -> `Foo bar!`
})
Rejecting Promise



The other argument available to us in a Promise is reject(). This function should only be used when the Promise fails to resolve or encounters an error. The reject() function works in the same way that resolve() does—it technically still resolves the Promise. However, instead of the data being passed through to the .then() method, it is passed to the .catch() method. Let’s take a look at an example of what happens when a Promise encounters an error.


Javascript
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
As you can see in our previous example, the code got a bit more complex, but the fundamentals remain the same. In this case, we created a function that returns a promise, which checks the age passed into its wrapper function.

In the example, we pass in the value of 16, which triggers the promise to invoke the reject() function, leading us to the .catch() method later on, in lines 15 through 17.

Constructing a Promise Chain



Now we’ll dive into promises in their ultimate form. Promises become incredibly powerful when we start chaining them together. For example, if we have four different asynchronous steps that need to be executed one after the other, we can simply use a promise chain. Imagine we have four steps outlined below, each one depending on the previous step and its data. We can construct a promise chain to handle this sequence for us. Once again, I’ll demonstrate this using simple synchronous code for the sake of clarity.


Javascript
const words = ["Foo!", "Bar!", "Biz", "Baz!"]
// This function generates a Promise that strips a word off from the array
function removeOneWordFromList(words)
{
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log(`Removed word: `, words.shift())
      resolve(words)
    }, 500)
  })
}
                  // Removes "Foo!"
const wordList = removeOneWordFromList(words)
                  // Removes "Bar!"
                  .then((words) => removeOneWordFromList(words))
                  // Removes "Biz!"
                  .then((words) => removeOneWordFromList(words))
                  // Removes "Baz!"
                  .then((words) => removeOneWordFromList(words))
                  // Console logs whats left in the array of words
                  .then((words) => console.log(words))


In the example above, we start with an array of words. We construct a promise chain using a single Promise wrapper function called removeOneWordFromList. Each time the function is called, it returns a promise that attaches itself to the last promise. Essentially, with each new promise in the chain, one word is removed from our list and then passed down to the next promise in the chain.