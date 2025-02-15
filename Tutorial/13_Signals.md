### Signals

Widget Used
connect_signal(obj=\<widget>, name=\<signal>, callback=\<func>, weak_args=\<[]>)
- obj (_object_) – the object sending a signal
- name (_signal name_) – the signal to listen for, typically a string
- callback (_function_) – the function to call when that signal is sent

CODE
```py
from urwid import (
    Text, Filler, AttrMap,
    Edit,
    Pile, Divider,
    MainLoop, ExitMainLoop, connect_signal
)

palette = [
    ("Bold", 'bold', '', '')
]

def on_type_change(reply, reply2, widget, new_text):
    reply2.set_text(("", f"Nice to meet you, {new_text}!"))

def exit_on_esc(key):
    if key == 'esc':
        raise ExitMainLoop()
    if key == 'enter':
        reply.set_text(f"You entered: {ask.get_edit_text()}")

ask = Edit(("Bold", "What is your name?\n"), align='center')
reply = Text("text appears here.", align='center')
reply2 = Text("and here too.", align='center')

connect_signal(ask, "change", on_type_change, weak_args=[reply, reply2])

div = Divider()
pile = Filler(Pile([ask, div, reply, div, reply2]))

loop = MainLoop(pile, palette, unhandled_input=exit_on_esc)
loop.screen.set_terminal_properties(256)
loop.run()
```

EXPLANATION
- [Connect signals](https://urwid.org/reference/signals.html#urwid.connect_signal) let you connect a widget's action to a specific function, so that function runs automatically when the action happens.
- When a matching signal is sent, callback will be called.
- The _weak_args_ is an iterable that passes all its arguments to the function, allowing you to assign them to parameters (in the func def) with any names you choose. What matters is their order, which must match the arrangement of variables in the iterable.
- The _weak_args_ parameter passes whatever widgets, arguments, or variables you include in it to the callback function. The order in which the function receives the arguments is as follows: first the *weak_args*, then the *obj* arguments.
- The `Edit` widget provides two signals *change* and *postchange*. Here we use the *change* signal that sends the input given by the user to the function first before it displays in on the `Edit` widget itself.
- There are some keys that are handled by the `Edit` widget like the arrows for moving arround in the text, letter keys for input, backspace for deleting input. Rest are returned to the loop which we can use to exit out of the application.
- The *get_edit_text()* method in `Edit` widget does exactly what it says, gets you the text entered by the user

| Widget        | Signal Name       |
| ------------- | ----------------- |
| Edit          | change, postchage |
| Button        | click             |
| CheckBox      | change            |
| RadioButton   | change            |
| PopUpLauncher | open_pop_up       |
| PopUpLauncher | close_pop_up      |
| ListBox       | modified          |
| TreeWidget    | expanded          |
| TreeWidget    | collapsed         |
