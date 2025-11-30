# Vim Cheat Sheet

## Global

|Command|Description|
|---|---|
|`:h[elp] keyword`|Open help for keyword|
|`:sav[eas] file`|Save file as|
|`:clo[se]`|Close current pane|
|`:ter[minal]`|Open a terminal window|
|`K`|Open man page for word under the cursor|

> **Tip:** Run `vimtutor` in a terminal to learn the first Vim commands.

## Cursor Movement

### Basic Movement

|Command|Description|
|---|---|
|`h`|Move cursor left|
|`j`|Move cursor down|
|`k`|Move cursor up|
|`l`|Move cursor right|
|`gj`|Move cursor down (multi-line text)|
|`gk`|Move cursor up (multi-line text)|

### Screen Positioning

|Command|Description|
|---|---|
|`H`|Move to **top** of screen|
|`M`|Move to **middle** of screen|
|`L`|Move to **bottom** of screen|
|`zz`|Center cursor on screen|
|`zt`|Position cursor on top of the screen|
|`zb`|Position cursor on bottom of the screen|
|`Ctrl` + `e`|Move screen down one line (without moving cursor)|
|`Ctrl` + `y`|Move screen up one line (without moving cursor)|
|`Ctrl` + `b`|Move screen up one page (cursor to last line)|
|`Ctrl` + `f`|Move screen down one page (cursor to first line)|
|`Ctrl` + `d`|Move cursor and screen down 1/2 page|
|`Ctrl` + `u`|Move cursor and screen up 1/2 page|

### Word Movement

|Command|Description|
|---|---|
|`w`|Jump forwards to the start of a word|
|`W`|Jump forwards to the start of a word (words can contain punctuation)|
|`e`|Jump forwards to the end of a word|
|`E`|Jump forwards to the end of a word (words can contain punctuation)|
|`b`|Jump backwards to the start of a word|
|`B`|Jump backwards to the start of a word (words can contain punctuation)|
|`ge`|Jump backwards to the end of a word|
|`gE`|Jump backwards to the end of a word (words can contain punctuation)|

### Line & Document Movement

|Command|Description|
|---|---|
|`0`|Jump to the start of the line|
|`^`|Jump to the first non-blank character of the line|
|`$`|Jump to the end of the line|
|`g_`|Jump to the last non-blank character of the line|
|`gg`|Go to the first line of the document|
|`G`|Go to the last line of the document|
|`5gg` or `5G`|Go to line 5|
|`}`|Jump to next paragraph (or function/block)|
|`{`|Jump to previous paragraph (or function/block)|
|`%`|Move to matching character (default: '()', '{}', '[]')|
|`gd`|Move to local declaration|
|`gD`|Move to global declaration|

### Search Character

|Command|Description|
|---|---|
|`fx`|Jump to next occurrence of character **x**|
|`tx`|Jump to before next occurrence of character **x**|
|`Fx`|Jump to previous occurrence of character **x**|
|`Tx`|Jump to after previous occurrence of character **x**|
|`;`|Repeat previous f, t, F or T movement|
|`,`|Repeat previous f, t, F or T movement, backwards|

> **Tip:** Prefix a cursor movement command with a number to repeat it. For example, `4j` moves down 4 lines.

## Insert Mode

|Command|Description|
|---|---|
|`i`|Insert before the cursor|
|`I`|Insert at the beginning of the line|
|`a`|Insert (append) after the cursor|
|`A`|Insert (append) at the end of the line|
|`o`|Append (open) a new line below the current line|
|`O`|Append (open) a new line above the current line|
|`ea`|Insert (append) at the end of the word|
|`Ctrl` + `h`|Delete the character before the cursor|
|`Ctrl` + `w`|Delete word before the cursor|
|`Ctrl` + `j`|Add a line break at the cursor position|
|`Ctrl` + `t`|Indent (move right) line one shiftwidth|
|`Ctrl` + `d`|De-indent (move left) line one shiftwidth|
|`Ctrl` + `n`|Insert (auto-complete) next match|
|`Ctrl` + `p`|Insert (auto-complete) previous match|
|`Ctrl` + `r` `x`|Insert contents of register `x`|
|`Ctrl` + `o` `x`|Temporarily enter normal mode to issue one command `x`|
|`Esc` or `Ctrl` + `c`|Exit insert mode|

## Editing

|Command|Description|
|---|---|
|`r`|Replace a single character|
|`R`|Replace more than one character, until `Esc` is pressed|
|`J`|Join line below to the current one with one space in between|
|`gJ`|Join line below to the current one without space in between|
|`gwip`|Reflow paragraph|
|`cc`|Change (replace) entire line|
|`c$` or `C`|Change (replace) to the end of the line|
|`ciw`|Change (replace) entire word|
|`cw` or `ce`|Change (replace) to the end of the word|
|`s`|Delete character and substitute text|
|`S`|Delete line and substitute text|
|`xp`|Transpose two letters (delete and paste)|
|`u`|Undo|
|`U`|Restore (undo) last changed line|
|`Ctrl` + `r`|Redo|
|`.`|Repeat last command|

### Case Changing

|Command|Description|
|---|---|
|`g~`|Switch case up to motion|
|`gu`|Change to lowercase up to motion|
|`gU`|Change to uppercase up to motion|

## Visual Mode

|Command|Description|
|---|---|
|`v`|Start visual mode, mark lines, then do a command|
|`V`|Start linewise visual mode|
|`Ctrl` + `v`|Start visual block mode|
|`o`|Move to other end of marked area|
|`O`|Move to other corner of block|
|`aw`|Mark a word|
|`ab`|A block with ()|
|`aB`|A block with {}|
|`at`|A block with <> tags|
|`ib`|Inner block with ()|
|`iB`|Inner block with {}|
|`it`|Inner block with <> tags|
|`>`|Shift text right|
|`<`|Shift text left|
|`y`|Yank (copy) marked text|
|`d`|Delete marked text|
|`~`|Switch case|

> **Tip:** Instead of `b` or `B` one can also use `(` or `{` respectively.

## Registers

|Command|Description|
|---|---|
|`:reg[isters]`|Show registers content|
|`"xy`|Yank into register x|
|`"xp`|Paste contents of register x|
|`"+y`|Yank into the system clipboard register|
|`"+p`|Paste from the system clipboard register|

**Special Registers:**

- `0` - Last yank
    
- `"` - Unnamed register, last delete or yank
    
- `%` - Current file name
    
- `#` - Alternate file name
    
- `*` - Clipboard contents (X11 primary)
    
- `+` - Clipboard contents (X11 clipboard)
    
- `/` - Last search pattern
    
- `:` - Last command-line
    
- `.` - Last inserted text
    
- `-` - Last small (less than a line) delete
    
- `_` - Black hole register
    

## Marks, Jumps, and Macros

|Command|Description|
|---|---|
|`:marks`|List of marks|
|`ma`|Set current position for mark A|
|`` `a ``|Jump to position of mark A|
|`y` `a`|Yank text to position of mark A|
|`` `0 ``|Go to position where Vim was last exited|
|`` `.` ``|Go to position of last change|
|``|Go to position before last jump|
|`:ju[mps]`|List of jumps|
|`:changes`|List of changes|
|`Ctrl` + `i`|Go to newer position in jump list|
|`Ctrl` + `o`|Go to older position in jump list|
|`g,`|Go to newer position in change list|
|`g;`|Go to older position in change list|
|`Ctrl` + `]`|Jump to the tag under cursor|
|`qa`|Record macro a|
|`q`|Stop recording macro|
|`@a`|Run macro a|
|`@@`|Rerun last run macro|

## Cut and Paste

|Command|Description|
|---|---|
|`yy`|Yank (copy) a line|
|`2yy`|Yank (copy) 2 lines|
|`yw`|Yank (copy) characters to start of next word|
|`yiw`|Yank (copy) word under the cursor|
|`yaw`|Yank (copy) word under the cursor and space|
|`y$` or `Y`|Yank (copy) to end of line|
|`p`|Put (paste) after cursor|
|`P`|Put (paste) before cursor|
|`gp`|Put (paste) after cursor and leave cursor after new text|
|`gP`|Put (paste) before cursor and leave cursor after new text|
|`dd`|Delete (cut) a line|
|`dw`|Delete (cut) characters to start of next word|
|`diw`|Delete (cut) word under the cursor|
|`daw`|Delete (cut) word and space|
|`x`|Delete (cut) character|

### Delete Ranges

|Command|Description|
|---|---|
|`:3,5d`|Delete lines starting from 3 to 5|
|`:.,$d`|Delete from current line to end of file|
|`:g/{pattern}/d`|Delete all lines containing pattern|
|`:g!/{pattern}/d`|Delete all lines **not** containing pattern|

## Indent Text

|Command|Description|
|---|---|
|`>>`|Indent (move right) line one shiftwidth|
|`<<`|De-indent (move left) line one shiftwidth|
|`>%`|Indent a block with () or {} (cursor on brace)|
|`<%`|De-indent a block with () or {} (cursor on brace)|
|`>ib`|Indent inner block with ()|
|`>at`|Indent a block with <> tags|
|`3==`|Re-indent 3 lines|
|`=%`|Re-indent a block with () or {} (cursor on brace)|
|`=iB`|Re-indent inner block with {}|
|`gg=G`|Re-indent entire buffer|
|`]p`|Paste and adjust indent to current line|

## Exiting

|Command|Description|
|---|---|
|`:w`|Write (save) the file, but don't exit|
|`:w !sudo tee %`|Write out the current file using sudo|
|`:wq` or `:x` or `ZZ`|Write (save) and quit|
|`:q`|Quit (fails if there are unsaved changes)|
|`:q!` or `ZQ`|Quit and throw away unsaved changes|
|`:wqa`|Write (save) and quit on all tabs|

## Search and Replace

|Command|Description|
|---|---|
|`/pattern`|Search for pattern|
|`?pattern`|Search backward for pattern|
|`\vpattern`|'Very magic' pattern (regex symbols don't need escaping)|
|`n`|Repeat search in same direction|
|`N`|Repeat search in opposite direction|
|`:%s/old/new/g`|Replace all old with new throughout file|
|`:%s/old/new/gc`|Replace all old with new throughout file with confirmations|
|`:noh[lsearch]`|Remove highlighting of search matches|

## Search in Multiple Files

|Command|Description|
|---|---|
|`:vim[grep] /pattern/ {file}`|Search for pattern in multiple files|
|`:vim /foo/ **/*`|Example: Search for "foo" in all files recursively|
|`:cn[ext]`|Jump to the next match|
|`:cp[revious]`|Jump to the previous match|
|`:cope[n]`|Open a window containing the list of matches (Quickfix)|
|`:ccl[ose]`|Close the quickfix window|

## Tabs, Buffers, and Windows

### Tabs

|Command|Description|
|---|---|
|`:tabnew`|Open a file in a new tab|
|`Ctrl` + `wT`|Move the current split window into its own tab|
|`gt` or `:tabn`|Move to the next tab|
|`gT` or `:tabp`|Move to the previous tab|
|`#gt`|Move to tab number #|
|`:tabm[ove] #`|Move current tab to the #th position|
|`:tabc[lose]`|Close the current tab|
|`:tabo[nly]`|Close all tabs except the current one|

### Buffers

|Command|Description|
|---|---|
|`:e[dit] file`|Edit a file in a new buffer|
|`:bn[ext]`|Go to the next buffer|
|`:bp[revious]`|Go to the previous buffer|
|`:bd[elete]`|Delete a buffer (close a file)|
|`:ls`|List all open buffers|

### Windows

|Command|Description|
|---|---|
|`:sp[lit] file`|Open a file in a new buffer and split window|
|`:vs[plit] file`|Open a file in a new buffer and vertically split window|
|`Ctrl` + `ws`|Split window horizontally|
|`Ctrl` + `wv`|Split window vertically|
|`Ctrl` + `ww`|Switch windows|
|`Ctrl` + `wq`|Quit a window|
|`Ctrl` + `wh/j/k/l`|Move cursor to the left/down/up/right window|
|`Ctrl` + `wH`|Move window to far left|
|`Ctrl` + `wL`|Move window to far right|
|`Ctrl` + `wJ`|Move window to very bottom|
|`Ctrl` + `wK`|Move window to very top|
|`Ctrl` + `w=`|Make all windows equal height & width|

### ✅ Pro Tip

> **Vim mastery comes from composing commands:** `Operator` + `Motion` = **Power**
> 
> Examples:
> 
> - `daw` (Delete Around Word)
>     
> - `ci(` (Change Inside Parentheses)
>     
> - `yap` (Yank Around Paragraph)
>     
> - `gUiw` (Make Inner Word Uppercase)
>