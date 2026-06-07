# The Hub

**Grade Range:** 6–8  
**Time:** 50–60 minutes  
**Format:** Discussion + hands-on  
**Materials:** Devices with a browser and text editor; at least two working pet files; the willow-2.0 server (or python3 -m http.server) running

---

## The Big Question

If you have four programs, how do you put them all on one screen?

---

## The Setup (5 min)

Students have pets. The pets live in separate files. To see all of them, you open four tabs and switch between them.

Ask: what if you wanted to see them all at the same time?

This is a dashboard — a single page that brings multiple things together. Dashboards exist everywhere: flight control rooms, trading platforms, home automation apps. They all solve the same problem: too many things, one screen.

Today they build one.

---

## The iframe (10 min)

A web page can embed another web page using an `<iframe>` — an "inline frame." The inner page runs completely independently. It has its own JavaScript, its own localStorage, its own state. The outer page just shows it.

Write this on the board:

```html
<iframe src="star/index.html" width="700" height="420"></iframe>
```

That's it. If the server is running, this shows the Star pet inside the page that contains it.

Ask: does the pet inside the iframe know it's inside an iframe?

It doesn't. The iframe is transparent to the inner page. Star still runs at 60fps, still decays, still saves to localStorage — all of it happens exactly as if it were in its own tab.

This is the key property that makes iframes useful for a hub: the inner programs don't have to be modified at all.

---

## The Grid (15 min)

Four pets in four iframes need a layout. CSS Grid is the right tool.

Build the full hub structure:

```html
<!DOCTYPE html>
<html>
<head>
  <title>All the Pets</title>
  <style>
    body {
      display: flex;
      flex-direction: column;
      height: 100vh;
      margin: 0;
      background: #0a0a1a;
    }

    #grid {
      flex: 1;
      display: grid;
      grid-template-columns: 1fr 1fr;
      grid-template-rows: 1fr 1fr;
      gap: 8px;
      padding: 8px;
    }

    .cell {
      position: relative;
      overflow: hidden;
      border-radius: 10px;
      cursor: pointer;
    }

    .cell iframe {
      position: absolute;
      top: 0;
      left: 0;
      width: 700px;
      height: 420px;
      transform-origin: top left;
      pointer-events: none;
    }
  </style>
</head>
<body>
  <div id="grid">
    <div class="cell" data-src="star/index.html">
      <iframe src="star/index.html"></iframe>
    </div>
    <div class="cell" data-src="ivy/index.html">
      <iframe src="ivy/index.html"></iframe>
    </div>
    <div class="cell" data-src="astro/index.html">
      <iframe src="astro/index.html"></iframe>
    </div>
    <div class="cell" data-src="gerald/index.html">
      <iframe src="gerald/index.html"></iframe>
    </div>
  </div>
</body>
</html>
```

Walk through the CSS:

- `height: 100vh` — the body fills the full viewport height
- `flex: 1` — the grid takes all remaining space after the header
- `grid-template-columns: 1fr 1fr` — two equal columns
- `grid-template-rows: 1fr 1fr` — two equal rows. Four cells total.
- `overflow: hidden` — clips the iframe to the cell boundary

---

## Scaling (10 min)

There's a problem. Each pet canvas is 700×420 pixels. Each grid cell is much smaller — maybe 400×240. The iframe is too big for the cell.

The fix is CSS `transform: scale()`. Instead of resizing the iframe (which would break the inner layout), we scale it down visually.

Add this JavaScript:

```javascript
function scaleFrames() {
  document.querySelectorAll('.cell').forEach(cell => {
    const iframe = cell.querySelector('iframe');
    const sx = cell.clientWidth  / 700;
    const sy = cell.clientHeight / 420;
    iframe.style.transform = `scale(${sx}, ${sy})`;
  });
}

window.addEventListener('load',   scaleFrames);
window.addEventListener('resize', scaleFrames);
setTimeout(scaleFrames, 100); // catch layout settling
```

Ask: what does `cell.clientWidth / 700` calculate?

The ratio. If the cell is 350px wide and the iframe is 700px wide, the ratio is 0.5 — scale it to half size. The iframe content stays at its designed resolution; we just shrink it visually.

This is the same technique used for responsive video embeds, PDF viewers, and mobile app previews.

---

## Click to Expand (remaining time)

The grid view is small. Sometimes you want to see one pet full-size. Add an overlay:

```html
<!-- add inside body, after #grid -->
<div id="overlay" style="display:none; position:fixed; inset:0; background:rgba(0,0,0,0.88); z-index:1000;">
  <button id="close-btn" onclick="collapse()"
    style="position:fixed; top:14px; right:14px; z-index:1002; font-size:1.2rem; padding:8px 16px; cursor:pointer;">
    ✕ Close
  </button>
  <div id="overlay-inner" style="position:absolute; top:50%; left:50%; transform:translate(-50%,-50%); width:700px; height:420px; overflow:hidden; border-radius:12px;">
    <iframe id="overlay-frame" src="" style="width:700px; height:420px; border:none;"></iframe>
  </div>
</div>
```

```javascript
document.querySelectorAll('.cell').forEach(cell => {
  cell.addEventListener('click', () => {
    const src = cell.dataset.src;
    document.getElementById('overlay-frame').src = src;
    document.getElementById('overlay').style.display = 'block';
  });
});

function collapse() {
  document.getElementById('overlay').style.display = 'none';
  document.getElementById('overlay-frame').src = '';
}

document.getElementById('overlay').addEventListener('click', function(e) {
  if (e.target === this) collapse();
});
```

Click a cell: the pet expands to full size in an overlay. Click the backdrop or the close button: it collapses back to the grid.

Note: the close button must be positioned *outside* the iframe's boundaries using `position:fixed`, or the iframe will intercept the click.

---

## Discussion (5 min)

Ask:
- The pets in the grid have `pointer-events: none`. What does that mean? Why?
- The overlay loads a fresh iframe. Does that pet remember its stats?
- If you have the grid open and the overlay open at the same time, are two copies of the same pet running?

The answers:
- `pointer-events: none` lets clicks pass through to the cell div, so the expand works
- Yes — it loads from localStorage, so stats survive
- Yes, both run independently. This is why timestamp deduplication in the mail system matters

The third question is the hardest and the most interesting. Two instances of the same program running simultaneously, sharing the same localStorage. Each sees what the other writes. This is exactly the same problem distributed systems deal with at scale.

---

## Closing Question

Ask: a dashboard is a view — it doesn't change the programs it shows. But it changes how you use them. What else is a "view" in computing?

Students who are ready: a browser is a view over the internet. A file manager is a view over your disk. A spreadsheet is a view over a table of data. The data exists; the view is how you interact with it.

---

## Assessment

Student has a working hub that:
- Shows at least two pets in a grid layout
- Scales iframes to fit their cells
- Expands to full view on click
- Has a working close button

---

## Differentiation

**For students who finish early:** Style each cell with a different border color based on which pet it contains. Add a hover effect using CSS `:hover`. Add a small label overlay with the pet's name.

**For students who are stuck:** Get one iframe in one cell working and scaled. That's the complete pattern. Two cells is just doing it twice.

**For students who want more:** The overlay scales for desktop but may be too large on a phone. Add `max-width: 96vw; max-height: 92vh` to `#overlay-inner` and a `Math.min(sx, sy)` scaling approach that keeps the aspect ratio.

---

## Teacher Notes

The close button placement — `position:fixed` outside the iframe — is a real gotcha. An iframe is a separate document. Events inside it don't bubble to the parent. If you put the close button inside the overlay div but inside the iframe's area, it will never fire. Position it independently in the parent document.

This is a concrete lesson in document isolation: iframes are intentionally sandboxed. That's a security feature, not a bug. The hub works *because* the pets are isolated from each other — and it requires careful placement of controls *because* of that same isolation.

Students often ask: can the hub send messages to the pets? The answer is yes — `postMessage` exists for exactly that. But it's not needed here. The hub is a view. It doesn't need to command anything. Knowing when *not* to add complexity is also a design skill.
