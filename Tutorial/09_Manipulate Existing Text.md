### Manipulate Existing Text

CODE
```py
from urwid import Text, Filler, MainLoop, ExitMainLoop, Pile, AttrSpec, AttrMap

def Exit(key):
    if (key in ('q', 'Q', 'esc')):
        raise ExitMainLoop()
    elif (key in ['left']):
        txt.set_text(f'You moved {key!r}')
        txt.set_align_mode('left')
    elif (key in ['right']):
        txt.set_text(f'You moved {key!r}')
        txt.set_align_mode('right')
    else:
        # same as "txt.set_text"
        fill.original_widget.set_text(f'You Pressed {key!r}')
        fill.original_widget.set_align_mode('center')

bg = AttrSpec('#f60, underline', '', 256)
txt = Text((bg,  "Hello World!"), 'center')
fill = Filler(txt)

loop = MainLoop(fill, unhandled_input=Exit).run()
```

EXPLANATION
- Using `set_text` property of the [`Text`](https://urwid.org/reference/widget.html#text) widget we can change the value being displayed by that widget.
- `set_text` will override any display attribute specified inside the text tuple ie *(bg,  "Hello World!")*. So the styles will have to be specified again inside `set_text` as a tuple like this *set_text((\<style>, \<text>))*.
- When you see `!r` inside an f-string, it means "give me the raw, unfiltered version." Normally, if you write something like f"{value}", Python converts the value using the regular way using *str()*. But putting `!r`, like `f"{value!r}"`, tells Python to use *repr()* instead. *repr()* is like a "detailed" version of the value, often showing extra info like quotes around strings.
- The `set_align_mode` property is self explanatory, it updates the current alignment of the text ie, center, left or right.

> **NOTE:**
> Properties of Text Widget
> - get_text(): Returns (*text, display attributes*) --> tuple
> - set_align_mode(mode:\<alignment>): left/right/center
> - set_layout(align:<left/center/right>, wrap:<space/any/clip/ellipsis>)
> - set_text(markup:\<text>)
> - set_wrap_mode(mode:<space/any/clip/ellipsis>)

