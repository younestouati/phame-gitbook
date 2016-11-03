#Controller API
FIRST AN INTRO TO HOW YOU START IT (INCLUDE THE LIBRARY EITHER WITH SCRIPT TAG OR NPM INSTALL)


###getProps() : Object

Returns a clone of the props object. 


The props is data transferred from Master (either as initialProps when the controller is first loaded, or subsequently using the device.setProps() LINK method). //LINK DEALING WITH STATE GUIDE (WHY YOU SHOULD USE PROPS)

Note that the event ‘phame-props-changed’ will be triggered each time the props change.

```javascript
/* Assuming Master has transferred the prop “counterValue” like so: phame.device.setProps({counterValue: 3}); */
phameController.on('phame-props-change', () => {
  console.log('The counter value is: ' + phameController.getProps().counterValue);
});
```

### vibrate(vibrationPattern) : promise
Will cause the controller device to vibrate according to the given vibration pattern. The vibration pattern matches that of the navigator.vibrate api (link). It may be a number (in which case it means the number of milliseconds the device should vibrate). It may also be an array of numbers, in which case...

```javascript
//Vibrate for 2 sec, pause 1 sec, vibrate 2 sec
phameController.vibrate([2000,1000,2000]);
```

<aside class=“warning”>
Gotcha: If phone is in do not disturb mode, it may not be possible to make it vibrate.
</aside>

### on(eventName, eventHandler)
Registers an event handler for the given eventName. The eventName can be either the name of a built in event (link to full list) or a custom event (link to documentation about triggering events)

```javascript
const handler = () => console.log('Touch started');
phameController.on('phame-touchstart', handler);
```

Info: Just like Master can listen for events triggered by a controller, the controller itself can listen to these events. 

### off
Unregisters the given event handler for the given event name. Note, that the given event handler must be a reference to the same function that was passed to the `.on()` method when registering the handler.

```javascript
phameController.off('phame-touchstart', handler);
```

###log()
A wrapper around the browser's `console.log()` method. Works just like `console.log()` when running directly in the browser. When the controller runs inside the simulator it will prepend each log statement with a colored identifier message that identifies which simulator the log originated from. Useful when multiple simulators run in the same browser window thus sharing the console.

IMAGE HERE!

###trigger(eventName, eventData)
Triggers a custom event with the given `eventName` and the given `eventData`. Events are guaranteed to arrive at `Master` (retransmits if necessary).

```javascript
phameController.trigger('my-custom-event', {myData: 42});
```

###triggerFastButUnreliably(eventName, eventData) 

Works exactly like trigger, except it does not retransmit. CONTINUE

```javascript
phameController.trigger('my-custom-event', {myData: 42});
```



###getTime() : Number

Returns the estimated current time on `Master` in milliseconds since `1970/01/01`. LINK TO GUIDE ABOUT TIME

```javascript
console.log('Estimated time on master: ' + phameController.getTime());

```

###disconnect()
Disconnects the controller device (will leave the session permanently). On `Master` this device's state will change to `gone`.

```javascript
phameController.disconnect();
```

###addModifier(modifierName, callback)

Adds a callback method that will be invoked **each** time a custom event is triggered by the controller. The return value of the callback will be set as value for the `modifierName` property of the modifier object that is NOGET on every custom event. Note that modifiers are not supported for built-in events.

```javascript
const mainElement = document.getElementById('main-element');
let isMainElementPressed= false;
mainElement.addEventListener('mousedown', () => isMainElementPressed = true);
mainElement.addEventListener('mouseup', () => isMainElementPressed = false);
mainElement.addEventListener('mouseout', () => isMainElementPressed = false);

phameController.addModifier('mainElementPressed', () => isMainElementPressed)’
```

###enableHammerJS()
TODO

###createEventGroup(eventName)

Returns a new `eventGroup` object with the methods `triggerStart(data)`, `triggerMain(data)`, `triggerEnd(data)`, and `triggerCancel(data)`. These will trigger the events with the following names, respectively: `[eventName]start`, `[eventName]main`, `[eventName]end`, and `[eventName]cancel`. See XXX for more details about how and when you would use an `eventGroup` to trigger events.



###lockOrientationInPortrait()

Locks the controller orientation to portrait.

```javascript
phameController.lockOrientationInPortrait();
```

###lockOrientationInLandscape()

Locks the controller orientation to landscape.
```javascript
phameController.lockOrientationInLandscape();
```

###unlockOrientation()

Unlocks the orientation so that it respect the user settings. If the user has locked the orientation of her smartphone, this will be respected. If not, the orientation will

change as the user tilts her smartphone. This is the default controller state, so it only makes sense to call this method if `phameController.lockOrientationInPortrait()` or `phameController.lockOrientationInLandscape()` has been called previously.

```javascript
phameController.unlockOrientation();
```

###version (readonly)
Read only property holding the library version number

```javascript
console.log('Currently running version ', + phameController.version + ' of phameController.js');
```



#Dealing with state

If your controllers are stateless, state handling in a phame application is no different from state handling in any other application. All state will reside on Master, and the controllers are just ‘dumb’ devices that emit events, but don’t change depending on the state of the application.



Often, however, you will want you controllers to change depending on the current application state. In a card game, for example, you’ll want the controller to reflect the players current hand. This can get a little tricky, if you aren’t careful, as you may end up with application state spread across different devices, making it hard to keep it consistent and in sync. Phame does not strictly enforce any particular way of handling state, but strongly encourages an approach where all state is owned and controlled by Master and - on a need to know basis - transferred as read only properties to the controllers, thus ensuring a unidirectional data flow.



###Recommended approach

All state on Master (single state tree as redux, or however you like)
Only master can update the state (follows directly from all state residing on it)
Master will transfer subpart of state to relevant controllers, when needed (see below for how this is done)
While controllers can’t manipulate the application state directly, they can impact it indirectly by emitting events. It is however up the Master application that determines which events to listen for, and how to update the state in response to those event. This approach makes it much easier to reason about the application, since only a single actor has the ability to update the state.


This recommended approach is reflected in the asynchronous nature of the Phame APIs, where controllers only means of communication to Master is through events, whereas Master communicates to the controller through direct function invokations (remote procedure calls). This underscores the idea that master is in control, and can impact the controllers directly and determines which of the controller events it wishes to subscribe to.



##Transfering state to the controllers

CUSTOM METHOD VS SETPROPS!



##Exception



DRAWING HERE!



Easy when controller is dumb (just emits events). No different than any other web app. Deal with state as you usually do
When controllers aren’t stateless, it is more tricky, as you state is essentially distributed across different devices
Phame strongly encourages to keep the entire state on Master, and only allow Master to manipulate the state. Controller Devices gets subpart of state that is relevant, as read only properties. Controllers can only impact state indirectly (send events to master). Master determines how to update the state based on these events.
HAVE DRAWING HERE. EVENTS GO ONE WAY, PARTIAL (READONLY) STATE GOES THE OTHER WAY
Only exception is when controllers have local state, that isn’t relevant for the application as a whole (such as the sort order of a number of cards)
Two ways to transfer state, setProps (recommended) or custom remote procedure calls


#HAMMERJS

What is hammerjs
Link to tutorials about how you can use
Automatic integration in phame
enableHammerJS to do it manually


#Dealing with time

Typically latency is low enough that you don’t need to care about it
How clock offset is determined
Using getTime in controller
How this timestamp is used for events






###getProps()
###log()
###off()
###on()
###vibrate()
###triggerFastButUnreliably() 
###trigger()
###getTime()
###disconnect()
###addModifier()
###enableHammerJS()
###createEventGroup()
###lockOrientationInPortrait()
###lockOrientationInLandscape()
###unlockOrientation()
###version