# Save Your Pet

**Grade Range:** 5–7  
**Time:** 45–60 minutes  
**Format:** Discussion + guided hands-on  
**Materials:** Devices with a browser and text editor; pet file with stats and buttons from previous session

---

## The Big Question

What happens to your pet when you close the tab?

---

## The Setup (5 min)

Ask students to close their pet's tab right now. Not refresh — close it completely.

Now open it again.

What happened to the stats? They're back to whatever numbers were in the code. All the changes — the feeding, the playing, the stats that drained — gone.

Ask: is that okay?

Most students will say no. A pet that forgets everything when you close it isn't much of a pet.

Today they fix that.

---

## Where Programs Store Things (10 min)

When a program runs, it holds values in memory — the fast, temporary storage inside the computer. Memory is fast. It's also gone the moment the program stops.

If you want something to survive after the program closes, you need to write it somewhere more permanent. There are many ways to do this: databases, files, cloud services. For a web pet running in a browser, there's a simple one built in: **localStorage**.

localStorage is a small notebook the browser keeps for each website. It survives tab closes, browser restarts, and even reboots. It has one limitation: it only stores text strings, not numbers or objects directly.

Write this on the board:

```javascript
// writing to the notebook
localStorage.setItem('food', '80');

// reading from the notebook
let food = localStorage.getItem('food');
```

The key (`'food'`) is the label. The value is always a string.

---

## Saving a Number (10 min)

Numbers are easy. Add a save function:

```javascript
function save() {
  localStorage.setItem('pet-food',   String(food));
  localStorage.setItem('pet-happy',  String(happy));
  localStorage.setItem('pet-energy', String(energy));
}
```

And a load function:

```javascript
function load() {
  let savedFood = localStorage.getItem('pet-food');
  if (savedFood !== null) {
    food   = parseFloat(localStorage.getItem('pet-food'));
    happy  = parseFloat(localStorage.getItem('pet-happy'));
    energy = parseFloat(localStorage.getItem('pet-energy'));
    updateBars();
  }
}
```

Call `load()` once when the page starts (before the draw loop), and call `save()` inside the decay interval:

```javascript
load(); // runs once on page load

setInterval(function() {
  food   = Math.max(0, food   - 2);
  happy  = Math.max(0, happy  - 1);
  energy = Math.max(0, energy - 1.5);
  updateBars();
  save(); // save after every decay tick
}, 5000);
```

Save and refresh. Feed the creature a few times. Close the tab. Open it again.

The stats are still there.

---

## Why `parseFloat` (5 min)

Ask: what does `localStorage.getItem` return?

A string. Always a string. `'80'` is not the same as `80`. If you try to do math with `'80'`, JavaScript will sometimes do what you expect and sometimes not. `parseFloat` converts the string back to a real number.

Show the difference:

```javascript
'80' + 20    // → '8020'  (string + number = concatenation)
80   + 20    // → 100     (number + number = addition)
parseFloat('80') + 20  // → 100  (converted first)
```

This surprises students every time. The string `'80'` looks like a number, acts like a number in some situations, and breaks silently in others. `parseFloat` is the defensive move.

---

## Saving a Whole Object (optional, 10 min)

For students who have more than three stats, saving each one separately gets repetitive. Show how to save everything at once using JSON:

```javascript
function save() {
  let data = { food: food, happy: happy, energy: energy };
  localStorage.setItem('my-pet', JSON.stringify(data));
}

function load() {
  let raw = localStorage.getItem('my-pet');
  if (raw) {
    let data = JSON.parse(raw);
    food   = data.food;
    happy  = data.happy;
    energy = data.energy;
    updateBars();
  }
}
```

`JSON.stringify` converts an object to a string. `JSON.parse` converts it back. The object can have as many fields as needed.

This is the same pattern used in every production web app that stores user data locally.

---

## The Inspector (5 min)

Show students how to see what's in localStorage right now.

Open browser developer tools (F12). Go to Application → Local Storage → localhost.

Every key and value the pet has saved is visible there. Students can see their own data — and edit or delete it directly.

Ask: if you delete the entry in the inspector and refresh, what happens?

The `if (savedFood !== null)` check catches it — no saved data means start fresh with the code defaults.

This is a useful debugging tool. It's also a lesson: stored data isn't hidden. Anyone who opens the developer tools can see it. For a pet game, that's fine. For a password, it would not be.

---

## Closing Question

Ask: when should a program save?

Common answers: every time something changes, every few seconds, when you click a save button, when you close the tab.

All valid, with tradeoffs:

- *Every change*: most accurate, most writes — fine for a small game
- *On an interval*: simple, predictable, loses a few seconds of changes on crash
- *On close*: risky — browsers don't always fire the close event reliably

The pet saves on its decay interval. That's every few seconds. For a pet game, that's more than enough.

---

## Assessment

Student has:
- `save()` function that writes all stats to localStorage
- `load()` function that reads them back
- `load()` called on page start
- Pet survives a tab close and reopen with correct stats

---

## Differentiation

**For students who finish early:** Save the creature's name and color too. Use the JSON approach. When the page loads, the creature's customizations come back as well.

**For students who are stuck:** Get `food` saving and loading first. One stat, one key, one `setItem`, one `getItem`. Once that works, the others are the same pattern.

**For students who want more:** Add a "Reset" button that clears localStorage and reloads the page. `localStorage.removeItem('my-pet')` followed by `location.reload()`. Let them think about when a player might want to start over.

---

## Teacher Notes

The close-the-tab moment at the start is important. Do it before explaining anything. Let students feel the loss. The lesson lands differently when they've already experienced the problem.

The developer tools inspector is a genuine teaching moment — students are used to treating the browser as a black box. Seeing their data laid out in a table, editable in the browser, changes how they think about what a program is doing. The data is just data. They wrote it. They can read it. They can change it directly.

The JSON section is optional but worth doing if time allows. The pattern of serialize → store → retrieve → deserialize is universal. Students who get it here will recognize it in every backend course they take later.
