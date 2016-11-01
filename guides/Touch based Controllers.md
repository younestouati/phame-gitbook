
# Touch Based Controllers
Phame allows you to develop many different types of games including board games, Nintendo Wii-like games and traditional console games. When designing controllers for the latter use case, special care needs to be taken that the controller is usable even without looking at it. Phame controllers provide limited haptic feedback in that the user can't feel buttons rendered on the touch screen (although she can feel the edges/corners of the phone allowing her to orient herself without looking). Consequently, it requires a certain effort to design a controller that is a joy to use for this particular category of games. The first step is to realize that directly copying the layout used for physical controllers is unlikely to yield desirable results. The introduction of touchscreens for smartphones required new innovations regarding how to use the keyboard (better autocorrect, swiftkey, swype (links) etc), to compensate for some of its deficiencies compared to physical keyboards. Similarly, touchscreen-based game controllers (at least for games where we don't want the user to constantly look at the controllers) requires new designs and innovation to become truly user friendly. Below you'll find a number of suggestion as to how you can make you touchscreen based game controller a joy to use.

TODO: REFER TO THE CLASSIC CONTROLLER

### Limit the use of buttons
Given the limited haptic feedback of the touch screen, it is advisable to use as few buttons as possible. Buttons may be hard to hit without looking at the controller, and having too many increases the risk of hitting a wrong one. Furthermore, there are a limited number of good places to put the buttons, so if you use too many, you will have to put some of them in less than ideal places.

### Think about button size and placement
Make sure that the buttons you do use are large enough, and the distance between them is great enough, that they are easy to hit without looking at the controller. Also, placing the buttons near the edge of the phone - or better yet in the corners - will make it a lot easier for you user to hit them without looking.

### Develop for multitouch
Do not use simple HTML buttons with onClick event handlers as your buttons. Doing so will only allow for one button to be pressed at a time. Instead use the touch event api, to get all the current touch points (LINK TO SOME ARTICLE - HTML5ROCKS AND SIMILAR).

### Make use of the Touch Event level 2 Features for superior user experience
The touch event level 2 features (link) which is supported in XXX allows you to determine not just the touch point (as a single (x,y) coordinate) of each finger, but an ellipse estimation of the touch surface of each finger (link). This allows us to more precisely determine if the user is currently pressing a certain button, and makes it possible to press multiple buttons with a single touch. (Maybe add more explanation!)

### Consider using vibration for haptic feedback
You may want to consider using the vibrate API for haptic feedback, giving the user confirmation that he hit the button. Call `phameController.vibrate(100)` for a 100ms vibration.

### Consider using gestures instead of buttons
Imagine you are developing a football (soccer) game. You want to allow you user to shoot and spin the ball. With a traditional controller you would probably have her press one button to shoot, and another one (simultaneously or right after) to add spin. With a touchscreen controller you may consider using a button for shooting, but a swipe gesture (left or right) to add the spin. The swipe gesture, which can potentially be done anywhere on the controller, is easier to do, than hitting a button without looking.


### Consider using orientation and motion events in stead of buttons
In a similar vein, consider if intelligent use of the device orientation and device motion events (links) may be applicable in situations where you would otherwise use buttons


### Support arbitrary starting point for gestures
For gestures allow users to start it anywhere they like on the touchscreen (or at least anywhere within a large area). Interpret all subsequents touch events during that gesture in terms on their position relative to the initial touch point. This way the gesture becomes independent of its absolute coordinates and doesn’t require the user to hit any specific point.  The same goes if you implement knobs or joystick controls (link to the classic controller page knob section). Allow the user to start the gesture of puling the knob to any side from anywhere on or near the knob. Allowing the user to manipulate the knob even if she starts the gesture by touching next to it (e.g. missing it), makes it much easier to use it without looking at the controller.

##Other Possibilities

###Make the controller size adjustable
Allow users to scale the controller to better fit the size of their hands. A child may need the buttons to be closer to the each other to be able to reach them, than a grown up does.

###Make the controller layout fully adjustable
Allow the user to fully customize the controller layout to fit her preferences left/right handedness etc.

###Make the controller adapt automatically
For example: if you detect that the user keeps hitting right next to a button, why not automatically move it or make it bigger?

###Make the adaptability of the controller a feature of your game
Allow users to obtain/unlock new controllers/controller layouts as they progress in your game. Or change the difficulty by changing the controller or adding/removing features.

