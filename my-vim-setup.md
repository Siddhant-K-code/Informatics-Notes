# My Vim Setup

I realize that although setups are not technically informatics, a good setup means better productivity: you can accomplish more in less time. Thus, I will still write about them. 

When it comes to writing programs, Vim beats every text editor and IDE out there. I will not detail the reasons necessary to prove this, as this can be easily searched up, but Vim has extreme performance gains and is very close to the OS level. This makes it easy to not only write and edit text but also write commands, automate, and organize, complete with its own scripting language. 

In terms of which Vim to use, I personally use MacVim \(gVim for Mac\). It conveniently provides its own graphical interface, comes with complete colors, and is well-integrated for Mac \(similar to how gVim is with Windows and possibly even better\). Other popular choices include NeoVim or just plain vanilla Vim, but these have much steeper learning curves if you want to set them up to what is offered with MacVim. 

Now, we can look at some of the best features we can get with Vim. This is by no means a complete list, but rather what I experiment with and what generally works well. If you are just starting out, these can seem overwhelming, which means you might want to use a standard text editor as a crutch. MacVim can be easily used as a standard text editor if you enable GUI Mac edit movements, which can be easily searched up. 

Plugins I use: 

* VundleVim \(to actually install plugins easily\)
* Cpp Enhanced Highlight \(to get better syntax coloring in C++\)
* One Dark Vim \(a port of One Dark, a rich dark color scheme, for Vim\)
* NERD Tree \(an integrated file manager in Vim\)
* NERD Tree Tabs \(to have a tree consistent with all files\)
* Vim Airline \(a status bar light as air, along with a nice tabline that shows buffers\)
* NERD Commenter \(to quickly comment code in an extensible way for numerous languages\)

Theme/Font I use: 

One Dark Vim with Source Code Pro \(13\)

Intrinsic keybindings I use:

* **hjkl** instead of arrow keys \(I also replace j and k with gj and gk to go up and down graphical lines instead of just those associated with line number unless there is a number prefixing j or k\)
  * this also includes commands like {\#}j for example to go down some number of lines \(when prefixing with a number, I define j to actually invoke the line numbers in the file instead of graphical lines\)
* **{\#}G** to go to an absolute line number directly
* **a** to go to insert mode \(prefer over i\), **A** to go in insert mode at end of line and its opposite **I**
* **/{string}** to search for all occurrences of a string in a document \(with n to go next and N for previous\), especially useful for finding files in NERD tree
* **:** to run commands \(such as :b to get to a buffer\)
* **.** to repeat a pattern
* **w** to move word forward, **b** to move word backward

Personal keybindings:

* **CapsLock** to ESC \(using Karabiner\) to normal mode
* **s** for visual mode, **S** for visual line mode
* **SS** to toggle on/off spellcheck
* **z** to toggle on/off relative line numbering \(useful with hjkl\)
* **Z** to NERD comment
* **W** to toggle between the NERD tree and the current file
* **Q** to close/delete the current file buffer \(and remove it from the tabline\)
* **M** to wipe buffer
* **Tab** and **Shift + Tab** to tab and untab
* **;** and **'** to go to left and right buffers on tabline
* **K** to compile a general file, **L** to run \(so that **KML** is enough to compile and run\)





