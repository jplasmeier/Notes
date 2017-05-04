# Emacs Notes

In support of forking knowledge.

### Commands:

|Command|Action|
|-------|------|
|C-a	|Move to beginning of line.|
|C-b	|Move backward one character.|
|C-e	|Move to end of line.|
|C-f	|Move forward one character.|
|C-s	|Regex search for text in current buffer and move to it. Press C-s again to move to next match.|
|C-r	|Same as C-s, but search in reverse.|
|M-f	|Move forward one word (I use this a lot).|
|M-b	|Move backward one word (I use this a lot, too).|
|M-m |Move to first non-whitespace character on the line.|
|M-<	|Move to beginning of buffer.|
|M->	|Move to end of buffer.|
|M-g g	|Go to line.|

### Kill Ring:

|Command|Action|
|-------|------|
|C-w	|Kill region.|
|M-w	|Copy region to kill ring.|
|C-y	|Yank.|
|M-y	|Cycle through kill ring after yanking.|
|M-d	|Kill word.|
|C-k	|Kill line.|

### Windows

|Command|Action|
|-------|------|
|C-x o	|Switch cursor to another window. Try this now to switch between your Clojure file and the REPL.
|C-x 1|	Delete all other windows, leaving only the current window in the frame. This doesn’t close your buffers, and it won’t cause you to lose any work.
|C-x 2|	Split frame above and below.
|C-x 3|	Split frame side by side.
|C-x 0|	Delete current window.

### Clojure Buffer Key Bindings

|Command|Action|
|-------|------|
|C-c M-n	|Switch to namespace of current buffer.|
|C-x C-e	|Evaluate expression immediately preceding point.|
|C-c C-k	|Compile current buffer.|
|C-c C-d C-d	|Display documentation for symbol under point.|
|M-. and M-,	|Navigate to source code for symbol under point and return to your original buffer.|
|C-c C-d C-a	|Apropros search; find arbitrary text across function names and documentation.|

### CIDER Buffer Key Bindings

|Command|Action|
|-------|------|
|C-↑, C-↓	|Cycle through REPL history.|
|C-enter	|Close parentheses and evaluate.|