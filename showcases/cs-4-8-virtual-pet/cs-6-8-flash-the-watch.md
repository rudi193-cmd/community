# Flash the Watch

**Grade Range:** 6–8  
**Time:** 60–90 minutes  
**Format:** Guided hands-on (teacher demo + student follow-along)  
**Materials:** Waveshare ESP32-S3-Touch-AMOLED-2.06 watch, USB-C data cable, computer with Arduino IDE, step-by-step guide (printed or on-screen)

---

## The Big Question

Your pet lives in a browser. What would it take to move it into a device you can wear?

---

## Background for the Teacher

This lesson follows a working web-based pet and assumes students have added at least stats and one interactive button. It is the hardware track — optional, and requires purchasing the watch (~$45 USD at time of writing).

The lesson works best when students have already built something they're attached to. Moving *their pet* onto a watch is different from programming a generic demo.

The watch used in the original project: **Waveshare ESP32-S3-Touch-AMOLED-2.06** (SKU 31957). It runs on an ESP32-S3 chip, 2.06-inch AMOLED display via QSPI, touch via I2C. Arduino IDE with the `Arduino_GFX_Library` handles the display. The library and board setup are covered in the step-by-step guide that accompanies this lesson.

**Verify pin numbers before class.** The guide includes specific pin defines for the display and touch controller. These must be confirmed against the Waveshare wiki for your specific hardware revision before students use them. If pins are wrong, the display won't initialize — nothing will be damaged, but nothing will work.

---

## The Setup (10 min)

Ask students: what's the difference between a program that runs in a browser and a program that runs in a watch?

Let them guess. Common answers:
- The watch doesn't have a keyboard
- The watch doesn't have internet (sometimes)
- The watch is smaller

All true. But the deeper answer: **a browser is a translation layer**. JavaScript runs inside the browser, and the browser handles talking to the actual hardware — the screen, the touch, the memory. The browser does that translation for you.

When you program a microcontroller directly, *you* are writing the translation layer. You tell the chip exactly which pins the screen is connected to. You initialize the display driver yourself. You write the loop that runs forever, checking for touch and updating the screen.

This is lower-level. It's also closer to the machine.

---

## Two Languages, One Concept (10 min)

Put these side by side on the board:

**JavaScript (browser):**
```javascript
let food = 80;
function feed() {
    food = Math.min(100, food + 20);
    draw();
}
```

**C++ (watch):**
```cpp
int food = 80;
void feed() {
    food = min(100, food + 20);
    draw();
}
```

Ask: what's the same? What's different?

Same: variable names, function names, the logic inside, `min()`.  
Different: `let` vs `int`, the type declaration, the semicolons were already there but now they matter.

The concept transfers. The pet's logic — what makes it hungry, what makes it happy — is the same in both languages. The syntax changes. The thinking doesn't.

---

## The Upload Cycle (30–40 min)

Work through the step-by-step guide together as a class. The guide has 11 steps:

1. Install Arduino IDE
2. Add ESP32 board support
3. Install Arduino_GFX_Library
4. Connect the watch via USB-C
5. Select the correct board and port
6. Copy the first code section (pin defines and setup)
7. Copy the second section (display initialization)
8. Copy the third section (stat logic and touch handling)
9. Copy the fourth section (draw loop)
10. Compile and upload
11. Watch it run

Do steps 1–5 before class if possible, or have a pre-configured machine available. The library installation and board setup take 10–15 minutes and have the most variation between machines.

Steps 6–11 are the part students do themselves. Each section is a copy-paste block. Read it together before pasting. Ask: what do you think this section does? What would happen if we skipped it?

When the upload finishes and the screen lights up with their pet — stop.

Let it land.

---

## Discussion (10 min)

Ask:
- What had to change when we moved from the browser to the watch?
- What stayed exactly the same?
- If you wanted to add a new feature to the watch version — say, a sound when the pet is happy — where would you start?

The third question is the important one. Students who can answer it understand that the watch is now *their* system to extend, not a finished product.

---

## What Can Go Wrong (and What to Do)

**The upload fails — "port not found":** USB-C cable may be charge-only. Try a different cable. Data cables work; charge-only cables don't.

**The upload fails — "board not found":** Board support not installed, or wrong board selected. Walk through step 2 again.

**The screen doesn't initialize:** Pin numbers may not match your hardware revision. Check the Waveshare wiki for your specific SKU and update the pin defines in the code.

**The display is upside down:** Add a rotation call in setup. This is a two-line fix.

**The touch isn't responding:** Touch I2C address may differ. The default is 0x38; some revisions use 0x14.

Keep this list visible during class. When something breaks — and something will — naming the failure mode before diving into it is a skill. "I don't know what's wrong" is the starting point, not the ending point.

---

## Closing

Ask students: is the watch version better than the browser version?

There's no right answer. The browser version works on any device, is easier to update, and doesn't require a cable. The watch version fits in a pocket, runs without a computer, and is something you made with your hands.

Both things can be true.

---

## Assessment

Student can explain:
- What an "upload" does (transfers compiled code to the device)
- Why the pin numbers matter
- One difference and one similarity between JavaScript and C++

Successful upload with pet visible on screen is the primary demonstration.

---

## Differentiation

**Extension:** Modify one behavior on the watch — change a decay rate, change a color, add a new button action. Any change that compiles and uploads successfully counts.

**Simpler version:** Pre-load the code on the watch before class. Use the session to explore how it works rather than building from scratch. Modify one variable (a stat starting value) and re-upload.

---

## Teacher Notes

The upload moment is the payoff for everything that came before. A kid who designed a creature on paper, built it in a browser, and then watches it run on a device they're holding has completed a full arc from concept to physical hardware. That arc is rare in a classroom context.

The guide was built alongside a real kid's project. The star graphic in the guide is Star the Axolotl — the same pet that started the whole project when a kid typed "I wantine to mate a tonagachi." That context is available to share or not, depending on the class.

The code in the guide is paste-and-upload. The lesson is not "write C++ from scratch." The lesson is "understand what the code is doing well enough to modify it." Those are different skills, and the second one is more appropriate here.
