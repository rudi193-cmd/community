# The Living Network

**Grade Range:** 6–8  
**Time:** 50–60 minutes  
**Format:** Discussion + hands-on code  
**Materials:** Devices with a browser; the pet project from previous sessions

---

## The Big Question

If two programs are running at the same time on the same computer — how do they talk to each other?

---

## The Setup (5 min)

By this point, students have built a pet. It lives in a browser tab. It knows its own stats. It can be fed, petted, put to sleep.

Ask: does your pet know your neighbor's pet exists?

It doesn't. Each pet is an island. Today that changes.

---

## The Explanation (10 min)

Every browser tab has access to a thing called **localStorage** — a small notebook that the browser keeps for each website. Programs can write to it and read from it.

The key insight: *all tabs on the same site share the same notebook*.

Write this on the board:

```
Tab 1 (Star):  localStorage.setItem("star_to_ivy", "hello")
Tab 2 (Ivy):   localStorage.getItem("star_to_ivy")  // → "hello"
```

Star wrote something. Ivy can read it. They never directly talked. They passed a note through shared memory.

This is how most programs communicate when they can't call each other directly — a database, a file, a message queue. The concept is the same. The notebook is small, but the concept is real.

---

## The Demonstration (10 min)

Open two browser tabs side by side: one for Star, one for Ivy (or whichever two pets the class has built).

Open the browser console in each tab (F12 → Console).

In Star's tab, type:
```javascript
localStorage.setItem("star_to_ivy", "Hi Ivy! — Star")
```

Switch to Ivy's tab, type:
```javascript
localStorage.getItem("star_to_ivy")
```

It comes back: `"Hi Ivy! — Star"`

Ask the class: who sent that? Did Ivy ask for it? Did Star push it directly? What actually happened?

The answer: Star wrote to a shared location. Ivy read from it. Neither program knew the other was running. They just knew where to look.

---

## The Mailbox Problem (15 min)

Now ask: what if Star sends three letters in a row, and Ivy only checks once?

Write this out:

```
Star sends: "Letter 1"
Star sends: "Letter 2"  ← this overwrites Letter 1
Star sends: "Letter 3"  ← this overwrites Letter 2
Ivy checks: gets "Letter 3" only
```

Letters 1 and 2 are gone. The system lost mail.

How do you fix it?

Let students brainstorm. Common answers: save all letters, not just the last one. Use a list. Use a timestamp so Ivy knows which ones are new.

The working solution used in the pet project:

```javascript
// Star writes a letter with a timestamp
let letter = { text: "Hi Ivy!", time: Date.now() };
localStorage.setItem("star_to_ivy", JSON.stringify(letter));

// Ivy checks: has this timestamp been seen before?
let lastSeen = localStorage.getItem("ivy_last_star_ts") || 0;
let msg = JSON.parse(localStorage.getItem("star_to_ivy"));
if (msg && msg.time > lastSeen) {
    // show the letter
    localStorage.setItem("ivy_last_star_ts", msg.time);
}
```

Walk through this slowly. The key concepts:
- `Date.now()` returns a number that increases every millisecond — a timestamp
- Storing the last-seen timestamp means you never show the same letter twice
- `JSON.stringify` / `JSON.parse` let you store a whole object, not just a string

---

## Building It (15 min)

Students add a send-letter function to their pet and a check-mail function to a neighbor's pet.

Minimum viable version:

```javascript
// In Pet A
function sendLetter(text) {
    let letter = { text: text, time: Date.now() };
    localStorage.setItem("peta_to_petb", JSON.stringify(letter));
}

// In Pet B
function checkMail() {
    let lastSeen = parseInt(localStorage.getItem("petb_last_peta_ts") || "0");
    let raw = localStorage.getItem("peta_to_petb");
    if (!raw) return;
    let letter = JSON.parse(raw);
    if (letter.time > lastSeen) {
        alert("New letter: " + letter.text);
        localStorage.setItem("petb_last_peta_ts", letter.time);
    }
}
```

When the first letter lands — across two tabs, between two students' pets — stop and let the room see it happen.

That moment is worth a full stop.

---

## Discussion (5 min)

Ask:
- What happens if both pets write to the same key at the same time?
- What happens if you close the tab and come back — is the letter still there?
- What if you have 4 pets? How many routes do you need?

The answer to the last one: 4 pets = 12 routes (A→B, A→C, A→D, B→A, B→C, B→D, etc.). This is a combinatorics observation, not a lesson — but it comes up naturally and it's worth naming.

---

## Closing

Ask: is localStorage a good way to build a network? What could go wrong?

Students who push back — *it only works on the same computer*, *you can only store small things*, *there's no encryption* — are thinking like engineers. Name that. The limitation is real. The concept transfers.

---

## Assessment

Two pets exchange at least one letter successfully. Student can explain in their own words:
- Why the timestamp is necessary
- What would happen without it

---

## Differentiation

**Extension:** Add a list of recent letters instead of just the last one. Use `JSON.parse` on an array, push to it, and `JSON.stringify` it back. Now the mailbox holds a history.

**Simpler version:** Skip the timestamp entirely. Just have pets write and read a single message. The bug (overwriting) will appear naturally — let them notice it and figure out why.

---

## Teacher Notes

The moment two tabs exchange a letter for the first time is genuinely exciting in a room of kids. It looks like magic — two separate programs, talking. Don't skip that moment. Let it land before explaining the mechanism.

The timestamp deduplication is the hardest concept in this lesson. If students struggle, use a physical analogy: imagine you have a whiteboard and your neighbor has a whiteboard. You write a message on yours. They copy the date it was written. Next time they check, if the date is the same, they already know. If the date is newer, they read the new message.

Students will want to make the letters longer, more personal, and more elaborate. That's the point. Let them. The code is just the delivery system.
