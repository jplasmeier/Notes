# n Days of Clojure

I'm learning Clojure. I'm also documenting the process through a series of incremental posts. I plan to spend at least one hour each day as often as possible on doing things with Clojure.

## day 0: 03/24/2017

I set up emacs with a bunch of cool plugins from [this GitHub repo.](https://github.com/flyingmachine/emacs-for-clojure)

## day 1: 03/25/2017

I watched two videos of Rich Hickey, the creator of Clojure, explaining the data structures of Clojure: [Clojure Data Structures Part 1](https://www.youtube.com/watch?v=ketJlzX-254) and [Part 2](https://www.youtube.com/watch?v=sp2Zv7KFQQ0). The speed (effectively constant time indexing) and immutability and persistance of the data structures is very impressive. I took notes: 

(link to git - Clojure Data Structures)

## day 2: 03/26/2017

I came up with an idea for a useful program to write in Clojure. I had been using `csvfilter`, a Python package to play around with a `.csv` file for a project. It was pretty slow, I assume, because Python is slow. So, that could be a cool idea for a first project. 

I read a bunch of [this Clojure tutorial](http://www.braveclojure.com/do-things) and took notes:

(link to git - Clojure Function Notes)

## day 3: 03/28/2017

I consolidated all of the relevant key bindings for emacs and paredit into a single place.

(link to git - Editor and Environment Notes)

## day 4: 03/31/2017

I spent some time messing around with the idea from day 2 to write a replacement for `csvfilter` in Clojure, and gave the project a name: Quip. My code eventually worked but only by consuming the entire file into memorty, which crashes for large files. I'm not really sure how to only take the lines that I need into memory while processing the entire file. 

If this is even possible, I think it would make the most sense to spend some more time familiarizing myself with Clojure in general before attempting to solve a problem without knowing what tools I have at my disposal.


