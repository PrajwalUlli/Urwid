### Buttons

Widget Used
- Button(label=\<str>, on_press=\<callable>, align=\<'left'/'center'/'right'>, wrap=\<'space'/'any'/'clip'/'ellipsis'>)
	- Signals supported: `click`

CODE
```py
from urwid import (
    Text, Filler, AttrMap,
    Edit,
    Pile, Divider,
    Button,
    MainLoop, ExitMainLoop
)
from time import sleep as s

palette = [
    ("Bold", 'bold', '', ''),
    ("OnHoverExit", '', '', '', '#f00, bold', ''),
    ("OnHoverToggle", '', '', '', '#0df, bold', ''),
]

class CustButton(Button):
    button_left = Text("")
    button_right = Text("")

mask_toggled = True
def handler(key):
    global mask_toggled
    if 'Button' in str(type(key)):
        if 'Toggle' in key.get_label():
            if mask_toggled:
                btn.base_widget.set_label('Toggle 🙊')
                ask.set_mask(None)
                mask_toggled = False
            else:
                btn.base_widget.set_label('Toggle 🙈')
                ask.set_mask('*')
                mask_toggled = True
        elif key.get_label() == 'Exit':
            raise ExitMainLoop()

ask = Edit(("Bold", "Enter the password?\n"), align='center', mask='*')

btn = AttrMap(CustButton("Toggle mask", on_press=handler, align='center'), '', 'OnHoverToggle')
btn2 = AttrMap(CustButton("Exit", on_press=handler, align='center'), '', 'OnHoverExit')

div = Divider()
pile = Filler(Pile([ask, div, btn, btn2]))

loop = MainLoop(pile, palette, unhandled_input=handler)
loop.screen.set_terminal_properties(256)
loop.run()
```

EXPLANATION
- Works just like a button and syntax similar to an `Edit` widget
- The `CustButton` is a custom subclass inheriting from the [`Button`](https://urwid.org/reference/widget.html#urwid.Button) widget and removes those ugly angle brackets they have going on. 
