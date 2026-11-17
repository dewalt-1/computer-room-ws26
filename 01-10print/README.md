# Day 1: 10 PRINT

[← Schedule](../README.md) · Next: [Day 2: Smalltalk →](../02-smalltalk/README.md)

**Open:** https://dewalt-1.github.io/10print-room/

```basic
10 PRINT CHR$(205.5+RND(1));:GOTO 10
```

One line of BASIC that draws an endless maze. It was printed in the Commodore 64 manual
(1982), and in 2012 ten authors wrote a whole book about it:
**[10 PRINT CHR$(205.5+RND(1)); : GOTO 10](https://10print.org)** (Montfort et al., MIT Press,
free PDF). Read the introduction and the chapter on randomness.

The page is a small Commodore-style BASIC written for this course: 40×25 characters,
graphic characters, a full-screen editor. Nothing to install.

## Run it

Type the line, press Return, type `RUN`, press Return. **Esc** stops it.
Or just click: [run the maze](https://dewalt-1.github.io/10print-room/#r=MTAgUFJJTlQgQ0hSJCgyMDUuNStSTkQoMSkpOzpHT1RPIDEwCg).

## Read it

| Piece | What it does |
|---|---|
| `10` | line number: this line is stored, not run straight away |
| `RND(1)` | a random number between 0 and 1 |
| `205.5+RND(1)` | a number between 205.5 and 206.5 |
| `CHR$(…)` | the character with that code; the fraction is dropped, so it is 205 `╲` or 206 `╱`, half and half |
| `;` | don't start a new line after printing |
| `:GOTO 10` | go back to line 10, forever |

## Change it

Change one thing at a time. Type `LIST` to see your program; move the cursor onto a line
with the arrow keys, edit it and press Return to store it again.

| Try | Line | |
|---|---|---|
| Lean the maze | `10 PRINT CHR$(205.2+RND(1));:GOTO 10` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUFJJTlQgQ0hSJCgyMDUuMitSTkQoMSkpOzpHT1RPIDEwCg) |
| Add empty space | `10 PRINT MID$(" ╲╱",RND(1)*3+1,1);:GOTO 10` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUFJJTlQgTUlEJCgiIHsyMDV9ezIwNn0iLFJORCgxKSozKzEsMSk7OkdPVE8gMTAK) |
| Other characters | `10 PRINT CHR$(209+INT(RND(1)*2)*6);:GOTO 10` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUFJJTlQgQ0hSJCgyMDkrSU5UKFJORCgxKSoyKSo2KTs6R09UTyAxMAo) |
| Slow it down | `20 FOR I=1 TO 30:NEXT` + `30 GOTO 10` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUFJJTlQgQ0hSJCgyMDUuNStSTkQoMSkpOwoyMCBGT1IgST0xIFRPIDMwOk5FWFQKMzAgR09UTyAxMAo) |
| Write straight into screen memory | `10 POKE 32768+RND(1)*1000,77.5+RND(1):GOTO 10` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUE9LRSAzMjc2OCtSTkQoMSkqMTAwMCw3Ny41K1JORCgxKTpHT1RPIDEwCg) |
| No randomness at all | see below | [run](https://dewalt-1.github.io/10print-room/#r=MTAgRk9SIEk9MCBUTyA5OTkKMjAgUE9LRSAzMjc2OCtJLDc3KygoSStJTlQoSS80MCkpIEFORCAxKQozMCBORVhUCjQwIEdPVE8gNDAK) |

```basic
10 FOR I=0 TO 999
20 POKE 32768+I,77+((I+INT(I/40)) AND 1)
30 NEXT
40 GOTO 40
```

**Shift + letter** types the graphic characters (Shift+M `╲`, Shift+N `╱`, Shift+Q `●`).
The **help** button lists every command.

## Write your own

Make a program of no more than five lines that draws something that is not a maze.
Pick one thing about 10 PRINT and change it on purpose:

| Change | Example | |
|---|---|---|
| **The odds change over time:** starts as all `╲`, slowly turns into `╱` | `10 P=0`<br>`20 PRINT CHR$(205-(RND(1)<P));`<br>`30 P=P+.0005:GOTO 20` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUD0wCjIwIFBSSU5UIENIUiQoMjA1LShSTkQoMSk8UCkpOwozMCBQPVArLjAwMDU6R09UTyAyMAo) |
| **Math instead of chance:** a sine wave | `10 X=X+.3:PRINT TAB(19+19*SIN(X));"*":GOTO 10` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgWD1YKy4zOlBSSU5UIFRBQigxOSsxOSpTSU4oWCkpOyIqIjpHT1RPIDEwCg) |
| **Where it draws:** a ball that wanders across the screen | `10 P=33268`<br>`20 POKE P,81`<br>`30 P=P+INT(RND(1)*3)-1+40*(INT(RND(1)*3)-1)`<br>`40 IF P<32768 OR P>33767 THEN P=33268`<br>`50 GOTO 20` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUD0zMzI2OAoyMCBQT0tFIFAsODEKMzAgUD1QK0lOVChSTkQoMSkqMyktMSs0MCooSU5UKFJORCgxKSozKS0xKQo0MCBJRiBQPDMyNzY4IE9SIFA-MzM3NjcgVEhFTiBQPTMzMjY4CjUwIEdPVE8gMjAK) |
| **When it draws:** the maze only grows while you press keys | `10 GET K$:IF K$="" THEN 10`<br>`20 PRINT CHR$(205.5+RND(1));`<br>`30 GOTO 10` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgR0VUIEskOklGIEskPSIiIFRIRU4gMTAKMjAgUFJJTlQgQ0hSJCgyMDUuNStSTkQoMSkpOwozMCBHT1RPIDEwCg) |
| **How long:** it stops by itself | `10 FOR I=1 TO 400`<br>`20 PRINT CHR$(205.5+RND(1));`<br>`30 NEXT`<br>`40 PRINT "THAT IS ENOUGH."` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgRk9SIEk9MSBUTyA0MDAKMjAgUFJJTlQgQ0hSJCgyMDUuNStSTkQoMSkpOwozMCBORVhUCjQwIFBSSU5UICJUSEFUIElTIEVOT1VHSC4iCg) |
| **How it looks:** every second character reversed | `10 PRINT CHR$(18);CHR$(205.5+RND(1));CHR$(146);CHR$(205.5+RND(1));:GOTO 10` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUFJJTlQgQ0hSJCgxOCk7Q0hSJCgyMDUuNStSTkQoMSkpO0NIUiQoMTQ2KTtDSFIkKDIwNS41K1JORCgxKSk7OkdPVE8gMTAK) |
| **Time:** a clock in the corner | `10 PRINT CHR$(147)`<br>`20 PRINT CHR$(19);TI$`<br>`30 GOTO 20` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUFJJTlQgQ0hSJCgxNDcpCjIwIFBSSU5UIENIUiQoMTkpO1RJJAozMCBHT1RPIDIwCg) |

More to combine:

- `INPUT "YOUR NAME";N$` asks a question and waits for an answer.
- `IF … THEN` makes a decision, `GOSUB 100` … `RETURN` reuses a piece of code.
- `PRINT CHR$(147)` clears the screen, `CHR$(19)` jumps to the top left corner.
- `POKE 32768+X+40*Y,C` puts character code `C` at column `X`, row `Y` (codes 64–127 are the graphics).
- `PEEK(32768+X+40*Y)` reads what is already on the screen: draw something that reacts to itself.
- `POKE 59468,14` switches to lower case, `POKE 59468,12` back.
- `?speed=4` at the end of the web address makes everything slower; `?phosphor=green` or `amber` changes the screen colour.

## Hand it in

Click **link**: it copies a web address that contains your whole program. Post it to are.na
with one sentence on what you changed and why. **save** downloads it as a `.bas` file.

## Code

The BASIC room is its own repository: [dewalt-1/10print-room](https://github.com/dewalt-1/10print-room).
