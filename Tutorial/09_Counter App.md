### Counter App

CODE
```py
import pyfiglet as pf
from urwid import Text, Filler, MainLoop, ExitMainLoop

palette = [
    ("red", '', '', '', '#d00', ''),
    ("blue", '', '', '', '#0ad', ''),
    ("white", '', '', '', '#fff', ''),
]

font = 'big'

def Exit(key):
    global num
    if (key in ['q', 'Q', 'esc']):
        raise ExitMainLoop()
    elif (key in '+'):
        num += 1
        ascii_rt = pf.figlet_format(f"{num}", font=font)
        if (num >= 0):
            txt.set_text(('blue', f"{ascii_rt}"))
        else:
            txt.set_text(('red', f"{ascii_rt}"))
    elif (key in '-'):
        num -= 1
        ascii_rt = pf.figlet_format(f"{num}", font=font)
        if (num >= 0):
            txt.set_text(('blue', f"{ascii_rt}"))
        else:
            txt.set_text(('red', f"{ascii_rt}"))

num = 0
ascii_rt = pf.figlet_format(f"{num}", font=font)
txt = Text(('white',  f"{ascii_rt}"), 'center')
fill = Filler(txt)

loop = MainLoop(fill, palette, unhandled_input=Exit)
loop.screen.set_terminal_properties(256)
loop.run()
```
EXPLANATION
- Just a simple app that increases count based on keypresses like Plus to increase the number and Minus to decrease it.
- It uses `pyfiglet` to show a ascii art of that number.
- Uses all the logic learned till now and nothing more.
