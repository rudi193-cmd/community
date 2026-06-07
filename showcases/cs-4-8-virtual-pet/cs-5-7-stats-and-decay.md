# Stats and Decay

**Grade Range:** 5–7  
**Time:** 45–60 minutes  
**Format:** Discussion + guided hands-on  
**Materials:** Devices with a browser and text editor; working pet file from previous session

---

## The Big Question

If nobody plays with your pet, what happens to it?

---

## The Setup (5 min)

Students have a pet on screen. It moves. It has a body and eyes. But it doesn't need anything yet. It just runs forever, completely fine, no matter what.

Ask: is that a pet?

Most students will say no. A pet needs you. If you forget about it, something changes.

Today they build the part of the program that makes that true.

---

## Variables as Stats (10 min)

Start by adding three variables to the top of the script:

```javascript
let food   = 80;
let happy  = 70;
let energy = 50;
```

Ask: what do these numbers mean?

Each one is a stat — a measurement of how the creature is doing right now. 100 is perfect. 0 is as bad as it gets.

Ask: if food is 80 right now, and nobody feeds the creature, what should it be in five minutes? Lower. Much lower? A little lower?

This is a design decision. The student decides how hungry their creature gets over time. A creature that gets hungry in five minutes is needy. A creature that goes a week without food is low-maintenance. Both are valid. The code doesn't care — but the design does.

---

## The Decay Loop (15 min)

Here's how to make a stat go down over time:

```javascript
setInterval(function() {
  food   = Math.max(0, food   - 2);
  happy  = Math.max(0, happy  - 1);
  energy = Math.max(0, energy - 1.5);
}, 5000);
```

Walk through each piece:

- `setInterval(fn, ms)` — calls the function every `ms` milliseconds. 5000ms = 5 seconds.
- `food - 2` — food drops by 2 every 5 seconds
- `Math.max(0, ...)` — makes sure food never goes below 0. Without this, it would go negative forever.

Ask: what happens if you change 5000 to 1000? What about 10000?

Let students experiment. The decay rate is their design decision.

---

## Drawing the Stat Bars (10 min)

Stats that only exist as variables aren't visible. Add bars to show them.

Add this HTML above the canvas:

```html
<div id="bar-food"   style="width:80%; height:12px; background:green;"></div>
<div id="bar-happy"  style="width:70%; height:12px; background:gold;"></div>
<div id="bar-energy" style="width:50%; height:12px; background:dodgerblue;"></div>
```

Then add a function that updates the bars, and call it from inside the interval:

```javascript
function updateBars() {
  document.getElementById('bar-food').style.width   = food   + '%';
  document.getElementById('bar-happy').style.width  = happy  + '%';
  document.getElementById('bar-energy').style.width = energy + '%';
}
```

Updated interval:

```javascript
setInterval(function() {
  food   = Math.max(0, food   - 2);
  happy  = Math.max(0, happy  - 1);
  energy = Math.max(0, energy - 1.5);
  updateBars();
}, 5000);
```

Save and refresh. The bars slowly drain.

---

## What Happens at Zero (10 min)

Stats that go to zero and do nothing aren't very interesting. Ask: when food hits zero, what should happen?

Students will have different answers. That's correct — this is a design decision. Common options:

- The creature looks sad
- The creature moves more slowly
- The creature gets sick

Show how to check in the draw function:

```javascript
if (food < 20) {
  ctx.fillStyle = 'gray'; // creature looks pale
} else {
  ctx.fillStyle = creatureColor; // normal color
}
```

This is a *conditional* — a check that runs every frame. When food is low, the draw function does something different.

Ask: what would you check for? What does *your* creature do when it's hungry?

Students add one visible change that happens when at least one stat gets low. It doesn't have to be complex — a color change, a slower speed, a face change. One thing.

---

## One Stat Per Design (remaining time)

Students choose which stat their creature cares about most. If they designed a high-energy creature, energy decays fastest. If they designed a creature that needs lots of attention, happy decays fastest.

The numbers should match the design from lesson one. A student who wrote "it gets hungry really fast" should set a food decay of 5 or more. A student who wrote "it's very self-sufficient" should set a food decay of 0.5.

This is a good moment to bring out the design sheet from lesson one. Ask: does your decay match what you said your creature would be like?

---

## Closing Question

Ask: what is `Math.max(0, food - 2)` protecting against?

Students who get it: the number going negative, which would make the bar disappear off the left side of the screen and mean nothing.

Students who don't get it yet: take food out of `Math.max` temporarily. Watch the bar disappear. Put it back.

Sometimes the best explanation is the broken version.

---

## Assessment

Student has:
- At least two stats as variables (0–100)
- A `setInterval` that decrements them over time
- Visible bars (or some other visual) that reflect the current values
- One visible change to the creature when a stat gets low

---

## Differentiation

**For students who finish early:** Add a fourth stat of their choosing — something specific to their creature. Gerald has ROTATION. Ivy has a feather-ruffling stat. What does *their* creature track that's unique?

**For students who are stuck:** Focus on one stat and one bar. Food only. Get that working and looking right. The other stats are the same pattern.

**For students who want more:** Add a `console.log(food, happy, energy)` inside the interval. Open the browser's developer tools (F12 → Console). They can watch the numbers change in real time — this is how programmers debug stat systems.

---

## Teacher Notes

The `Math.max(0, ...)` pattern is worth dwelling on. It introduces the idea that code needs to account for edge cases — what happens at the boundaries. A number that goes negative would still "work" technically but would produce nonsense in the display. This is a simple example of defensive programming: knowing what the valid range is and enforcing it.

Students will immediately ask how to feed the creature to bring food back up. That's the next lesson. Tell them the interval is what drains it; a button will be what refills it. Let them feel the asymmetry — the world drains things; you are the one who refills.
