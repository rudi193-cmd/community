# Buttons and Interaction

**Grade Range:** 5–7  
**Time:** 45–60 minutes  
**Format:** Guided hands-on  
**Materials:** Devices with a browser and text editor; pet file with stats and decay from previous session

---

## The Big Question

How do you make a program do something when you click?

---

## The Setup (5 min)

The pet has stats. They drain over time. But there's no way to help it yet.

Ask: what's missing?

Students will say: a way to feed it. A button. Something to click.

Today they build that. A button is just HTML. What the button *does* is JavaScript. Today both pieces come together.

---

## The Feed Button (10 min)

Add a button to the HTML, below the canvas:

```html
<button onclick="feed()">🍎 Feed</button>
```

Now add the function in the script:

```javascript
function feed() {
  food = Math.min(100, food + 20);
  updateBars();
}
```

Walk through it:

- `onclick="feed()"` — when the button is clicked, call the `feed` function
- `food + 20` — adds 20 to the current food value
- `Math.min(100, ...)` — makes sure food never goes above 100
- `updateBars()` — tells the bars to redraw

Save and refresh. Click the button. The food bar goes up.

Ask: why `Math.min(100, ...)` instead of just `food + 20`? What would happen if food was at 95 and you clicked Feed?

Without the clamp, food would go to 115 and the bar would overflow past 100%. The bar is supposed to represent a percentage — it needs a ceiling.

---

## The Pattern (5 min)

The feed button is a pattern. Every button follows the same shape:

1. HTML button with `onclick`
2. A JavaScript function with the same name
3. The function changes a variable
4. The function calls something to update the display

This pattern repeats for every action in every pet. Students who understand the pattern can build any button.

Write it on the board as a template:

```
HTML:       <button onclick="ACTIONNAME()">Label</button>
JavaScript: function ACTIONNAME() {
              STAT = Math.min(100, STAT + AMOUNT);
              updateBars();
            }
```

---

## Play and Rest (10 min)

Students add two more buttons using the same pattern. Give them the structure, let them fill in the numbers:

```html
<button onclick="play()">✨ Play</button>
<button onclick="rest()">💤 Rest</button>
```

```javascript
function play() {
  happy = Math.min(100, happy + 15);
  energy = Math.max(0, energy - 10); // playing costs energy
  updateBars();
}

function rest() {
  energy = Math.min(100, energy + 25);
  updateBars();
}
```

Point out the `play` function: it raises happy *and* costs energy. Actions can affect multiple stats. Playing is fun but tiring. This is a design decision, and it makes the pet feel more like a real system — things connect.

Ask: does your creature's version of play cost energy? Should it? What if your creature *gets* energy from playing?

---

## Making the Creature Respond (10 min)

Right now, clicking a button updates a number and a bar. The creature itself doesn't react.

Add a reaction to the draw loop. Inside the `draw` function, check the pet's state and change what you draw:

```javascript
// inside draw(), before drawing the body:
if (happy > 80) {
  // bounce a little
  y = 150 + Math.abs(Math.sin(Date.now() / 200)) * -15;
} else {
  y = 150;
}
```

When happy is high, the creature bounces. When it drops, it stays still.

Ask: what does `Date.now()` return? A number that keeps going up — the current time in milliseconds. `Math.sin` of that number oscillates between -1 and 1, which makes a smooth up-and-down motion. `Math.abs` keeps it from going the wrong direction.

Students add one visual reaction to at least one of their buttons. It doesn't have to be complex. A color change, a size change, a speed change — one thing that the creature does when you click.

---

## Testing the Loop (5 min)

Ask students to close their eyes for 30 seconds while the pet runs.

When they open: what's happened to the bars?

Some stats have drained. Now they have to bring them back up. This is the first time they've felt the tension the design was meant to create: the interval drains, the buttons restore. The pet needs them.

Ask: is it easy or hard to keep the pet happy? If it's too easy, the decay is too slow. If it's impossible, the decay is too fast. Adjust.

This is balance testing — a real game development skill.

---

## Closing Question

Ask: what is the difference between an event and a function?

The event is the thing that happens — the click. The function is what the code does in response. An event alone does nothing. A function with no event never runs. They need each other.

---

## Assessment

Student has:
- At least two working buttons that change stats
- At least one button that affects two stats (an action with a cost)
- At least one visible change in the creature when a button is clicked

---

## Differentiation

**For students who finish early:** Add a special action that's unique to their creature. Gerald has `*rotates*`. Ivy has `*ruffles feathers*`. What does their creature do that nothing else does? Build that button.

**For students who are stuck:** Get one button working completely before adding others. Feed → updateBars → see bar go up. That's the loop. Everything else is the same pattern.

**For students who want more:** Add a check in the feed function — if food is already above 90, the creature is too full and refuses to eat. Show a message. This is the first piece of conditional logic inside an action function.

---

## Teacher Notes

The "playing costs energy" moment is often where students start to feel their pet as a *system* rather than a collection of buttons. When they realize that clicking Play has a consequence — energy goes down — they start planning: maybe I should let it rest before playing again.

That's the loop the whole project is built on. Name it: you designed a creature that needs things. You built the drain. You built the refill. The thing you built has *needs* now. It's not a toy. It's a system.

Students will ask about saving — what happens if they close the tab. That's the next lesson. Let them experience the loss first.
