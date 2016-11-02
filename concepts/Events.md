#Phame Events

In Phame you can subscribe to events from two different sources: from the controller devices and from the global `phame` object. 



###Events trigger by the global phame object

The events from the global `phame` object are various events pertaining to the Phame session in general, e.g `'phase-session-ended'` which will be triggered - you guessed it - when a session ends. You would subscribe to it like this:


```javascript
phame.on(‘phame-session-end’, () => console.log(’The session has ended’));
```

The complete list of events triggered by the global `phame` object is:

...



Note that these events can only be listened for on the main device. That is, it is not possible to use the controller API to listen for these event, like so
```javascript
//WRONG - controllers cannot listen for these events
phameController.on('phame-session-ended', () => console.log(’This event will never be triggered...’));
```

SOMETHING ABOUT WHAT THE EVENT OBJECT LOOKS LIKE





###Events triggered by Controller Devices

NOGET. Go here to find a complete list of the built in controller device events. And go hear to learn how to trigger custom events from your own custom controllers.


####Listening for events

You can listen for controller device events from the main device as well as from you controller, like so:

```javascript
phame.device.on(‘phame-touchstart’, (e) => console.log(’The event: ‘, e)); //Listen (on the main device) for phame-touchstart event from the primary controller
```


```javascript
phameController.on(‘phame-touchstart’, (e) => console.log(’The event: ‘, e));//Listen (in custom controller) for phame-touchstart event
```


###The Event Object

All event objects, whether it is a built-in event or a custom events have the following properties: 



* `type`: The type eventType, e.g. 'phame-touchstart' or 'my-custom-event'

* `timestamp`: The timestamp for when the event was triggered.  this timestamp is 'approximated main device time'.

* `data`: The data. Will differ depending in the type of event.


Furthermore, when listening for an event main side, the event object will have have a `device` property, holding a reference to the controller device that triggered the event. E.g:

```javascript
phame.devices('all').solo('phame-touchstart', e => {
  //All devices, except from the first on to trigger phame-touchstart will vibrate for 500ms 
  phame.devices('all').except(e.device).vibrate(500);
});
```


Note that when using old fashioned function expressions (not arrow notation), the `this` keyword will be set to refer to the device that triggered the event. That is, the snippet above could also be written as:

```javascript
phame.devices('all').solo('phame-touchstart', function() => {
  //All devices, except from the first on to trigger phame-touchstart will vibrate for 500ms
  phame.devices('all').except(this).vibrate(500); 
});
```
However, when using es6 arrow notation, the `this` keyword will XXX, in which case you'll need to use the device property, as shown in the first example.

Finally, all custom events will have a modifier property, with the values of all the modifiers (LINK). Note that modifiers are currently not supported for built-in events. That is, for built-in events, the modifiers property is always `undefined`. If you need modifiers for built-in events, you can listen for the events in your custom controller, an 'retrigger' them as a custom event, which will automatically add the modifiers:

```javascript
phameController.addModifier('buttonAPressed', () => /* some logic to determine if button a is pressed (true/false)*/);

phameController.on('phame-touchstart', e => phameController.trigger('custom-phame-touchstart', e.data));
```

Now you can listen for the custom event main side, which will have the modifier data (if any):

```javascript
phame.device.on('custom-phame-touchstart', e => console.log('Is button a pressed?: ', e.modifiers.buttonAPressed));
```