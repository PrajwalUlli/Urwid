### Display Attributes

**Widgets Used**
- MainLoop(widget=\<widget>, palette=\<palette list var>, unhandled_input=\<func(key)>, handle_mouse=<True/False>)
	- handle_mouse: just like enable/disable userInput in css.
- AttrMap(w=\<widget>,  attr_map=\<attribute>, focus_map=\<attribute>)
	- attr_map: applies attributes for the unfocused state.  
	- focus_map: applies attributes for the focused state. (for instance, you can tab or arrow-key focus onto it)
- AttrSpec(fg=<>, bg=<>, colors=<8, 16, 88, 256>)

**CODE**
```py
import urwid

def exit_on_q(key):
    if key in {"q", "Q"}:
        raise urwid.ExitMainLoop()

palette = [ # (attrName, fg, bg, mono, 88-256 fg, 88-256 bg)
    ("outermost", "yellow, strikethrough", ""),
    ("inner1", "dark red", ""),
    ("inner2", "", "yellow"),
    ('txt-block', '', 'dark magenta')
]

  

txt = urwid.Text(markup=("outermost", [" nesting example ", ('inner1', "inside"), " ", ('inner2', "outside")]), align="center")
bg = urwid.AttrSpec('#86f', '#800', 256)
map1 = urwid.AttrMap(txt, 'txt-block') # applies the rest of the values not already used by other text attrs
fill = urwid.Filler(map1)
map2 = urwid.AttrMap(fill, attr_map=bg)
loop = urwid.MainLoop(map2, palette, unhandled_input=exit_on_q)
loop.run()
```

**EXPLANATION**
- [Display attributes](https://urwid.org/manual/displayattributes.html#foreground-background) are defined as part of a palette. A palette is a list of tuples containing:
	- Name of the display attribute, typically a string (!required)
	- Foreground color and settings for 16-color (normal) mode. (!required)
		- Valid settings ( "bold", "underline", "standout", "blink", "italics", "strikethrough" )
		- These settings may be tagged on to foreground colors using commas, eg: `"light gray, underline, bold, strikethrough"`
	- Background color for normal mode. (!required)
	- Settings for monochrome mode (optional). 
		- For monochrome mode combinations of above mentioned are the only values that may be used.
	- Foreground color and settings for 88 and 256-color modes (optional)
	- Background color for 88 and 256-color modes (optional)
- `'default' or ""` may be specified as a foreground or background to use a terminal’s default color. For terminals with transparent backgrounds `'default'` is the only way to show the transparent background.
- The attributes of text in a Text widget is set by using a `(attribute, text)` tuple instead of a simple text string. Display attributes will flow with the text, and multiple display attributes may be specified by combining tuples into a list. This format is called [Text Markup](https://urwid.org/manual/displayattributes.html#text-markup).
	- `Text(('attr1', "String will use attr1"))`
	- `Text(["Uses default display", ('attr1', "Uses attr1")])`
	- `Text([('attr1', "start in attr1 "), ('attr2', "end in attr2")])`
	- `Text(('attr1', ["nesting example ", ('attr2', "inside"), " outside"]))`
		- When markup is nested only the innermost attribute applies. Here `"inside"` has attribute `'attr2'` and all the rest of the text has attribute `'attr1'`.
- An `AttrMap` widget is created to wrap the text widget with display attribute `'streak'`. `AttrMap` widgets allow you to map any display attribute to any other display attribute, but by default they will set the display attribute of everything that does not already have a display attribute. In this case the text has an attribute, so only the areas around the text used for alignment will have the new attribute.
- A second `AttrMap` widget is created to wrap the `Filler` widget with attribute `'bg'`.
- Attributes can be layered on top of one another. Each attribute only defines certain “domains” of styling—like foreground color, background color, boldness, or underline. If an attribute only specifies one domain (e.g., foreground color) and leaves background color unset, then wrapping it with an AttrMap that does specify a background color applies the AttrMap’s background while leaving the original foreground unchanged. 
	- If an attributes' bg is empty it means "default", so if another attribute is trying to set the bg to say "yellow" it wont be able to as the color has already been set i.e., "default". 
	- And, an attribute cannot modify another attributes domain espically when its specified alongside the text itself in a tuple form ex: ('attr1', "Hello").
- Instead of the palettes, you can also specify an exact foreground and background using an [`AttrSpec`](https://urwid.org/reference/attrspec.html#urwid.AttrSpec "urwid.AttrSpec") instance instead of a display attribute name.

> **NOTE:**
> Many terminals will turn foreground colors into their bright versions when you use bold, eg: `'dark blue,bold'` might look the same as `'light blue'`. Some terminals also will display bright colors in a bold font even if you don’t specify bold. To inhibit this you can try setting `bright_is_bold=False` with `BaseScreen.set_terminal_properties()`, but it is not always supported.
