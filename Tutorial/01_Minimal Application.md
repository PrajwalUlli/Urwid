### Minimal Application

**Widget Used**
- SolidFill(fill_char: <str=' '>)

**CODE**
```py
import urwid as u

placeholder = u.SolidFill('I')
loop = u.MainLoop(placeholder)
loop.run()
```

**EXPLANATION**
- [`SolidFill`](https://urwid.org/reference/widget.html#urwid.SolidFill) is a box widget that fills an area with a single character. the param used is `fill_char` and can be given a single character as a value.
- If given a string of characters only the first character will be considered. 
- The `MainLoop` is what keeps the program running indefinitely. You can exit out of it for now using `ctrl+c+z` which is like a force quit, and results in *keyboard Interrupt* errors.  