### Minimal Application 2

**Widget Used**
- SolidFill(fill_char: <str=' '>)

**CODE**
```py
from __future__ import annotations
import urwid as u

def exit_on_q(key):
    if key in {"q", "Q"}:
        raise u.ExitMainLoop()

placeholder = u.SolidFill('I')
loop = u.MainLoop(placeholder, palette, unhandled_input=exit_on_q)
loop.run()
```

**EXPLANATION**
- [`SolidFill`](https://urwid.org/reference/widget.html#urwid.SolidFill) is a box widget that fills an area with a single character. the param used is `fill_char` and can be given a single character as a value.
- If given a string of characters only the first character will be considered. 