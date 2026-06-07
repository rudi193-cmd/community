# Your First Pet

**Grade Range:** 4–6  
**Time:** 45–60 minutes  
**Format:** Guided hands-on (teacher demo + student follow-along)  
**Materials:** Devices with a browser and a text editor (VS Code, Notepad, TextEdit)

---

## The Big Question

How do you tell a computer to draw something?

---

## The Setup (5 min)

Students have a design. They know their creature's name, its colors, what it needs, and one special thing it does. Today they turn that paper design into something a computer actually shows on screen.

Ask: what does a computer need before it can draw anything?

Let them guess. Common answers: instructions, code, a program. All true.

The specific answer: a file. A plain text file with a specific structure. When a browser opens that file, it reads the instructions and draws what you described.

That file is HTML. They're going to write one now.

---

## The HTML Skeleton (10 min)

Every web pet starts the same way. Write this on the board:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My Pet</title>
  </head>
  <body>
    <canvas id="tank" width="400" height="300"></canvas>
    <script>
      // your code goes here
    </script>
  </body>
</html>
```

Walk through it line by line:

- `<!DOCTYPE html>` — tells the browser what kind of file this is
- `<html>` / `</html>` — the whole page lives between these
- `<head>` — information *about* the page (title, settings)
- `<body>` — what actually shows on screen
- `<canvas>` — a blank drawing surface. Width and height are in pixels.
- `<script>` — where JavaScript code lives

The canvas is the pet's home. It's where everything happens.

---

## Drawing the First Shape (15 min)

Inside the `<script>` tags, type this:

```javascript
const canvas = document.getElementById('tank');
const ctx = canvas.getContext('2d');

ctx.fillStyle = 'blue';
ctx.beginPath();
ctx.arc(200, 150, 50, 0, Math.PI * 2);
ctx.fill();
```

Save the file. Open it in a browser. Ask: what do they see?

A blue circle. That's their creature's body — one shape.

Now walk through what each line means:

- `document.getElementById('tank')` — finds the canvas element by its id
- `getContext('2d')` — opens the drawing tools for 2D graphics
- `fillStyle` — sets the color
- `beginPath()` — starts a new shape
- `arc(x, y, radius, start, end)` — draws a circle. `x` and `y` are the center.
- `fill()` — fills the shape with the current color

Ask: how would you move the circle? Change the x and y numbers. What happens if you change 200 to 100? Students try it.

---

## Adding Eyes (10 min)

The creature needs to look like something. Add eyes:

```javascript
// white of eye
ctx.fillStyle = 'white';
ctx.beginPath();
ctx.arc(180, 135, 10, 0, Math.PI * 2);
ctx.fill();

ctx.beginPath();
ctx.arc(220, 135, 10, 0, Math.PI * 2);
ctx.fill();

// pupil
ctx.fillStyle = 'black';
ctx.beginPath();
ctx.arc(182, 137, 5, 0, Math.PI * 2);
ctx.fill();

ctx.beginPath();
ctx.arc(222, 137, 5, 0, Math.PI * 2);
ctx.fill();
```

Save and refresh. A face appears.

Ask: why did we draw the eyes *after* the body? What happens if you draw them before?

The answer: the canvas draws in order. Later shapes go on top. This is called the *draw order*.

---

## Making It Move (10 min)

A creature that doesn't move isn't much of a pet. Replace the drawing code with this structure:

```javascript
let x = 200;
let y = 150;
let dx = 1; // speed: moving right

function draw() {
  ctx.clearRect(0, 0, canvas.width, canvas.height); // erase everything

  ctx.fillStyle = 'blue';
  ctx.beginPath();
  ctx.arc(x, y, 50, 0, Math.PI * 2);
  ctx.fill();

  x = x + dx; // move right every frame
  if (x > canvas.width - 50) dx = -1; // hit right wall, go left
  if (x < 50) dx = 1;                 // hit left wall, go right

  requestAnimationFrame(draw); // call this function again next frame
}

draw();
```

Save and refresh. The creature moves.

Ask: what is `requestAnimationFrame`? It asks the browser to call the `draw` function again as soon as it's ready to draw the next frame — about 60 times per second. That's animation.

---

## Design Your Own (remaining time)

Students use their paper design from the previous session. Goals:

1. Change the body color to their creature's color
2. Add one more feature — a tail, a fin, a hat, anything drawn with a shape
3. Adjust the starting position so the creature begins where they want it

This is free exploration. The only rule: they have to be able to explain one line of code they changed and why.

---

## Closing Question

Ask: where is the creature right now?

Students who think physically: on the screen. Students who think like programmers: in a variable called `x` and `y`. Both answers are correct. But only one of them explains why moving those numbers moves the creature.

---

## Assessment

Student has a working HTML file that:
- Opens in a browser without errors
- Shows at least one shape that represents their creature
- Has the creature moving or responding in some way

A creature that consists of one circle and moves left and right is a complete first pet.

---

## Differentiation

**For students who finish early:** Add a mouth. A straight line (`ctx.moveTo` / `ctx.lineTo`) when the creature is in one part of the screen, a curve (`ctx.arc` drawn partially) when it's elsewhere.

**For students who are stuck:** Keep them on one shape. A colored circle that stays still is enough. The goal is: file opens, shape appears, no errors.

**For students who want more:** Show them how to draw a rectangle with `ctx.fillRect(x, y, width, height)`. Ask: what would a rectangle make? Let them decide.

---

## Teacher Notes

The draw order moment — why eyes go on top of the body — is often where the concept of *layered rendering* clicks. It's the same reason Photoshop has layers. Don't rush past it.

`requestAnimationFrame` doesn't need a full explanation here. "It calls your draw function 60 times per second" is enough. The deeper concept (the browser's render loop, frame timing) comes later. For now: they see it working, and they understand the shape of the idea.

Students will almost immediately ask how to make the creature respond to clicks. That's the next lesson. Name that it's coming, and let the curiosity build.
