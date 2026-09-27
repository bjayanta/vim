# Vim Practice

The Plan

Stage 1: Survival mode — modes, opening/closing, basic movement, insert/save/quit. Enough to not feel trapped in vim.

Stage 2: Efficient editing — deleting, copying/pasting, undo/redo, word/line-level motions. This is where vim starts feeling faster than a normal editor.

Stage 3: Moving fast — jumping around files without arrow keys, searching, line numbers, marks.

Stage 4: Power editing — combining verbs + motions (vim's real superpower), visual mode, find-and-replace.

Stage 5: Customization & workflow — .vimrc basics, multiple files/buffers/splits, registers, macros.

## Stage 1, Step 1: Modes — the core idea

Vim's biggest mental shift from other editors: it has modes. You're not always typing text.

**Normal mode** — the default. Keys are commands, not text. This is where you move around, delete, copy, etc.
**Insert mode** — this is like a normal text editor. What you type appears as text.
**Visual mode** — for selecting text (we'll get here in Stage 4).
**Command-line mode** — for things like saving, quitting, search-and-replace (triggered by :).

The #1 beginner confusion: you open vim, start typing, and letters don't appear — or worse, weird things happen. That's because you're in Normal mode, and every letter is a command.

## Stage 1, Step 2: Saving and quitting

Insert mode, hit Esc first — always. Here are the essential ones:

```text
Command What it does

:w  = Write (save) the file
:q  = Quit (only works if no unsaved changes)
:wq = Save and quit
:q! = Quit without saving (force quit, discard changes)
:x  = Save and quit (only writes if there were changes — slightly smarter than :wq)
```

## Stage 1, Step 3: Moving around with h j k l

Vim purists avoid the arrow keys — not just for tradition, but because h j k l keep your fingers on the home row, so you never leave typing position to move around.

| Key | Direction | Mnemonic                 |
| --- | --------- | ------------------------ |
| h   | left      | leftmost key             |
| j   | down      | looks like a down-hook ↓ |
| k   | up        | opposite of j            |
| l   | right     | rightmost key            |

Arrow keys do still work in vim, so nothing breaks if you slip — but training on **hjkl** early pays off fast.

NB. A useful trick: typing a number before a movement key repeats it. Try 5j — moves down 5 lines in one go. Try 3l — moves right 3 characters.

## Stage 1, Step 4: Word and line jumps

Moving one character at a time is slow. Vim gives you bigger jumps.

Word movement:

| Key | Moves to                      |
| --- | ----------------------------- |
| w   | start of next word            |
| b   | start of previous word (back) |
| e   | end of current/next word      |

Line movement:

| Key | Moves to                              |
| --- | ------------------------------------- |
| 0   | very start of the line                |
| ^   | first non-blank character of the line |
| $   | end of the line                       |

File movement:

| Key | Moves to                         |
| --- | -------------------------------- |
| gg  | very start of the file           |
| G   | very end of the file             |
| 5G  | line 5 specifically (number + G) |

NB. Combine with counts (remember those from last step?): try 3w (jump forward 3 words) and 2G (jump to line 2).

## Stage 2, Step 1: Deleting text

In vim, deletion doubles as "cut" — anything you delete goes into a register (like a clipboard), so you can paste it later.

Basic deletes:

| Command | Deletes                           |
| ------- | --------------------------------- |
| x       | single character under cursor     |
| dd      | entire current line               |
| dw      | from cursor to start of next word |
| d$ or D | from cursor to end of line        |
| d0      | from cursor to start of line      |

Notice dd, dw, d$ — this is the verb + motion pattern starting to show up. d is the verb ("delete"), and whatever follows (w, $, a line via the doubled dd) is the motion telling it how much.

## Stage 2, Step 2: Undo, redo, and copy/paste

Undo & redo — your safety net

| Command | Effect               |
| ------- | -------------------- |
| u       | undo last change     |
| Ctrl-r  | redo (undo the undo) |
| 5u      | undo last 5 changes  |

u is one of the most-pressed keys once you start editing seriously — never hesitate to delete something experimentally, since u is right there.

---

Copy (called "yank") and paste

Vim calls copying "yank" — the verb is y, and it follows the same verb+motion pattern as d.

| Command | Effect                       |
| ------- | ---------------------------- |
| yy      | yank (copy) the current line |
| 3yy     | yank 3 lines                 |
| yw      | yank to start of next word   |
| p       | paste after cursor/line      |
| P       | paste before cursor/line     |

Important detail: deleting also fills the paste register. So dd then p acts like cut-and-paste — deleting a line and pasting it elsewhere.

## Stage 3, Step 1: Searching

Once files get long, jumping word-by-word isn't enough — you need to search.

| Command  | Effect                                         |
| -------- | ---------------------------------------------- |
| /pattern | search forward for "pattern"                   |
| ?pattern | search backward for "pattern"                  |
| n        | jump to next match                             |
| N        | jump to previous match (opposite direction)    |
| \*       | search for the word under the cursor (forward) |

Search wraps around the file by default, so if you keep pressing n you'll loop back to the top.

## Stage 3, Step 2: Line numbers and marks

Line numbers

Seeing line numbers makes jumping around much easier.

| Command             | Effect                                                               |
| ------------------- | -------------------------------------------------------------------- |
| :set number         | show line numbers                                                    |
| :set nonumber       | hide them again                                                      |
| :set relativenumber | show numbers relative to your cursor line (great for counts like 5j) |

Once numbers are on, you can jump straight to a line: :42 (colon + number) or 42G takes you to line 42.

## Stage 4, Step 1: The verb + motion pattern (vim's superpower)

You've already seen pieces of this: dd, dw, yy, yw. Now let's make the pattern explicit, because once it clicks, vim editing speed jumps dramatically.

The formula: verb + motion (or verb + text object)

Verbs you already know:

**d** = delete
**y** = yank (copy)
**New one: c** = change (delete + drop you into Insert mode, for when you're about to retype something)

Motions you already know: w, b, e, $, 0, G, j, k, etc.

So you can now combine any verb with any motion:

| Combo | Meaning                                              |
| ----- | ---------------------------------------------------- |
| dw    | delete to next word                                  |
| d$    | delete to end of line                                |
| cw    | change to next word (delete word, enter Insert mode) |
| c$    | change to end of line                                |
| y$    | yank to end of line                                  |
| dG    | delete from cursor to end of file                    |
| dgg   | delete from cursor to start of file                  |

---

Text objects — an even more powerful motion type

Instead of "to the next word," you can target a thing: "this word," "this quoted string," "these parentheses."

| Command | Targets                                                           |
| ------- | ----------------------------------------------------------------- |
| diw     | delete inner word (the word itself, cursor can be anywhere in it) |
| daw     | delete a word (word + trailing space)                             |
| di"     | delete inside quotes "..."                                        |
| di(     | delete inside parentheses (...)                                   |
| ci"     | change inside quotes — deletes contents, drops you in Insert mode |

diw is one of the most-used commands in all of vim — it doesn't matter where your cursor is inside the word, it grabs the whole thing.
