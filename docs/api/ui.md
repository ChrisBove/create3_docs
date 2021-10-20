# User Interface

The Create® 3 robot can interact with the user through its buttons and its lightring.

### Responding to buttons presses

Whenever a button on the Create® 3 robot is pressed, its information will be published on the `interface_buttons` topic.
By subscribing to this topic, you will be able to detect when to start your custom policies.

### Changing the lightring colors

You can change the color of the lightring on your Create® 3 robot by publishing a corresponding message on the `cmd_lightring` topic.

!!! important 
    The Create® 3 robot uses its lightring to signal about critical events. If you override its color, you may not notice these events.
