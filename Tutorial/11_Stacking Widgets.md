### Stacking Widgets

Widget Used
- Pile(widget_list=\<([])>, focus_item=<widget/index>)
- Divider(div_char=\<char>, top=\<int>, bottom=\<int>)

CODE
```py
from urwid import Text, Filler, MainLoop, ExitMainLoop, Pile, AttrMap, Divider

def Exit(key):
    if (key in ('q', 'Q', 'esc')):
        raise ExitMainLoop()

palette = [
    ("top1", "", "", "", "g100", "#00a"),
    ("top2", "", "", "", "g100, bold", "#f60"),
    ("top3", "", "", "", "g100, bold", "#800"),
    ("bg", "", "", "", "g7", "#ad6"),
    ("line", "", "", "", "#800", "#ad6"),
]
 
gap = Filler(AttrMap(Divider("─"), 'line'))
txt1 = Filler(Text(("top2", " Given three rows (Fixed) "), align="center"))
txt2 = Filler(Text(("top1", " Gets 1/4th of available space. This is a pretty looong text written by me "), align="center"))
txt2_1 = Filler(Text(("top1", " Gets 3/4th of available space. This is a pretty looong text written by me. "), align="center"))
txt3 = Filler(Text(("top2", " Given minimum available rows "), align="center"))
footer = Filler(Text((" Default weighed. I am the last one in the 'pile' "), align="center"))

pile1 = AttrMap(Pile([
    gap,
    ('given', 3, txt1),
    gap,
    ('weight', 1, txt2),
]), 'bg')

loop = MainLoop(pile1, palette, unhandled_input=Exit)

# Access the underlying Pile
pile1 = loop.widget.base_widget

# takes min space needed to fit content in available space (rows)
pile1.contents.append((txt2_1, ("weight", 3)))
  
for item in (gap, footer, gap):
    # similar to Pile.contents.append(item, Pile.options())
    pile1.contents.append((item, pile1.options('pack', None))) # default: ('weight', 1)

loop.screen.set_terminal_properties(256)
loop.run()
```

EXPLANATION
- The Pile widget in Urwid is a [container](https://urwid.org/manual/widgets.html#container-widgets) widget. 
- [`Pile`](https://urwid.org/reference/widget.html#urwid.Pile) widget stacks given widgets vertically from top to bottom.
- [`Divider`](https://urwid.org/reference/widget.html#urwid.Divider) is acts like a break tag in html.
- Container Widgets like `Pile` have a `contents` property that we can treat like a list of (_widget_, _options_) tuples. [`Pile.contents`](https://urwid.org/reference/widget.html#urwid.Pile.contents "urwid.Pile.contents") supports normal list operations including `append()` to add child widgets.
- Add more widgets to a `Pile` using the *append()* method in [`contents`](https://urwid.org/reference/widget.html#urwid.Pile.contents) property.
	- append(\<tuple of (widget, options)>)
- To pass the same options to multiple widgets [*options()*](https://urwid.org/reference/widget.html#urwid.Pile.options) method in `Pile` can be used.
	- options(height_type=\<pack/given/weight>, height_amount=<None/int/int>)

> **NOTE:**
>  The Pile Options
>  - ('given', \<num>): assigns a specific number of rows to the widget. Its like giving a fixed amount of rows to a widget. It will always try to assign the given amount of rows as the screen is resized or zoomed. This option always try to claim the specified number of rows before passing any remaining space to other widgets. And if there isn’t enough overall space, the items further down can get compressed or even hidden.
>  - ('weight', \<num>): allocates space in proportion to the previous weighted widget. For example, "weight,2" and "weight,5" means the second widget is allocated space in a 2:5 ratio compared to the third. The more the widget weighs, more the control it has over the remaining space than the prev weighted widget.
>  - ('pack', None): allocates the widget only enough space to fit its content exactly in available as the screen is resized or zoomed. Once you define a widget in the Pile as ('pack', widget), it doesn’t consume leftover space (unlike 'weight') nor does it claim a fixed number of rows (like 'given'). Instead, it’s sized to whatever it needs at that moment.
>
> Whatever remains after all the “given” items (if anything) is distributed among the “weight” items based on their weight values. A weight=2 item gets twice as much of the leftover space as a weight=1 item.
> For example, if the screen is too small for the sum of all “given” rows, the lower widgets (including other “given” lower down the pile or those with “weight”) will be truncated (not shown) to fit the screen.
> Precedence based on option: 'given' > 'pack' > 'weight'
> Precedence based on order: the widget lower down in the pile gets affected that the ones at the top.