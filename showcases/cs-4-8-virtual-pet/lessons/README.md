# Creature Lab — CS Curriculum (Redesigned)

A complete, hands-on computer science curriculum for grades 4–8, reimagined as **Creature Lab** — a playful, cohesive learning experience where students build a virtual pet from design sketch to deployed code.

## What's Here

**8 interactive lessons** in HTML — no build step, no dependencies. All lessons follow the same design system.

| Lesson | Grade | Time | What You Build |
|--------|-------|------|----------------|
| Design Your Creature | 4–6 | 30 min | A paper design (name, colors, personality) |
| Your First Pet | 4–6 | 45 min | Canvas drawing + animation loop |
| Buttons & Interaction | 5–7 | 45 min | Action buttons that change pet state |
| Stats and Decay | 5–7 | 45 min | Hunger/energy/happiness draining over time |
| Save Your Pet | 5–7 | 45 min | localStorage persistence across sessions |
| The Living Network | 6–8 | 50 min | Two pets exchange letters via shared memory |
| The Hub | 6–8 | 50 min | CSS Grid + iframes: a 2×2 pet dashboard |
| Flash the Watch | 6–8 | 60 min | C++ microcontroller — your pet on hardware |

## Design System

**Creature Lab** is built on a cohesive visual + interaction language:

- **Palette**: Grape (`#7c4dff`) + Tangerine (`#ff7a4d`), with semantic colors for stats (mint food, sun happy, sky energy)
- **Type**: Bricolage Grotesque (display) + Lexend (body) + JetBrains Mono (code)
- **Paper background**: Warm cream graph-paper texture (`#f6efe1`)
- **Components**: Sticky notes, cozy editor windows, juicy button press feedback, living mascot creature
- **Motion**: Tasteful, reduced-motion respected

## How to Use

1. **Start at `lessons/index.html`** — a card directory of all 8 lessons
2. **Click any lesson** to open it full-screen
3. **Each lesson has two tabs**: Part 1 (concept / teaching), Part 2 (interactive sandbox)
4. Students unlock Part 2 after reading Part 1 — prevents skipping

## Structure of Each Lesson

```
Header (masthead)
  ├─ Grade + time tag
  ├─ Big question
  ├─ Living mascot creature (clickable)
  └─ Sticker label

Navigation (tabs)
  Part 1 — The Concept
  Part 2 — Build It (locked until unlocked)

Part 1 Content
  ├─ Narrative explanation
  ├─ Code windows (styled as "editor" with syntax highlight)
  ├─ Sticky-note callouts
  ├─ "Think" prompts (dashed cards)
  └─ Ready button (unlocks Part 2)

Part 2 Sandbox
  ├─ Live canvas creature
  ├─ Interactive controls / builder panel
  ├─ Stat bars or other visuals
  └─ "Peek at the code" reveal button

Footer
  Branding + CC BY 4.0 license
```

## Technical Notes

- **All HTML.** No build step, no framework. Drop into any static server.
- **Canvas drawings shared.** All 8 lessons use the same `paintCreature()` function so the mascot is consistent.
- **Original sandboxes intact.** We swapped the CSS and added the masthead — all original JS logic unchanged.
- **Responsive.** Works on phones (stacks vertically) and desktop (side-by-side layouts).
- **Accessible.** Reduced-motion respected. All interactive elements keyboard-navigable.

## Deployment

Copy the `lessons/` folder to any static web server:

```bash
python3 -m http.server          # local dev
# or
cp -r lessons/ /var/www/html/  # production server
```

Visit `http://localhost:8000/lessons/` and start with the index.

## Credits

**Curriculum** designed by Sean Campbell. **Redesign** (Creature Lab system) applied 2026.

**License:** CC BY 4.0 — use, remix, teach with it freely.

**GitHub:** https://github.com/Emerging-Rule/community

---

**The Long Game:** Gerald Prime, the Acting Dean of UTETY, carries forward a thread that connects to a larger fictional universe. If students encounter SAFE or other curriculum from the same author later, Gerald's oracle napkin will mean something. Leave the door open.
