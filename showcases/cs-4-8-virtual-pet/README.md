# Virtual Pets: A Multi-Session CS Project for Kids (Grades 4–8)

**Subject:** Computer Science / Creative Making  
**Grade Range:** 4–8  
**Format:** Multi-session project (5–12+ sessions)  
**Contributed by:** Sean Campbell

---

## How This Started

It started with a kid typing: *"I wantine to mate a tonagachi."*

That's a direct quote. The phonetic spelling is part of the record.

The kid had been watching Star — a pixel axolotl living in a browser tab — eat pixel fish and write letters to her friends. She wanted to carry Star around in her pocket. She wanted to *build* that. She didn't know what a Tamagotchi was by name, but she knew what she wanted.

That request became a fun weekend project, four pets, a shared mail network, a 2×2 hub dashboard, and eventually a hardware watch where the same axolotl runs as C++ on a microcontroller.

This is the record of how that happened — what worked, what surprised us, and what another educator might replicate.

---

## The Pets

Each pet is a single HTML file, no build step, no framework. Canvas drawing for the character, vanilla JS for state, localStorage for persistence and cross-pet communication.

| Pet | Character | Who Designed It |
|-----|-----------|-----------------|
| Star | Axolotl | Kid (age ~8) |
| Ivy | Owl | Kid (age ~8) |
| Astro | Black cat on a rooftop | Kid (age ~8) |
| Gerald Prime | Acting Dean, headless rotisserie chicken | Kid (age ~8) |

Gerald is the one who needs explaining. He is the Acting Dean of UTETY (a fictional university that exists in a separate educational universe). He runs a department called UTETY and keeps oracle notes on a napkin. He has a variable in the code called `ΔΣ=42`. The kids invented all of this. Gerald is the only canonical bridge between the kid-projects universe and the larger fictional world they inhabit — which means there's a long-term payoff for kids who grow up with these pets and later encounter the curriculum that Gerald's universe belongs to.

---

## The Cross-Pet Mail Network

Each pet can send and receive letters from every other pet. Letters are stored in `localStorage` with keys like `ivy_to_star`, `star_to_gerald`, `gerald_to_astro`. The sender writes, the receiver checks on a timer. Timestamps prevent duplicate deliveries.

The network has 12 routes. The kids don't know that. They know that Star got a letter from Ivy and wrote back.

What they *also* learned, without being taught it as a lesson:
- Programs share state through shared memory
- Order matters when two programs write to the same place
- A timestamp is a way of saying "I already know about this one"

---

## The Hub

A 2×2 grid of iframes. Each cell is one pet. Click to expand to fullscreen; the same iframe instance moves — it doesn't reload, so state is preserved. The kids use this on a laptop; on a phone they navigate directly to each pet's URL.

The hub required real debugging: Star lived at `/` instead of `/star/`, which made relative paths from hub fail. The fix was a probe function that tries multiple candidate URLs until it finds the one that loads. We kept it.

---

## The Tamagotchi

When the kid asked to carry Star in a pocket, we built a mobile web prototype first: egg-shaped CSS device, stat bars, buttons, poop mechanic, offline decay calculation. Four stats: food, happy, energy, sleep. Poop every 14 minutes. Stage progression from egg to baby to kid to adult (with a crown).

The prototype confirmed the idea worked before committing to hardware. The kids loved it. That was the test.

Hardware target: Waveshare ESP32-S3-Touch-AMOLED-2.06. An 11-step interactive guide walks the kids through writing C++ and uploading it themselves. The C++ code is embedded in the guide as copy-paste sections.

---

## What Worked

**Paper first.** Every pet started with a drawing. Name, color, one special thing it does, what makes it happy. The design worksheet came before any code. Kids who designed on paper built more intentional pets.

**One thing per session.** Add the food button. Add the sleep button. Add the letter. Add the friend. Each session had one clear win. Multi-hour "let's build the whole thing" sessions produced less than focused 45-minute ones.

**Let them name everything.** Gerald Prime. Acting Dean. UTETY. ΔΣ=42. The oracle napkin. These came entirely from the kid. The more the naming was theirs, the more invested they were in the code that implemented it.

**The parent held the technical frame.** The kids drove creative decisions; the adult held the syntactic frame (what the file needed to be valid, what the function needed to return). This is replicable in a classroom: the teacher doesn't need to design the pet, but does need to know what a valid HTML file looks like.

**Bugs are features.** Gerald's crown appeared at the wrong stage once. The kid decided Gerald wears the crown whenever he wants because he's the dean. It stayed.

---

## What Would Help a Classroom Replication

- A blank pet template (HTML skeleton with canvas, stat system, localStorage hooks)
- A design worksheet (paper-first: name, drawing, stats, one special behavior)
- A teacher guide for the cross-pet mail session (the moment where two pets first exchange a letter is always a good one)
- The hardware track is optional and expensive (~$45/watch) — the web version is complete on its own

The lesson files that accompany this showcase cover the design session (grades 4–6) and the networking session (grades 6–8) as standalone teachable units.

---

## File Locations

All files are single-file HTML, no dependencies, no build step. They run with `python3 -m http.server` or any static server.

```
star/index.html      — Star the Axolotl
ivy/index.html       — Ivy the Owl
astro/index.html     — Astro the Cat
gerald/index.html    — Gerald Prime
hub/index.html       — 2×2 hub dashboard
star/tamagotchi.html — Mobile Tamagotchi prototype
guide/index.html     — 11-step C++ upload guide
```

---

## The Long Game

Gerald knows something. The kids don't know what yet. They built him anyway, because he was funny and weird and the oracle napkin seemed important. When they're older and encounter the curriculum that Gerald's universe belongs to, that napkin will mean something different.

That's the long game. You don't explain it. You just leave the door open.
