# Simon Game 🎮

A simple Simon memory game built using HTML, CSS, JavaScript, and jQuery.

## How to Play
- Press any key to start.
- Watch the pattern of colors.
- Click the colors in the same order.
- The sequence increases every level!

## Preview
(You can add a GitHub Pages link here later)

## Technologies Used
- HTML5
- CSS3
- JavaScript
- jQuery

### ✅ PART 1: VARIABLES

```js
var buttonColours = ["red", "blue", "green", "yellow"];
```

* This array holds the possible colors of the buttons.
* These will be randomly chosen by the game to create a sequence.

```js
var gamePattern = [];
```

* This will store the **correct pattern** (sequence) that the game generates.

```js
var userClickedPattern = [];
```

* This stores the **pattern of colors the user clicks**.
* It resets every level.

```js
var started = false;
```

* This flag keeps track of whether the game has started.
* It prevents re-triggering the game on multiple keypresses.

```js
var level = 0;
```

* Keeps track of the current level.
* Starts from level 0 and increases after each correct sequence.

---

### ✅ PART 2: START GAME ON KEY PRESS

```js
$(document).keypress(function() {
  if (!started) {
    $("#level-title").text("Level " + level);
    nextSequence();
    started = true;
  }
});
```

* When any key is pressed:

  * If game hasn’t started:

    * Set the title to `Level 0`
    * Call `nextSequence()` to begin
    * Set `started` to `true` so it doesn't restart again

---

### ✅ PART 3: WHEN USER CLICKS A COLOR

```js
$(".btn").click(function() {
  var userChosenColour = $(this).attr("id");
  userClickedPattern.push(userChosenColour);

  playSound(userChosenColour);
  animatePress(userChosenColour);

  checkAnswer(userClickedPattern.length - 1);
});
```

* When a color button is clicked:

  1. Get the button's ID (`red`, `green`, etc.)
  2. Add it to `userClickedPattern`
  3. Play sound for that color
  4. Animate the press
  5. Call `checkAnswer()` with the index of the latest click to verify if it matches

---

### ✅ PART 4: CHECK USER'S ANSWER

```js
function checkAnswer(currentLevel) {
  if (gamePattern[currentLevel] === userClickedPattern[currentLevel]) {
```

* Checks if the **latest user click** matches the correct color in `gamePattern` at that same position.

```js
    if (userClickedPattern.length === gamePattern.length){
```

* If the user has finished clicking the **entire pattern correctly** (same length):

  * Move to next level after 1 second.

```js
      setTimeout(function () {
        nextSequence();
      }, 1000);
```

```js
    }
  } else {
```

* ❌ If the color does **not** match the pattern:

  * Play the “wrong” sound.
  * Add `game-over` class to flash red.
  * Update title to show Game Over.
  * Remove red flash after 200ms.
  * Reset everything via `startOver()`.

```js
    playSound("wrong");
    $("body").addClass("game-over");
    $("#level-title").text("Game Over, Press Any Key to Restart");

    setTimeout(function () {
      $("body").removeClass("game-over");
    }, 200);

    startOver();
  }
}
```

---

### ✅ PART 5: GENERATE NEXT SEQUENCE

```js
function nextSequence() {
  userClickedPattern = [];
```

* Clear the user’s previous clicks (start fresh for the new level).

```js
  level++;
  $("#level-title").text("Level " + level);
```

* Move to the next level and show it in the title.

```js
  var randomNumber = Math.floor(Math.random() * 4);
  var randomChosenColour = buttonColours[randomNumber];
  gamePattern.push(randomChosenColour);
```

* Pick a random color from the list and add it to the `gamePattern`.

```js
  $("#" + randomChosenColour).fadeIn(100).fadeOut(100).fadeIn(100);
  playSound(randomChosenColour);
}
```

* Animate and play sound for the new color in the sequence.

---

### ✅ PART 6: ANIMATION FUNCTION

```js
function animatePress(currentColor) {
  $("#" + currentColor).addClass("pressed");
  setTimeout(function () {
    $("#" + currentColor).removeClass("pressed");
  }, 100);
}
```

* Adds a pressed effect (like a flash) when user clicks a button.
* Removes it after 100 milliseconds.

---

### ✅ PART 7: SOUND FUNCTION

```js
function playSound(name) {
  var audio = new Audio("sounds/" + name + ".mp3");
  audio.play();
}
```

* Plays the sound file for the given color name.
* `"sounds/red.mp3"`, `"sounds/wrong.mp3"`, etc.

---

### ✅ PART 8: RESET THE GAME

```js
function startOver() {
  level = 0;
  gamePattern = [];
  started = false;
}
```

* Resets the game:

  * Sets level back to 0
  * Clears the game pattern
  * Allows the user to start again with keypress

---

## 💡 Summary Flow

| Event           | What Happens                                              |
| --------------- | --------------------------------------------------------- |
| Key press       | Starts game and shows 1st color                           |
| User click      | Captures input, plays sound, animates, checks correctness |
| Pattern correct | Proceeds to next level                                    |
| Pattern wrong   | Shows Game Over and resets                                |
