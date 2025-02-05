### Get Widget 3

CODE
```py
from urwid import *

def Exit(key):
    if (key in ('q', 'Q', 'esc')):
        raise ExitMainLoop()

palette = [
    ("txt", "white", "", "", "g100", "#00a"),
    ("bg", "", "", "", "#800", "#080"),
]

txt = Filler(AttrMap(Text(("txt", " Hello World "), align="center"), 'bg'))

loop = MainLoop(txt, palette, unhandled_input=Exit)
 
base_wid = loop.widget.base_widget
# here set_text overrides the text widget's style attrib too, so AttrMap's styling takes over
txt.base_widget.set_text(f"{base_wid}")

loop.screen.set_terminal_properties(256)
loop.run()
```

EXPLANATION
- The `base_widget` property returns the very core widget that is being wrapped.
- In this case the `Text` widget is being wrapped by `AttrMap` which is being wrapped by `Filler`, if *original_widget* property was used then it would have returned only the widget that the `Filler` wrapper directly contains ie `AttrMap`. So we used `base_widget` property that drills down through all of those layers to reveal the innermost, actual widget ie `Text`.