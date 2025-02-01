### High Color Modes

**Methods Used**
- set_terminal_properties(colors=<8/16/88/256>, bright_is_bold=<True/False>)
	- some terminals use brightness as a way to show a text is bold

**CODE**
```py
import urwid as u

def exit_on_q(key):
    if key in ['q', 'Q']:
        raise u.ExitMainLoop()

palette = [
    ("txt-styles", '', '', '', '#fff, underline, bold', '#f60'),
    ("txt-bg", '', '', '', '', "#ff0"),
    ("bg", '', '', '', '', "#f00"),
]

txt = u.Text(('txt-styles', " Hello World! "), 'center')
fill = u.Filler(txt)
loop = u.MainLoop(fill, palette=palette, unhandled_input=exit_on_q)
loop.screen.set_terminal_properties(colors=256)
loop.run()
```

**EXPLANATION**
- The [screen](https://urwid.org/manual/displayattributes.html) object this main loop uses for screen updates and reading input. Default is a new `raw_display.Screen` instance; stored as `screen`.
- When setting up a palette with `MainLoop` (or directly on your screen instance), you may specify attributes (for 16-color, monochrome and high color modes). You can then switch between these modes with `screen.set_terminal_properties()`, where `screen` is your screen instance or `MainLoop.screen`.