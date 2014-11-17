[![Build Status](https://travis-ci.org/thomasballinger/curtsies.svg?branch=master)](https://travis-ci.org/thomasballinger/curtsies)
[![Documentation Status](https://readthedocs.org/projects/curtsies/badge/?version=latest)](https://readthedocs.org/projects/curtsies/?badge=latest)
Curtsies: Terminal interaction
=========================================
![Curtsies Logo](http://ballingt.com/assets/curtsiestitle.png)

Curtsies is a library for interacting with the terminal.
This is what using (nearly every feature of) curtsies looks like:

```python
from __future__ import unicode_literals # convenient for Python 2
import random

from curtsies import FullscreenWindow, Input, FSArray
from curtsies.fmtfuncs import red, bold, green, on_blue, yellow

print(yellow('this prints normally, not to the alternate screen'))
with FullscreenWindow() as window:
    with Input() as input_generator:
        msg = red(on_blue(bold('Press escape to exit')))
        a = FSArray(window.height, window.width)
        a[0:1, 0:msg.width] = [msg]
        for c in input_generator:
            if c == '<ESC>':
                break
            elif c == '<SPACE>':
                a = FSArray(window.height, window.width)
            else:
                s = repr(c).decode()
                row = random.choice(range(window.height))
                column = random.choice(range(window.width-len(s)))
                color = random.choice([red, green, on_blue, yellow])
                a[row, column:column+len(s)] = [color(s)]
            window.render_to_terminal(a)
```

Paste it in a `something.py` file and try it out!

Installation: `pip install curtsies`

[Documentation](http://curtsies.readthedocs.org/en/latest/)

Primer
------

[FmtStr](http://curtsies.readthedocs.org/en/latest/FmtStr.html) objects are strings formatted with
colors and styles displayable in a terminal with [ANSI escape sequences](http://en.wikipedia.org/wiki/ANSI_escape_code>`_).

(the api shown below is outdated)
![fmtstr example screenshot](http://i.imgur.com/7lFaxsz.png)

[FSArray](http://curtsies.readthedocs.org/en/latest/FSArray.html) objects contain multiple such strings
with each formatted string on its own row, and FSArray
objects can be superimposed on each other
to build complex grids of colored and styled characters through composition.

(the api shown below is outdated)
![fsarray example screenshot](http://i.imgur.com/rvTRPv1.png)

Such grids of characters can be rendered to the terminal in alternate screen mode
(no history, like `Vim`, `top` etc.) by [FullscreenWindow](http://curtsies.readthedocs.org/en/latest/window.html#curtsies.window.FullscreenWindow) objects
or normal history-preserving screen by [CursorAwareWindow](http://curtsies.readthedocs.org/en/latest/window.html#curtsies.window.CursorAwareWindow) objects.
User keyboard input events like pressing the up arrow key are detected by an
[Input](http://curtsies.readthedocs.org/en/latest/input.html) object.

Examples
--------

* [Tic-Tac-Toe](/examples/tictactoeexample.py)

![screenshot](http://i.imgur.com/AucB55B.png)

* [Avoid the X's game](/examples/gameexample.py)

![screenshot](http://i.imgur.com/nv1RQd3.png)

* [Bpython-curtsies uses curtsies](http://ballingt.com/2013/12/21/bpython-curtsies.html)

[![ScreenShot](http://i.imgur.com/r7rZiBS.png)](http://www.youtube.com/watch?v=lwbpC4IJlyA)

* [More examples](/examples)

About
-----

* [Curtsies Documentation](http://curtsies.readthedocs.org/en/latest/)
* Curtsies was written to for [bpython-curtsies](http://ballingt.com/2013/12/21/bpython-curtsies.html)
* `#bpython` on irc is a good place to talk about Curtsies, but feel free
  to open an issue if you're having a problem!
* Thanks to the many contributors!
* If all you need are colored strings, consider one of these [other libraries](http://curtsies.readthedocs.org/en/latest/FmtStr.html#rationale)!
