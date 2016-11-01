#The Classic Controller
The Classic Controller (inline it in an iframe and print the events it emits). Or open it in the simulator!

####Some notes (incorporate and delete)
The a and b buttons (placement conveniently for the thumb)
Landscape only
Use of modifiers
Press multiple buttons with one touch 
Configurability
Inspired by NES controller (adapted to touch screen)


###The Joystick

The joystick is a draggable knob the user can pull in any direction indicating a direction and a force. The direction is a angle between X and Y (do longer explanation here) and the force is a number between 0 and 1 indicating how far the user has pulled the knob (1 meaning she pulled it all the way to the edge of the knobs container).



The joystick will work even if the user misses it by a bit (touches down next to it). This makes it easier and more reliable to use, without looking at the controller.



###Buttons
The controller features two buttons, A and B. A is placed conveniently in the lower right corner of the screen making it easy to hit with you bent right thumb. The B button is an arc shaped button placed 'around' A making it easy to hit by stretching the right thumb. The placement of the buttons make it possible to hit both of the with the same finger, by stretching the thumb and placing it flat on the screen (see XXX section below for more info).

The A button will trigger the events `'a.pressstart'` and `'a.press'` when pressed and `'a.pressend'` when released. The B button will trigger similar events (`'b.pressstart'` etc).

The classic controller supports multitouch so that the joystick and both buttons can be activated at the same time.


###Modifiers
Phame has support for `modifiers` (link to documentation), and the Classic controller makes use of these. Each time an event is triggered the pressed state of the A and B buttons are inspected and added to the event. You can inspect their value by accessing the event properties `event.modifiers.a.pressed` and `event.modifiers.b.pressed`.



###Finger/screen contact area
The controller allows you to press both the A and B button with a single touch. Rather than considering a touch as a single touchpoint the controller inspects the entire area of contact between finger and screen. If there is an overlap between this contact area and a button is will be pressed. Note that the contact area is monitored throughout the duration of a touch, meaning that if the finger 'slides off' the button, the button will be released. Similarly if it slides onto a button, the button will be pressed. This way it simulates a physical controller as closely as possible.

 

###Configurability
The classic controllers appearance and behavior can be configured using props (link). 

mode: ‘any-direction’/‘horisontal-vertical’/‘horisontal’/‘vertical’
color: grey


Example:
```javascript
phame.startAndShowModal({
  controllers: {
    'phame-classic':'https://www.phame.io/standard-controllers/classic/index.html'
  },
  initialControllers: {
    name: 'phame-classic',
    props: {
        mode: 'vertical'
    }
  }
});
```


And changing programmatically during the session:

```javascript 
phame.device.setProps({color: 'red'});
```

