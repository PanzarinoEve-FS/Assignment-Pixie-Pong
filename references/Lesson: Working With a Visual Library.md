This final lesson for week 3 might seem like a complete shift in topics (especially coming from Promises), however it will all make sense when you start your assignment for this week. In this lesson we will be learning how to utilize a graphics library. I must say, graphics libraries are really fun to learn and use. By the end of this week you should theoretically be able to code almost any 2D game with just some simple JavaScript. So if you have always been a big fan of video games, than you should have loads of fun in this lesson. 

The graphics library we will be learning with is called PixiJS.

Introduction To Using PixiJS

PixiJS
PixiJS
What Is PixiJS?

PixiJS is an open-source, 2D rendering engine that allows developers to create rich, interactive graphics, games, and animations directly in the browser. Built using WebGL and HTML5, PixiJS takes advantage of hardware-accelerated graphics, making it incredibly fast and efficient, even for complex applications. Its main goal is to provide developers with a fast, versatile platform for creating high-performance, visually stunning 2D content while simplifying the development process. Unlike other game engines like Unity or Unreal, which are more focused on 3D and complex game logic, PixiJS is lightweight and tailored specifically for 2D rendering. It’s particularly useful when you want to build browser-based applications or games that need to render thousands of sprites or animations smoothly.

We use PixiJS because it strikes a perfect balance between performance and ease of use. JavaScript, by itself, isn't inherently optimized for rendering complex graphics, but PixiJS taps into WebGL for hardware-accelerated rendering, making it possible to push the limits of what a browser can do. It’s highly flexible and works seamlessly with HTML5, allowing developers to integrate PixiJS into various projects without steep learning curves. Whether you want to build games, interactive websites, data visualizations, or any other 2D graphical application, PixiJS is a tool that empowers developers to create rich, engaging experiences with relative ease. It also offers a range of features like texture mapping, filters, and sprite manipulation, which enable developers to add more complexity to their visual applications.

How PixiJS Works

At its core, PixiJS renders graphics using WebGL when available, falling back to HTML5 Canvas if WebGL is not supported in the browser. This ensures compatibility across different devices and browsers while maintaining optimal performance. The basic structure of a PixiJS application involves creating a renderer, a stage (which is essentially a scene graph), and then adding visual elements such as sprites, shapes, or text to the stage. These elements can be animated or interacted with, and the renderer takes care of drawing them onto the screen. In the following example you will see a simplified breakdown of how to create your first PixiJS app. 



HTML Template - Copy/paste the following HTML into its own file.
HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Module 3 | Visual Libraries | PixiJS</title>
</head>
<body>
  <game id="game"></game>
  <!-- Load in the PixiJS -->
  <script src="https://pixijs.download/release/pixi.min.js"></script>
  <!-- Our ES6 code goes here as a type="module" -->
  <script type="module">
  // PixiJS code goes here...
  </script>
</body>
</html>
JavaScript - Copy/paste the following code between the <script type="module"> tags
Javascript
// We first need to create a new PixiJS application instance
const app = new PIXI.Application();
// Now we need to call an asyncronous method on our PixiJS 
// application called "init" which initializes our display
// using a defined width and height measured in pixels.
await app.init({ 
  backgroundColor: "#3398b9", 
  width: 800, 
  height: 800 
});
// Now all we need to do is to tell PixiJS what HTMLElement
// in our DOM we want to show our game using a DOM helper
document.getElementById('game').appendChild(app.canvas);
// First lets create ourselves a new graphic instance to 
// represent our "cirlce" on the screen and assign it to 
// a variable called "circle"
const circle = new PIXI.Graphics()
// Now we can set it's "fill" color to a nice fun yellow!
circle.beginFill("#f5ef42")
// Now we need to tell Pixi to draw an actual circle 
// the three arguments we need are: X, Y, and radius size
// all measured in pixels. So this cirlce will start at 
// zero X and zero Y coordinates with a 5 pixel radius
circle.drawCircle( 400, 400, 5 )
// Now we need to call an end to our "fill" that we started 
// earlier with our "beginFill" call with the "endFill" call
circle.endFill()
// Now finally!!! Let's add our cirlce to our game stage 
// to see what happens using the addChild call
app.stage.addChild(circle)
If you did everything according to the instructions outlined above. You should see the following results when you open the .html file in your web browser. You should see the yellow circle on top of a blue background that we defined in our JavaScript code. 
Expected results in browser
Expected results in browser
Giving Our Circle Some Motion

So now that have have a circle drawn on our blue background let's assign some movement to our circle. Not only are we going to make our circle move across the screen, but we are also going to make it bounce off the edges of the blue screen. 



The Ticker

Every game developer will tell you that every video game is powered by something called a clock. Sometimes it's called a "game engine" or in the case of PixiJS: the "Ticker." Essentially a ticker in a game is just a big fancy never ending loop. In fact we very well could create our very own ticker without using PixieJS's built in ticker with the following code. Note that the code below is strictly for demonstration purposes only. We won't actually use it in favor of PixieJS's built in ticker.  
Javascript
...
app.stage.addChild(circle)
// Ticker/Game loop 
while (true) {
  // Note that this loop will never end, and 
  // that's what we want when powering a video game
  // Every time this loop gets executed we can
  // move our circle 1px to the right on our screen
  circle.x += 1
}
...
Whatever you do, do not run the code from the previous example, as it might crash your browser. This is because using while(true) { ... } creates a never-ending loop, since the value true will always evaluate to true. A while loop's purpose is to keep executing until the given condition evaluates to false.

Even though most video games run on continuous loops like this, PixiJS's ticker feature provides additional built-in tools, such as debouncing, to create more efficient game loops. If you'd like to learn more about debouncing, you can read up on it here. 





Setting Up a Ticker Inside PixieJS

Let's set up our PixiJS application code to include our new game ticker. In the following example, you'll see how we do this. The ticker accepts a single callback/arrow function that runs every time PixiJS 'ticks.' Essentially, this acts as your frame rendering callback. Each time PixiJS 'ticks,' it executes your function to render a new frame. I'm sure all you video game enthusiasts have heard the term 'frame rate.' This refers to how many times your game ticker 'ticks' per second. Typically, you'll see frame rates up to 60fps (Frames Per Second), meaning your game loop can render 60 times in one second.
Javascript
...
  
  // Now what we need to do is to add a refresh engine that 
  // powers our graphic's movement. We can use Pixi's built
  // in "ticker" feature like so...
  app.ticker.add(() => {
      // All of the following code will be executed on every
      // "frame" update/refresh
      // Lets move the circle one pixel to the right
      circle.x += 1
      // Good! Now lets also move it down one pixel
      circle.y += 1
  })
...
Awesome! Now, when we open our pixiejs.html file in the browser, you'll notice that our circle moves diagonally from the center of the screen towards the bottom right. This happens because, each time our PixiJS application 'ticks,' it increments the circle's x and y values by 1. The value of 1 represents one pixel in PixiJS. You can think of the display we initialized above the ticker as a grid.



Understanding The X, Y Coordinate Grid System

Every object (like our circle) added to our application's stage requires x and y coordinates to let PixiJS know where it is located on the grid. The X-axis runs from left to right, starting at zero (0) and extending up to the width of the application (in our case, 800px). The Y-axis runs from top to bottom. PixiJS also supports a Z-axis for basic 3D rendering, but we will only focus on the X and Y axes.
Final browser result of moving circle in PixieJS
Final browser result of moving circle in PixieJS
Hit Detection In PixeJS

Awesome job making it this far! Now let’s talk about the last part of this lesson (I promise). We’re going to cover something well-known in the video game development community: hit detection.

Hit detection occurs when two objects in a game collide. It’s a pretty simple concept, but when it comes to coding, it can be a bit more involved. In our previous example, we got the circle moving diagonally down the screen, but if you haven’t already noticed while viewing it in your browser, the circle eventually disappears from our application's blue stage.

That’s because our grid system is technically infinite. Well, technically it’s limited by the largest signed 64-bit number a computer can store, but for our purposes, we can think of the PixiJS grid system as being endless.
Sandlot the movie - Forever scene
Sandlot the movie - Forever scene
Sandlot, what a great movie! On a serious note, our circle has essentially disappeared off the edge of our 800px by 800px display area and will continue moving indefinitely until both its X and Y coordinates reach the largest signed 64-bit number possible (which equals 2^64).

What we’re going to do now is add some logic to our circle that will "detect" when it hits any of the edges of our blue stage. It’s simple, really. All we need to do is add an if condition to each tick that checks whether the circle's X or Y values are equal to or greater than 800px (the width of our stage).

If either the X or Y coordinate is equal to or greater than the width of our stage (800px), we will alert the user with the string "Hit detected: X|Y axis". Note that in the following code, we use app.ticker.stop() whenever we want to halt the ticker. If we don’t stop the ticker, you may get stuck in an endless alert prompt loop.
Javascript
...
app.ticker.add(() => {
  if (circle.x >= 800) {
    alert(`Hit detected: X axis`)
    app.ticker.stop() // Halts our ticker
  }
  if (circle.y >= 800) {
    alert(`Hit detected: Y axis`)
    app.ticker.stop() // Halts our ticker
  }
  // Lets move our circle's by their direction/velocity
  circle.x += 1
  circle.y += 1
})
...
Changing The Circle Direction

Now that we have hit detection working, we can take our application to the next level by making the circle bounce off the stage borders, completing our Pixie Pong example.

What we need to do is reverse the circle's direction on the axis where the hit was detected. For example, if the circle's x value is equal to or greater than the stage's edge (800px), we need to reverse the direction the circle is traveling. The same applies to the Y axis.



Circle Velocity

We are going to use a physics concept called velocity. Velocity tells us how fast an object is moving through space. Here’s the key point: in our example, velocity can be both positive and negative. For instance, if we want the circle to travel along the X axis to the right at a rate of 1 pixel per tick, we set the velocity to a positive value of 1. Now, if we want the circle to switch directions and travel left at the same rate, we change the velocity to a negative value (-1). Make sense?

Let’s look at the following example, where we declare two variables outside our ticker function that will act as our "velocity" values for both the X and Y axes. Anytime the circle hits the edge of either axis, we will flip the velocity value from positive to negative, and vice versa.

Below is the final code, along with a working example demonstrated in my browser. Note that I adjusted the circle’s initial X and Y coordinates to prevent it from bouncing perfectly in a diagonal direction.
Javascript
// We first need to create a new PixiJS application instance
const app = new PIXI.Application();
// Now we need to call an asyncronous method on our PixiJS 
// application called "init" which initializes our display
// using a defined width and height measured in pixels.
await app.init({ 
  backgroundColor: "#3398b9", 
  width: 800, 
  height: 800 
});
// Now all we need to do is to tell PixiJS what HTMLElement
// in our DOM we want to show our game using a DOM helper
document.getElementById('game').appendChild(app.canvas);
// First lets create ourselves a new graphic instance to 
// represent our "cirlce" on the screen and assign it to 
// a variable called "circle"
const circle = new PIXI.Graphics()
// Now we can set it's "fill" color to a nice fun yellow!
circle.beginFill("#f5ef42")
// Now we need to tell Pixi to draw an actual circle 
// the three arguments we need are: X, Y, and radius size
// all measured in pixels. So this cirlce will start at 
// zero X and zero Y coordinates with a 5 pixel radius
circle.drawCircle( 5, 5, 5 )
// Now we need to call an end to our "fill" that we started 
// earlier with our "beginFill" call with the "endFill" call
circle.endFill()
// Now we need to tell our circle where to "spawn" on our 
// game display by setting it's "view" x and y coordinates
circle.x = 450 
circle.y = 400 
// This will determine our circle's "velocity" and direction
let xv = 1
let yv = 1
    
// Now finally!!! Let's add our cirlce to our game stage 
// to see what happens using the addChild call
app.stage.addChild(circle)
// Now what we need to do is to add a refresh engine that 
// powers our graphic's movement. We can use Pixi's built
// in "ticker" feature like so...
app.ticker.add(() => {
  // All of the following code will be executed on every
  // "frame" update/refresh
  // If our circle exceeds 800px or is less than zero
  if (circle.x >= 800 || circle.x <= 0) {
    // This will flip a postitive number to a negative
    // as well as a negative to a positive due to the 
    // "math of signs" property in mathematics which 
    // states two negatives equals a positive
    xv = -xv 
  }
      
  // Rinse and repeat for our Y axis
  if (circle.y >= 800 || circle.y <= 0) {
    yv = -yv
  }
  // Lets move our circle's by their direction/velocity
  circle.x += xv
  circle.y += yv 
})


