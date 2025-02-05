### Text

**Widgets Used**
- Text(markup=\<tuple with strings & arrtibutes>, align=<left/right/center>, wrap=<space/any/clip/ellipsis>)
	- default align='left'
- Filler(body=\<widget>, Valign=<top/bottom/middle>, top=\<num>, bottom=\<num>)
	- default Valign='middle'
- MainLoop(widget=\<widget>)

**CODE**
```py
import urwid

txt = urwid.Text("Hello World", "center") # align=left/right/center
fill = urwid.Filler(txt, "top") # Valign=top/bottom/middle
loop = urwid.MainLoop(fill)
loop.run()
```

**EXPLANATION**
- The  `Text` widget handles formatting blocks of text, like wrapping to the next line when necessary. Widgets like this are called *flow widgets* (because their sizing can have a number of columns given (in this case the full screen width), then they will flow to fill as many rows as necessary). `Text` has an alignment attribute "align" that aligns text in the X space ie horizontally. So `Text` widget only expects a single dimension for rendering (width), and has no control over the vertical space.
- `Text` can be aligned in the center of the screen by removing the "Valign" param passed in the `Filler` (now the left & right params of the `Text` widget will align the text in the center-left & center-right). 
- `Filler` is a *box widget* (widgets which are given both the no. of columns & rows in which they must be displayed in).
- The `Filler` widget fills in blank lines above or below "flow widgets" so that they can be displayed in a fixed number of rows. This Filler will align our Text to the top of the screen, filling all the rows below with blank lines. `Filler` has an alignment attribute "Valign" that aligns text in the Y space ie vertically.
- The `MainLoop` class handles displaying our widgets as well as accepting input from the user. 
- The widget passed to `MainLoop` is called the “topmost” widget. The topmost widget is used to render the whole screen and so it must be a *box widget*. 
- In this case our widgets can’t handle any user input so we need to interrupt the program to exit with *Ctrl+C*.

> **NOTE:** Not using the `Filler` widget and passing the `Text` directly to `MainLoop`, uwrid tries to give it (rows, cols), causing the “too many values to unpack” error. And the`Text` widget being a *flow* widget expects only one dimension (width) causing extra arguments being passed the widget.