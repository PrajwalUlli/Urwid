### Get Widget 2

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

txt = Text(('t1', f" {loop.widget.original_widget} "), align="center")
fill = Filler(txt)

# its same as doing "atr.original_widget = fill"
loop.widget.original_widget = fill

# txt2 = Text(('t1', " Hello "), align="center")
# fill.original_widget = txt2

loop.run()
```

**EXPLANATION**
- [Decoration Widgets](https://urwid.org/manual/widgets.html#decoration-widgets) like `AttrMap` have an `original_widget` property that we can assign to change the widget they wrap.
- This is used when there is a container or a wrapper at the top, and you only want to swap or modify whatever that container is wrapping.  
- For example, if your top-level widget is an `AttrMap`, it’s often helpful to keep that `AttrMap` so you preserve color or style settings, but change what’s inside it. In that case, you’d do: `loop.widget.original_widget = something_else`  
- This way, the container (`AttrMap`, `Padding`, `Frame`, etc.) remains, and only the content inside that wrapper is replaced.
- The above code will print the widget wrapped by the `AttrMap` (which is the value for `MainLoop.widget`) ie "placeholder" ie `SolidFill` widget, which will look like this: *<SolidFill box widget 'I'>*. 
- So in the backend we can assume the final `MainLoop` remains the same, and the `AttrMap` will look like this: *atr = AttrMap(fill, "bg")*, instead of the "placeholder" it was wrapping.

> **NOTE:**
> Decorantion widgets include:
> - AttrMap – Applies color/attribute settings to a widget.
> - WidgetDisable – Disables user input for its child widget. 
> - Padding – Adds horizontal (or fixed) space around a widget. 
> - Filler – Adds vertical space above and/or below a widget, often used to center or top/bottom-align.
> - LineBox – Draws a box with borders around its child widget.
> - Frame – Places a widget between optional header and footer widgets.
> - Overlay – Overlays one widget on top of another, allowing for layers.
> - BoxAdapter – Alters the height or width of a box widget to present a different size.

>**NOTE:** `<decorwidget>.original_widget` is for modifying the internal “child” widget of a wrapper.