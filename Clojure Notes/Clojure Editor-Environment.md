# Clojure Editor/Environment

### Basics

`emacs -nw` - opens emacs in terminal, not X window

|Keys	|Description|
|------|-----------|
|C-g   | Cancel current action |
|C-x k | Kill current buffer|
|C-x b | Switch buffers. Type to create the name of a new buffer|
|C-x C-f| Open a file. Type to create a new file|
|C-x C-s| Save file|
|C-spc | Start highlighting a region|
|C-j	|New line and indent, equivalent to enter followed by tab.|
|M-/	|Hippie expand; cycles through possible expansions of the text before point.|
|M-\	|Delete all spaces and tabs around point. (I use this one a lot.)|
|C-h k key-binding	|Describe the function bound to the key binding. To get this to work, you actually perform the key sequence after typing C-h k.|
|C-h f	|Describe function.|
|C-x o q| Close help window.|
|M-x paredit-mode| toggle paredit-mode|	
|C-h t | Tutorial|
|C-x u | Undo|

### Navgating Text

|Keys	|Description|
|------|-----------|
|C-a	|Move to beginning of line.|
|M-m	|Move to first non-whitespace character on the line.|
|C-e	|Move to end of line.|
|C-f	|Move forward one character.|
|C-b	|Move backward one character.|
|M-f	|Move forward one word (I use this a lot).|
|M-b	|Move backward one word (I use this a lot, too).|
|C-s	|Regex search for text in current buffer and move to it. Press C-s again to move to next match.|
|C-r	|Same as C-s, but search in reverse.|
|M-<	|Move to beginning of buffer.|
|M->	|Move to end of buffer.|
|M-g g	|Go to line.|

### Killing and Yanking Text

The kill ring is a place where regions of text go after being killed. The kill ring is similar to where text would go after being `cut` but can store multiple blocks. You can also add things to the kill ring without killing them, similar to `copy`.

|Keys|	Description|
|----|------------|
|C-w	|Kill region.|
|M-w	|Copy region to kill ring.|
|C-y	|Yank.|
|M-y	|Cycle through kill ring after yanking.|
|M-d	|Kill word.|
|C-k	|Kill line.|

### Window Key Bindings

|Keys|	Description|
|----|------------|
|C-x o	|Switch cursor to another window. Try this now to switch between your Clojure file and the REPL.|
|C-x 1	|Delete all other windows, leaving only the current window in the frame. This doesn’t close your buffers, and it won’t cause you to lose any work.|
|C-x 2	|Split frame above and below.|
|C-x 3	|Split frame side by side.|
|C-x 0	|Delete current window.|

### Execution Key Bindings

|Keys|	Description|
|----|------------|
|C-x C-e| `cider-eval-last-expression` - evaluates the current line|
|C-c M-n| sets the namespace to the namespace listed at the top of your current file|
|C-c C-k| compile your current file within the REPL session|

### Clojure Buffer Key Bindings

|Keys|	Description|
|----|------------|
|C-enter| close all open parens and evaluate|
|C-c C-d C-d| display documentation for the symbol under point|
| q| close the documentation buffer|
|M-. | navigate to the source code for the symbol under point|
|M-, | return to the original buffer and position. |
|C-c C-d C-a | search for arbitrary text across function names and documentation|

### Paredit Key Bindings

|Keys|	Description|
|----|------------|
|M-x |paredit-mode	Toggle paredit mode.|
|M-( |Surround expression after point in parentheses (paredit-wrap-round).|
|C-→ |	Slurp; move closing parenthesis to the right to include next expression.|
|C-← | Barf; move closing parenthesis to the left to exclude last expression.|
|C-M-f, C-M-b |	Move to the opening/closing parenthesis.|
|M-s | Delete parens anyway|