### User Inputs

Widget Used
- Edit(caption=\<str>, edit_text=\<placeholder str>, multiline=<True/False>, align=\<left/center/right>, wrap=<space/any/clip/ellipsis>, allow_tab=<True/False>, mask=\<char>)

CODE
```py
from urwid import (
    Text,
    Filler,
    MainLoop,
    ExitMainLoop,
    Edit,
)

def Exit(key):
    if key in ('q', 'Q'):
        raise ExitMainLoop

# To override a Urwid widget, inherit it to a new class and modify its methods
# Here, InputHandler can be treated as Filler Widget, as called a subclass of the Filler Widget in Urwid.
class InputHandler(Filler):
    # param order matters
    # self has to be passed so the method is acted upon the instance that called it
    def keypress(self, size, key):
        if key == 'enter':
            self.original_widget = Text(f"Your pwd leaked, {self.base_widget.edit_text!r}!\n\nUse 'Q' to quit", 'center')
            return None
        # super refers to the parent class this subclass is inheriting from, ie, Filler  
        super().keypress(size, key)

# Create an Edit widget with a prompt
# wrapping it with a subclass of Filler widget
edit = InputHandler(Edit("Whats your password? ", align='center', mask='o'))

loop = MainLoop(edit, unhandled_input=Exit)
loop.screen.set_terminal_properties(256)
loop.run()
```

EXPLANATION
- The [`Edit`](https://urwid.org/reference/widget.html#urwid.Edit "urwid.Edit") widget is based on the `Text` widget but it accepts keyboard input for entering text, making corrections and moving the cursor around with the HOME, END and arrow keys.
- Here we are customizing the `Filler` decoration widget that is wrapping our `Edit` widget by subclassing it and defining a new *keypress()* method. 
- Customizing decoration/container widgets to handle input this way is a common pattern in Urwid applications. This pattern is easier to maintain and extend than handling all special input in an *unhandled_input* function.
- *keypress()* is a method already inside `Filler` widget which here is being modified to do something else. The params passed to it in the function definition are the same and in order as per the one it has defined in the [docs](https://urwid.org/reference/widget.html#urwid.Filler). The *self* param has to be passed to every function in a class so that the method calls are applied to the instance that called it, and not to other instances.
- The *super()* method is used to call the actual parent class the subclass `InputHandler` is inheriting from.
- The *self* keyword is refering to the instance of the class being used ie "edit" in this case.
- A *None* return to the `MainLoop` means that the key inputs have been handled. While returning the key back to the loop means it has to be handled by the *Exit* function.  