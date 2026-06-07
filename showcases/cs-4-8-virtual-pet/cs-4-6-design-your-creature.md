# Design Your Creature

**Grade Range:** 4–6  
**Time:** 45–60 minutes  
**Format:** Drawing + discussion + optional code introduction  
**Materials:** Paper, pencil, crayons or markers

---

## The Big Question

If you had a pet that lived inside a computer, what would it need?

Not what it looks like yet. What does it *need*?

---

## The Setup (5 min)

Tell students: today they're going to design a creature that will live in a computer. It's not a game character. It's not a toy. It's their pet, and it needs them.

Before anyone touches a keyboard, everyone draws.

Hand out paper. Give them five minutes to draw their creature. No rules about what it looks like. It can be an axolotl, an owl, a cat, a headless rotisserie chicken. Whatever they choose.

When the five minutes are up: ask for names. Go around the room. Just names.

---

## The Design Questions (15 min)

Put these questions on the board. Students answer them on their paper, next to their drawing.

**1. What does your creature need to stay healthy?**  
Prompt: think about real animals. They need food, sleep, attention. What does your creature need? Pick 2–4 things.

**2. What does happy look like for your creature?**  
Prompt: how would you know if it was happy? What would it do? What would change?

**3. What does sad look like?**  
Prompt: what happens if you forget about it for a while?

**4. What is one special thing your creature does that no other creature does?**  
This is the most important question. Take your time.

Give students 10–12 minutes to answer these. Walk around. Ask follow-up questions. If a kid says "it gets hungry," ask: how hungry? Does it get hungry slowly or fast? What happens when it's really hungry?

---

## The Share (10 min)

Pick 3–4 students to share their creature and one answer — any answer they want to share.

After each share, ask the class: does that feel right for that creature? A slow, sleepy animal should probably get tired faster than it gets hungry. A bouncy, active animal might be the opposite.

This is the first design conversation. You're teaching them that design decisions have logic.

---

## The Bridge (10 min)

Ask: where does this information live in a computer?

A real program has to *remember* things. It has to know that your creature is currently at 70% happy and 40% hungry. It can't just guess.

Programmers call these **variables** — containers that hold values. Your creature probably needs a variable for each stat.

Write on the board:

```
food = 80
happy = 70
energy = 50
sleep = 90
```

Ask: if food goes from 80 to 0, what should happen? Who should know? What should change?

This is the second design conversation. You're teaching them that a program is a system — things connect.

---

## If You Have Time: First Code (optional, 10 min)

If students have devices and you have a code template ready:

Show them what a variable looks like in JavaScript:

```javascript
let food = 80;
let happy = 70;
let energy = 50;
```

Ask them to type their own creature's starting values. Their first code is just numbers — but they're *their* numbers, chosen because they know their creature.

If a kid says "my creature starts at 100% everything because it just woke up from a long nap" — that's a design decision. It's also correct syntax. Both things are true.

---

## Closing Question

Ask students: if you came back tomorrow and your creature was at 10% food and 20% happy — would that be your fault, or the computer's fault?

Let them argue about it.

There's no right answer. But the argument is a programming concept: whose responsibility is the state?

---

## Assessment

The design sheet is the assessment. Look for:
- At least 2 named stats
- An answer to "what does happy look like"
- One special behavior that's specific to *their* creature (not generic)

A student who wrote "it does stuff" for the special behavior needs another pass. A student who wrote "it hums quietly when it's tired but only after 8pm" is ready to build.

---

## Differentiation

**For students who finish early:** Ask them to draw what the screen would look like. Where are the stats? What does the creature do when it's full? When it's empty?

**For students who are stuck:** Start with the real-animal version. What does a cat need? Now make it stranger. What if your cat also needed to read a book every day?

**For students who want to skip the drawing:** Let them — but hold the design questions. The drawing is optional. The thinking isn't.

---

## Teacher Notes

The special behavior question is where kids stop being consumers and start being designers. Protect time for it. A room of 25 kids will produce 25 different answers and most of them will be genuinely surprising.

The creatures don't need to be realistic. One kid built an "Acting Dean" who is a headless rotisserie chicken and keeps oracle notes on a napkin. He has a variable called ΔΣ=42. He is arguably the most fully realized character in the project.

The design sheet travels with the student through every subsequent session. When bugs appear — and they will — the design sheet is the source of truth. "What did *you* decide this creature does?" is always the right question.
