# Day 1: 10 PRINT

[← Schedule](../README.md) · Next: [Day 2: Smalltalk →](../02-smalltalk/README.md)

**Open:** https://dewalt-1.github.io/10print-room/

```basic
10 PRINT CHR$(205.5+RND(1));:GOTO 10
```

This is a one-line BASIC program for an endless maze. A version of it was printed in the
Commodore 64 manual (1982). In 2012 ten authors wrote a whole book about it:
**[10 PRINT CHR$(205.5+RND(1)); : GOTO 10](https://10print.org)** (Montfort et al., MIT Press,
free PDF). Read the introduction and the chapter on randomness.

The page is a small Commodore-style BASIC made for this course. The screen has 40×25 characters.
You don't need to install anything.

## Run it

Type the line, press Return, type `RUN`, press Return. Press **Esc** to stop.
Or just click: [run the maze](https://dewalt-1.github.io/10print-room/#r=MTAgUFJJTlQgQ0hSJCgyMDUuNStSTkQoMSkpOzpHT1RPIDEwCg).

## Read it

| Part | Meaning |
|---|---|
| `10` | the line number. A line with a number is stored, and run later with `RUN` |
| `RND(1)` | a random number between 0 and 1 |
| `205.5+RND(1)` | a number between 205.5 and 206.5 |
| `CHR$(…)` | the character with that code. Only the whole number is used: 205 is `╲` and 206 is `╱`, each half of the time |
| `;` | don't start a new line after printing |
| `:GOTO 10` | go back to line 10, forever |

## Change it

Change one thing at a time. Type `LIST` to see your program; move the cursor onto a line
with the arrow keys, edit it and press Return to store it again.

| Try | Line | |
|---|---|---|
| More `╲` than `╱` | `10 PRINT CHR$(205.2+RND(1));:GOTO 10` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUFJJTlQgQ0hSJCgyMDUuMitSTkQoMSkpOzpHT1RPIDEwCg) |
| Add empty space | `10 PRINT MID$(" ╲╱",RND(1)*3+1,1);:GOTO 10` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUFJJTlQgTUlEJCgiIHsyMDV9ezIwNn0iLFJORCgxKSozKzEsMSk7OkdPVE8gMTAK) |
| Other characters | `10 PRINT CHR$(209+INT(RND(1)*2)*6);:GOTO 10` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUFJJTlQgQ0hSJCgyMDkrSU5UKFJORCgxKSoyKSo2KTs6R09UTyAxMAo) |
| Slow it down | `10 PRINT CHR$(205.5+RND(1));`<br>`20 FOR I=1 TO 30:NEXT`<br>`30 GOTO 10` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUFJJTlQgQ0hSJCgyMDUuNStSTkQoMSkpOwoyMCBGT1IgST0xIFRPIDMwOk5FWFQKMzAgR09UTyAxMAo) |
| Random places on the screen | `10 POKE 32768+RND(1)*1000,77.5+RND(1):GOTO 10` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUE9LRSAzMjc2OCtSTkQoMSkqMTAwMCw3Ny41K1JORCgxKTpHT1RPIDEwCg) |
| No randomness | see below | [run](https://dewalt-1.github.io/10print-room/#r=MTAgRk9SIEk9MCBUTyA5OTkKMjAgUE9LRSAzMjc2OCtJLDc3KygoSStJTlQoSS80MCkpIEFORCAxKQozMCBORVhUCjQwIEdPVE8gNDAK) |

```basic
10 FOR I=0 TO 999
20 POKE 32768+I,77+((I+INT(I/40)) AND 1)
30 NEXT
40 GOTO 40
```

Use **Shift + letter** for graphic characters (Shift+M `╲`, Shift+N `╱`, Shift+Q `●`).
Click **help** for a list of all commands.

## Write your own

Make a program of five lines or fewer. The result should not be a maze.
Choose one part of 10 PRINT and change it:

| Change | Example | |
|---|---|---|
| **Chance:** only `╲` at first, then more and more `╱` | `10 P=0`<br>`20 PRINT CHR$(205-(RND(1)<P));`<br>`30 P=P+.0005:GOTO 20` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUD0wCjIwIFBSSU5UIENIUiQoMjA1LShSTkQoMSk8UCkpOwozMCBQPVArLjAwMDU6R09UTyAyMAo) |
| **Math:** a sine wave | `10 X=X+.3:PRINT TAB(19+19*SIN(X));"*":GOTO 10` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgWD1YKy4zOlBSSU5UIFRBQigxOSsxOSpTSU4oWCkpOyIqIjpHT1RPIDEwCg) |
| **Place:** a ball in a new random place next to the last one | `10 P=33268`<br>`20 POKE P,81`<br>`30 P=P+INT(RND(1)*3)-1+40*(INT(RND(1)*3)-1)`<br>`40 IF P<32768 OR P>33767 THEN P=33268`<br>`50 GOTO 20` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUD0zMzI2OAoyMCBQT0tFIFAsODEKMzAgUD1QK0lOVChSTkQoMSkqMyktMSs0MCooSU5UKFJORCgxKSozKS0xKQo0MCBJRiBQPDMyNzY4IE9SIFA-MzM3NjcgVEhFTiBQPTMzMjY4CjUwIEdPVE8gMjAK) |
| **Keyboard:** press a key to add one character | `10 GET K$:IF K$="" THEN 10`<br>`20 PRINT CHR$(205.5+RND(1));`<br>`30 GOTO 10` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgR0VUIEskOklGIEskPSIiIFRIRU4gMTAKMjAgUFJJTlQgQ0hSJCgyMDUuNStSTkQoMSkpOwozMCBHT1RPIDEwCg) |
| **Length:** only 400 characters | `10 FOR I=1 TO 400`<br>`20 PRINT CHR$(205.5+RND(1));`<br>`30 NEXT`<br>`40 PRINT "THAT IS ENOUGH."` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgRk9SIEk9MSBUTyA0MDAKMjAgUFJJTlQgQ0hSJCgyMDUuNStSTkQoMSkpOwozMCBORVhUCjQwIFBSSU5UICJUSEFUIElTIEVOT1VHSC4iCg) |
| **Look:** every second character is reversed | `10 PRINT CHR$(18);CHR$(205.5+RND(1));CHR$(146);CHR$(205.5+RND(1));:GOTO 10` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUFJJTlQgQ0hSJCgxOCk7Q0hSJCgyMDUuNStSTkQoMSkpO0NIUiQoMTQ2KTtDSFIkKDIwNS41K1JORCgxKSk7OkdPVE8gMTAK) |
| **Time:** a clock in the corner | `10 PRINT CHR$(147)`<br>`20 PRINT CHR$(19);TI$`<br>`30 GOTO 20` | [run](https://dewalt-1.github.io/10print-room/#r=MTAgUFJJTlQgQ0hSJCgxNDcpCjIwIFBSSU5UIENIUiQoMTkpO1RJJAozMCBHT1RPIDIwCg) |

More to combine:

- `INPUT "YOUR NAME";N$`: ask a question. The answer is stored in `N$`.
- `IF … THEN`: do something only if a condition is true.
- `GOSUB 100` … `RETURN`: use the same lines of code again.
- `PRINT CHR$(147)`: clear the screen. `PRINT CHR$(19);`: continue at the top left corner.
- `POKE 32768+X+40*Y,C`: put character code `C` at column `X`, row `Y` (graphics are codes 64 to 127).
- `PEEK(32768+X+40*Y)`: the code of the character that is already there.
- `POKE 59468,14`: lower case. `POKE 59468,12`: back to upper case.
- Add `?speed=4` to the web address for a slower computer, or `?phosphor=green` or `?phosphor=amber` for another screen colour. Put it before the `#` if there is one.

## Hand it in

Click **link** to copy a web address with your whole program in it. Post it to are.na with one
sentence on what you changed and why. Click **save** to download a `.bas` file.

## Code

The code of the BASIC room is in another repository: [dewalt-1/10print-room](https://github.com/dewalt-1/10print-room).
