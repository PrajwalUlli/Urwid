### Quit App

**Widget Used**
- MainLoop(widget=\<widget>, unhandled_input=\<func(key)>)

**CODE**
```py 
import urwid

def show_or_exit(key):
    if key in {"q", "Q"}:
        raise urwid.ExitMainLoop()
    txt.set_text(repr(key))

txt = urwid.Text("Hello World")
fill = urwid.Filler(txt, "top")

loop = urwid.MainLoop(fill, unhandled_input=show_or_exit)
loop.run()
```

 **EXPLANATION**
 - The `MainLoop` class has an optional function parameter _unhandled_input_. 
 - This function will be called once for each keypress that is not handled by the widgets being displayed, and since none of the widgets here handle input, every key the user presses will be passed to the _show_or_exit_ function.
- The `ExitMainLoop` exception is used to exit cleanly from the `MainLoop.run()` function when the user presses _Q_. All other input is displayed by replacing the current Text widget’s content.

> **NOTE:** The `repr()` function is a python function that disables text manipulations like escape sequences, and returns them as is.