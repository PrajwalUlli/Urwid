### Get Widget

**CODE**
```py
from urwid import Text, Filler, MainLoop, AttrMap, SolidFill, Pile, Divider, ExitMainLoop

def exit_on_q(key):
    if key in ("q", "Q", "esc"):
        raise ExitMainLoop()

palette = [
    ("t1", "", "", "", "g100", "#00a"),
    ("bg", "", "", "", "g7", "#ad6"),
]

placeholder = SolidFill('I')
atr = AttrMap( placeholder, "bg")
loop = MainLoop(atr, palette, unhandled_input=exit_on_q)
loop.screen.set_terminal_properties(colors=256)

txt = Text(('t1', f" {loop.widget} "), align="center")
fill = Filler(txt)

loop.widget = fill

loop.run()
```

**EXPLANATION**
- We change the topmost widget used by the [`MainLoop`](https://urwid.org/reference/main_loop.html#urwid.MainLoop "urwid.MainLoop") by assigning to its [`MainLoop.widget`](https://urwid.org/reference/main_loop.html#urwid.MainLoop.widget "urwid.MainLoop.widget") property (the topmost widget used to draw the screen. This must be a box widget).
- `widget` Points to the actual top-level widget used by the MainLoop.  
- When you set `loop.widget`, you’re replacing the entire UI with your new widget.  
- If your top-level widget is a wrapper of some sort (like an `AttrMap` for color styling), setting `loop.widget` might remove that wrapper entirely if you replace it with a new widget.
- The above code will print the topmost widget wrapped by the `MainLoop` ie the `AttrMap` widget, which will look like this: *<AttrMap box widget <SolidFill box widget 'I'> attr_map={None:bg'}>*.
- So in the backend we can assume this is what the final `MainLoop` would look like: *MainLoop(fill, unhandled_input=exit_on_q)*, instead of the atr which was previously there.

> **NOTE:** In short, `loop.widget` is for setting a brand-new top-level widget