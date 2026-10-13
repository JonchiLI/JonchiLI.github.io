---
title: Ai Game Autopsy
---

# Code Autopsy: Dodge the Blocks

**Project:** AI-generated browser game  
**Language:** HTML, CSS, and JavaScript  
**Goal:** Understand how the game's code works and test how a deliberate change affects gameplay.

## 1. The Canvas

```html
<canvas id="gameCanvas" width="600" height="400"></canvas>
```

The canvas is in the HTML body. It is 600 pixels wide and 400 pixels tall. JavaScript draws the player, falling blocks, and score on it. If I increase the width, the game has more horizontal space. If I increase the height, the blocks have farther to fall before reaching the bottom.

## 2. State Variables

```javascript
const player = { x: 275, y: 350, width: 50, height: 20, speed: 6 };
let blocks = [];
let score = 0;
let gameOver = false;
let frameCount = 0;
const keys = {};
```

These variables store information the game needs while running. The player's position and speed determine how it moves. The `blocks` array stores the falling blocks, `score` tracks points, and `gameOver` tells the game whether the player has lost. `frameCount` helps decide when to create new blocks, and `keys` records which keys are pressed.

## 3. The Game Loop

```javascript
function gameLoop() {
  update();
  draw();
  requestAnimationFrame(gameLoop);
}

gameLoop();
```

The `gameLoop()` function repeats using `requestAnimationFrame()`, which schedules another frame. Each time, `update()` changes the game state and `draw()` redraws the screen. The browser controls the timing, usually aiming for smooth animation around 60 frames per second on a typical display.

## 4. Input Handling

```javascript
document.addEventListener("keydown", (event) => {
  keys[event.key] = true;
});

document.addEventListener("keyup", (event) => {
  keys[event.key] = false;
});
```

These event listeners detect when a key is pressed or released. They update the `keys` object so the game knows which direction I want to move. The `update()` function checks whether the left or right arrow key is held and changes the player's x-position.

## 5. Collision and Game-Over Logic

```javascript
function rectanglesOverlap(a, b) {
  return a.x < b.x + b.size &&
         a.x + a.width > b.x &&
         a.y < b.y + b.size &&
         a.y + a.height > b.y;
}

// Inside update():
if (rectanglesOverlap(player, block)) gameOver = true;
```

The game checks whether the player overlaps a falling block. The function compares the edges of the two rectangles. If they overlap, `gameOver` becomes `true`. The `update()` function then stops changing the game state, and the `draw()` function displays the game-over message.

## 6. Scoring

```javascript
const before = blocks.length;
blocks = blocks.filter(block => block.y < canvas.height);
score += before - blocks.length;
```

The game first records how many blocks exist. It removes blocks that have fallen past the bottom of the canvas. The difference between the old and new number of blocks tells the game how many blocks were removed, and that number is added to the score. This means I earn points by successfully avoiding blocks until they pass the bottom.

## 7. Deliberate Code Change

**Change tested:** I changed the player's speed from `6` to `10`.

```javascript
const player = { x: 275, y: 350, width: 50, height: 20, speed: 10 };
```

The player should move farther during each update, so it can dodge blocks more quickly. This shows that changing one variable can affect gameplay without rewriting the entire game.

**Observation:** After running both versions, I would compare how quickly the player crosses the canvas and whether dodging feels easier. I should record what I actually observe after testing the change.

## Conclusion

This activity helped me see that a game is made of smaller systems that work together. Variables store information, event listeners handle player input, the game loop updates and redraws the screen, and collision detection determines when the game ends. Testing a change to the player's speed helped me understand how code controls the actual game experience.

