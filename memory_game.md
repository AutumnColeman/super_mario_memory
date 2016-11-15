# Monster Memory Game

You will make a memory card game where the player flips over two cards at a time to try to find matching cards. If the 2 cards he flips over during a particular turn are the same, they match, and they remain open. If they don't match, they must be flipped back closed. The player plays until all cards are open.

## Step 0: Setup

Create a basic AngularJS application setup:

1. Make an HTML file.
2. Link to angular.js
3. Create a memorygame.js file
4. Link to memorygame.js
5. Create a style.css
6. Link to style.css
7. Create an NG app module
8. Create an NG controller within the app module
9. Link the app module and the controller to an element on the page.

## Step 1: Set up board data

The images directory has a set of monster images, you will use them as the faces of the cards. For the time being, you will hard code the arrangement of the cards to make a 4 x 2 grid. In a later step, you will randomize them.

Create a scope variable `cards` and initialize it to a two-dimensional array representing a grid of card objects. Each card object will contain:

* `url` - location of the image file used to render the card
* `open` - a boolean that determines whether the card's face is up
* `matched` - a boolean that determines whether the card has been matched

I recommend that you create a `Card` type by creating a constructor.

## Step 2: Render the board

Now that you have a two-dimensional array of cards, you can render the board. Layout the grid using a table element or any way you choose. You will use the `ng-repeat` directive to dynamically place each card in the two-dimensional array on the page. For now, display each card place up using an `img` element.

## Step 3: Open vs closed

Set the open state of each card object to closed, initially. In the template, show the card image if and only if the card's state is open. Display the logo.png in the back of the card when the card is closed. When the card is clicked, change it's open state to true and show the card.

## Step 4: Setup matching cards

Arrange your cards to make sure that every card has a matching buddy.

## Step 5: Card 1 vs Card 2 and Finite State Machines

An important logic piece in the game is that the game engine has to do a different thing depending on whether it's waiting for the first card in the turn or the second. You will implement this using the concept of state machines: https://en.wikipedia.org/wiki/Finite-state_machine

Imagine that the game can be in one of two states:

* waiting for the first card - call this the "first" state, or
* waiting for the second card - call this the "second" state

You can represent the state by using a scope variable to store a string value such as "first" or "second". You may even display the current state on the page for debugging purposes.

When the game is in the "first" state, and a card is clicked, it will:

1. flip open the card
2. save the card in a scope variable, call it `firstCard`
3. switch the state to "second"

When the game is in the "second" state, and a card is clicked, it will:

1. flip open the card
2. see if the clicked card has the same face as `firstCard` from the first step
  * if they match:
    1. change the matched property of both cards to true
    2. switch the state back to "first"
  * if they don't match:
    1. switch the state back to "second"
    2. wait momentarily so the player sees both cards
      3. flip the cards back over

To wait momentarily, you will want to use Angular's `$timeout` service instead of `setTimeout` because that allow's Angular's object difference checking to be notified when something has changed as a result of something happening within the callback function. To use `$timeout`, you have to pass it in as an additional parameter (in additional to `$scope`) into your controller function:

```
app.controller('MyController', function($scope, $timeout) {

});
```

And then when you want to wait momentarily, you would write:

```
$timeout(function() {
  console.log('This happened one second later');
}, 1000);
```

Basically the usage is identical to `setTimeout`.

## Step 6: Check win

Now you should be able to play the whole game until all the cards have been flipped open. Write a function to determine whether or not the player has won the game - it should return true for a win. You may use functional programming with the help of the [every](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every) or [some](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some) method.

## Bonus: Card flip animation

Use an animation to flip the cards over like: https://davidwalsh.name/demo/css-flip.php
Read this tutorial to figure out how to do it: https://davidwalsh.name/css-flip

## Step 7: Randomize the board

Randomize the board so that you get a different game every time. You have a total of 16 monster cards at your disposal. Every card in a game must have a match. For example: if you want to build a 4 x 2 board which has 8 cards in it total, you need 4 monster faces. So you want to

1. Select 4 monster faces randomly from the 16.
2. Duplicate each of the 4 selected cards to get 8 cards total.
3. Shuffle the cards.
4. Arrange the cards in a 4 x 2 two-dimensional array.

Hint: you might find a use for the `shuffle` function we used in the black jack game.

## Step 8: Different game board sizes

Before the start of the game, give the player a choice between 3 different game board sizes:

* 4 x 2 - easy
* 6 * 3 - medium
* 8 x 4 - hard

The selection menu will appear to be a different page. When they make the selection, it switches to the game board page to start the game. You can do this by using `ng-show` and `ng-hide` to show vs hide the respective DOM elements.
