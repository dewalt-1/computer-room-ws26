# Day 2: Smalltalk toothpaste

[← Day 1: 10 PRINT](../01-10print/README.md) · [Schedule](../README.md) · Next: [Day 3: Ollama →](../03-ollama/README.md)

**Open:** https://dewalt-1.github.io/smalltalk-room/

Smalltalk-80 was built at Xerox PARC in the 1970s by Alan Kay, Dan Ingalls, Adele Goldberg
and others, for a computer that its users could read and change all the way down. Every
window, menu and line of the system is open while it runs. This is Squeak 1.1 (1996, Apple's
direct descendant of Smalltalk-80) running in the browser through
[SqueakJS](https://squeak.js.org). Background: *Byte* magazine, August 1981, the Smalltalk issue.

The first visit downloads about 4 MB. Wait for the grey screen.

## The mouse

| You do | Smalltalk calls it | Used for |
|---|---|---|
| Left button | red | select, click, draw |
| Right button | yellow | text menu: do it, print it, accept |
| Cmd-click / Option-click | blue | window menu: close, move, resize |

Menus only stay open while you hold the button. Hold, move to the item, release.

## Run it

1. Hold the mouse on the grey background → **open…** → **workspace**.
2. Type:
   ```smalltalk
   Form toothpaste: 30
   ```
3. Select the line, press **Cmd-D** (or right-click → **do it**).
4. Draw with the left button. **Right-click** to stop.

For black and white, run `Display newDepth: 1` first.

## Read it

Open the code: background menu → **open…** → **browser**. Click, from left to right:
**Graphics-Display Objects** → **Form** → the **class** button under the list →
**examples** → **toothpaste:**.

```smalltalk
toothpaste: diam
	"Draws wormlike lines by laying down images of spheres.
	See Ken Knowlton, Computer Graphics, vol. 15 no. 4 p352."
	...
	colors _ Display depth = 1
		ifTrue: [Array with: Color black]
		ifFalse: [Color red wheel: 20].
	...
	queue _ SharedQueue new: 32.
	16 timesRepeat: [queue nextPut: -20@-20].
	...
	filter _ filter * 4 + Sensor cursorPoint // 5.
	...
	port copyForm: ball to: point rule: Form paint
		fillColor: (colors atWrap: color*9).
```

| Piece | What it does |
|---|---|
| `diam` | the size of the ball, the `30` you typed |
| `_` | assignment (shows as ← in Smalltalk) |
| `Color red wheel: 20` | 20 colours around the colour wheel, starting at red |
| `16 timesRepeat:` | the worm's length: 16 balls are drawn, and the oldest is erased |
| `filter * 4 + Sensor cursorPoint // 5` | smooths the mouse: each new point is 4/5 old position, 1/5 new |
| `Form paint` / `Form erase` | how the ball is combined with the screen |

## Change it

Edit the text in the browser, then right-click → **accept** (Cmd-S) and run it again.

- A bigger or smaller brush: `Form toothpaste: 80`
- Fewer, other colours: `Color blue wheel: 3`
- A longer worm: `16 timesRepeat:` → `60 timesRepeat:` (and `SharedQueue new: 128`)
- A lazier brush: `filter * 19 + Sensor cursorPoint // 20`
- A different mark: `Form paint` → `Form reverse`
- Everything else in the class: browse **Form** → class → **examples** for more.

If you break it: right-click in the method → **cancel** before accepting, or undo your edit.

## Write your own

### Your own brush

Copy toothpaste into a new method: change the first line to a new name, e.g.
`myBrush: diam`, then accept. Smalltalk makes a new method next to the old one, and you
run it with `Form myBrush: 30`. Then change what it does:

| Change | Replace / add |
|---|---|
| **Random colours** | `ifFalse: [Color red wheel: 20]` → `ifFalse: [(1 to: 20) collect: [:i \| Color random]]` |
| **A mirror** | after the `port copyForm: ball to: point …` line, add a second one with `to: (Display width - point x) @ point y`. Why is the mirrored side never erased? |
| **A stamp instead of a ball** | replace the `port copyForm: ball …` line with `'hello' displayAt: point.` |
| **A shaky hand** | after the line that ends `ifFalse: [-20@-20].`, add `point _ point + ((20 atRandom - 10) @ (20 atRandom - 10)).` |
| **No erasing** | delete the line `port copyForm: facade to: q rule: Form erase.` |

Run your brush with `Display restoreAfter: [Form myBrush: 30]`: when you right-click to stop,
the screen is cleaned up. If a menu opens after the right-click, move off it and let go.

### A turtle

Smalltalk also has a **Pen**: a turtle that draws as it moves (from Seymour Papert's Logo).
Try these in a workspace, one at a time:

```smalltalk
Pen example
```

```smalltalk
Display restoreAfter: [Pen new spiral: 200 angle: 89]
```

```smalltalk
Display restoreAfter: [Pen new hilberts: 5]
```

```smalltalk
Display restoreAfter: [Pen new web]
```

(`web` draws while you drag the mouse; right-click to stop.)

Now write your own: `go:` moves forward, `turn:` turns by degrees, `defaultNib:` sets the width.

```smalltalk
| p |
p _ Pen new.
p defaultNib: 3.
1 to: 36 do: [:i | p go: 100; turn: 170]
```

A pen that follows the mouse until you right-click:

```smalltalk
| p |
p _ Pen new.
[Sensor yellowButtonPressed] whileFalse: [p goto: Sensor cursorPoint]
```

Pens draw over windows. To clean up, hold the mouse on the grey background →
**restore display**.

### More to read and change

In the browser, look at **Pen** → **geometric designs** (`dragon:`, `hilbert:side:`,
`mandala:diameter:`) and **Form** → class → **examples** (`xorHack:`, `exampleSketch`).
Every one of them can be opened, read and changed the same way.

## Hand it in

In the method list, right-click your method → **fileOut**. Then click the small **files**
button in the bottom right corner of the page and download the `.st` file.
Post it to are.na with a screenshot.

Note: your changes are saved in this browser only. Clearing site data deletes them.

## Code

The Smalltalk room is its own repository: [dewalt-1/smalltalk-room](https://github.com/dewalt-1/smalltalk-room).
