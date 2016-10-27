#phame

phame is a global namespace from which the phame api methods and properties are accessed.



## Methods





### Session Handling

Only one session at a time // allows controller devices to connect // identified by a session id // auto reconnection is supported



### start(options) : promise

Starts a new phame session. Returns a promise that will resolve with the session id once the session has been created. Use the phame native apps or the phame simulator to connect controller devices to the session, using the session id.



If there is an ongoing session on the current domain, from a previous page load, the method will continue this session rather than starting a new one. This way all controller devices that have joined this session will remain connected (link to reconnection for more details).



### startAndShowModal(options) : promise

Like start() it start a new phame session. Will launch the phame connection modal ui once the session has been created. The UI explains to the user how to connect controller devices. The returned promise will resolve only when enough devices have connected (as defined by the the ‘minDevices' property of the options object, which defaults to 1).



Like start() the startAndShowModal method supports session reuse and thus provides auto reconnection.



### end()

Ends the current phame session. All connected controller devices will be disconnected.



### restore(options) : promise

Restores the ongoing session on the current domain, from a previous page load, if any. In other words, if there is an ongoing session, this method works exactly like the start() method. If there is no ongoing session, this method does nothing. Returns a promise that will behave like the start() methods return promise, in case there is an ongoing session. If there is no ongoing session the promise will be rejected with value ’no-session’.



//TODO: Write something about how it is convenient to call it at all times, and definitely during development 



### isSessionActive() : boolean

Return whether or not a phame session is active. Note, that an ongoing session from a previous page load is not active until start() or restore() has been called.



### getSessionId()

If there is an active session this method returns the session id (which is used to connect controller devices to the session). If there is currently no active session, null is returned.





## Device Handling

//Some blah blah



### device

A reference to the primary device. Allows you to interact with the primary controller device conveniently by phame.device.on(), phame.device.off() etc. Particularly convenient when there is only a single controller device connected. When multiple controllers devices are connected, you can still access the primary device this way, or you can chose to ignore the primary device concept all together and use other means (i.e. the .device() and devices() methods) to target the controller devices you wish to target. When multiple devices are connected, the primary device will be the controller device that joined the session first, unless another has been explicitly set as primary using the setPrimaryDevice() method.



### device(selector) : device

Returns the first of the controller devices that matches the given selector (See XXX for an explanation of what a selector is). The devices are sorted by the time at which they joined the session, meaning that if multiple devices match the selector, the one that joined the session first will be returned.  



### devices(selector) : deviceGroup

Returns the deviceGroup consisting of all the controller devices that match the given selector (see XXX for an explanation of what a selector is)



### setPrimaryDevice(device)

Sets the give device as the primary device. This means that device can be accessed directly using the phame.device property: e.g. phame.device.on(…). LINK

By default the primary device will always be the one - amongst those still part of the session (whether is present or away) that joined first. Use  setPrimaryDevice() to override this default.



## Event subscription

Will trigger some event pertaining to the session // Don’t confuse with the on/off methods for the device and deviceGroup objects



### on(eventName, eventHandler)

Registers an event handler for the given event name from the phame object. The phame object may trigger the following events:

* phame-session-started

* phame-session-ended

* phame-modal-opened

* phame-modal-closed

* phame-device-connected

* phame-device-disconnected (ADD THIS ONE??)

* phame-device-connection-restored (RENAME IN CODE!)

* phame-device-connection-denied

* phame-not-enough-devices

* phame-primary-device-set



### off

Unregisters the given event handler for the given event name. Note, that the given event handler must be a reference to the same function that was passed to the .on() method when registering the handler.



## Computer Vision (experimental)

//Experimental stuff // Some really cool and novel use cases // Harder to get to work properly



### setVisionTrackingFrame(domElement)

EXPLAIN LATER



###version (readonly)

```javascript

console.log(‘Currently running version ‘, + phame.version + ‘ of phame.js’);

```



Read only property holding the library version number







# device 

The device object represents a single controller device. Use this object to listen for events from this controller device or to send instructions/queries to it through remote procedure calls. If you wish to interact with more than one device at a time, see the documentation for deviceGroup (link)



See xxx for more information about how to obtain a device object.



## Methods



## Session Handling



### getState() : string

Will return a string indicating the device’s current device state (‘present’, ‘away’ or ‘gone’). See XXX for more information about device states.



### getAwayTime() : int

The number of seconds the device has been in the away state. If the device isn’t currently away, this method will return 0.



### getControllerName() : string

The name of the controller that is currently set to run on the controller device. The name is the key that was used when defining the controller in the controllers entry of the options object passed to `start()` or `startAndShowModal()` when initiating the session. (Show code example)



### disconnect() : void

Will disconnect the device (the device’s state will change to `gone`).



Example

phame.device.disconnect(); //Will disconnect the primary device



### sessionIndex

Readonly property (integer). A auto increment integer assigned to each controller device that connects to the session. Uniquely identifies the controller devices throughout the duration of the session (even after a restoration of a session). The same sessionIndex is never reused during a session. If a device disconnects from a session (device state ‘gone’) and then connects again, it will be assigned a new sessionIndex.



## Query Data

Query data is arbitrary data you can attach to a controller device and subsequently use when querying for (a) specific device(s). The query data is nothing more than a (possibly nested) object of key value pairs. No particular structure is enforced, but all values must be serializable (i.e. use strings, numbers, booleans etc.) 



See xxx for information about how to use the query data to query for specific controller devices.



Note that the controllers them themselves have no notion of what query data has been attached to them - this information is not propagated to the controller devices. Query data is a concept used by the main device alone to keep track of the controller devices.



### setQueryData(object)

Merges the given object into the current query data object (Object.assign style).



Example:

phame.device.setQueryData({a: 1, b: 2}); //Query data is now {a: 1, b:2}

phame.device.setQueryData({a: 3});	 //Query data is now {a: 3, b: 2}



### setQueryData(key, value)

Adds a property with the name key and the value value to the query data object, overwriting the property if it already exists.



Example

phame.device.setQueryData(a, 1); //Query data is now {a: 1}

phame.device.setQueryData(b, 2); //Query data is now {a: 1, b: 2}

phame.device.setQueryData(a, 3); //Query data is now {a: 3, b: 2}





### getQueryData(key)

Returns a clone of the value of the key property of the query data object. If no key is provided a clone of the entire query data object is returned. If the key property doesn’t exist, null is returned



Example

phame.device.setQueryData({a: 1, b: 2}); 

console.log(’The value of a: ‘, phame.device.getQueryData(‘a’)); //’The value of a: 1’

console.log(’The query data: ‘, phame.device.getQueryData()); //’The query data: {a: 1, b: 2}'

console.log(’The value of c: ‘, phame.device.getQueryData(‘c’)); //’The value of z: null'



### RemoveQueryData(key)

Removes the property key from the query data object. If no key is provided it removes all of the query data (resets if to an empty object {}). If the key property doesn’t exist, this method does nothing. //TODO: RETURN FALSE?



Example

phame.device.setQueryData({a: 1, b: 2, c: 3}); 

phame.device.removeQueryData(‘x’); //Nothing happens, x property does not exist

phame.device.removeQueryData(‘c’); //Query data is now {a: 1, b: 2}

phame.device.removeQueryData(); // Query data is now {}







## Remote Procedure Calls

// Method calls that run on the controller device // Abstracted as remote procedure calls with possible return values (as promises)



### setController(controllerName) : promise

Sets



### setProps() : promise





### ping() : promise

Pings the device. Returns a promise that resolves as soon as a reply has been received from the device. The promise resolves with an object with a ’time property’ holding the round trip time in milliseconds.



### vibrate(vibrationPattern) : promise

Will cause the controller device to vibrate according to the given vibration pattern. The vibration pattern matches that of the navigator.vibrate api (link). It may be a number (in which case it means the number of milliseconds the device should vibrate). It may also be an array of numbers, in which case...



<aside class=“warning”>

Gotcha: If phone is in silent mode, it may not be possible to make it vibrate.

</aside>



### getDeviceInfo() : promise

Retrieves an object describing the current state of the controller device, including the platform (iOS, Android or Simulator), current battery status and more. The method returns a promise that will resolve with device info object. EXAMPLE



do



## Event subscription

### on(eventName, eventHandler)

Registers an event handler for the given event name from the device. The eventName can be either the name of a built in event (link to full list) or a custom event (link to documentation about triggering events)



### off

Unregisters the given event handler for the given event name. Note, that the given event handler must be a reference to the same function that was passed to the .on() method when registering the handler.



### one(eventName, eventHandler)

Registers an event handler for the given event name from the device, which will unregister itself after being invoked once.



## Misc

### toString

Returns a string representation of the device (DESCRIBE WHAT IT LOOKS LIKE)





## Computer Vision (experimental)

### toMainCoordinates

### transformElement

### getDistance

### getPosition









#deviceGroup





##Iterating



###map(callback)

The map() method creates an array with the result of calling the provided function on every device in this deviceGroup



Example

const deviceStates = phame.devices(‘all’).map(device => device.getState());





###forEach(callback)

The forEach method executes a provided function once per device in the deviceGroup



Example

phame.devices(‘all’).forEach(device => device.vibrate(200)); //Note the same could be achieve by calling vibrate(200) on the deviceGroup itself, i.e. phame.devices(‘all’).vibrate(200)



###size()

Returns the number of devices in the deviceGroup



###and(selector)

Returns a new deviceGroup consisting of the devices in the current group plus the devices that match the given selector (link). Note that the current deviceGroup remains unaffected.



###except(selector)

Returns a new deviceGroup consisting of the devices in the current group except the devices that match the given selector (link). Note that the current deviceGroup remains unaffected.



###solo(eventName, evenHandler)

Registers an event handler for the given event name for every device in the device group. All the event handlers will all be removed as soon as the event is fired on one of the devices. In other works, the event handler will fire at most once (in total). Contrast this with one() where the event handler will fire at most once per device.



###live(eventName, eventHandler)

Registers an event handler for all devices which match the current selector now and in the future. Furthermore, the event handler will be be automatically removed from a device, if the device seizes to match the current device at some point.



Examples:

//This will register the event handler for all devices, including devices that will connect in the future

phame.devices(‘all’).live(‘phame-touchstart’, (e) => console.log(’Touch start event fired for device with session index ‘ + e.device.sessionIndex));



//Assuming 3 devices are connected with session indices 0, 1, and 2:

phame.device(0).setQueryData(“team”, “blue”);

phame.device(1).setQueryData(“team”, “blue”);

phame.device(2).setQueryData(“team”, “red”);



//This will register an event handler for devices with session indices 0 and 1

phame.devices({“team”: “blue”}).live(“phame-touchstart”, () => console.log(’Touch start fired by blue team member’));



//Updating device with session index 2’s query data to to team=blue. The event handler registered with live() above will automatically be applied to this device as well

phame.device(2).setQueryData(“team”, “blue”);



//Changing the team query data property to something other than blue, will automatically unregister the event handler registered with live() from the device

phame.device(0).setQueryData(“team”: “green”); //The event handler is automatically removed from this device



Note that live() also works on deviceGroups that are generated using the and() and except() methods. That is:



phame.devices({team: ‘blue’}).and({team: ‘red’}).live(’some-event’, someHandler);



…will register the handler for all devices with query data prop team set to either ‘blue’ or ‘red’ now and in the future. 



###die(eventName, eventHandler)

Removes event handler previously attached using live(). Note that the current selector must match the selector used previously when applying live(). For example, the following line attaches a live handler to the deviceGroup consisting of devices with sessionIndices 0 and 1:

phame.devices(0).and(1).live(“some-event”, () => console.log(‘Foor'))



Trying to remove this handler again, using a different selector (even if the deviceGroups consist of the same devices), will not have any effect:

phame.devices(1).and(0).die(“some-event”); //Will not remove the event handler! Selector is different!



The event handler can however be removed by using the exact same selector as when applying live():

phame.devices(0).and(1).die(“some-event”); //TODO: FIGURE OUT IF HANDLER IS NEEDED?



Also, when using a predicate function as (part of) the selector, a reference to the exact same function must be passed in when calling die, as was used when calling live() for it to work:



//WRONG - won’t work.

phame.devices((queryData, sessionIndex) => sessionIndex > 0).live(“some-event”, someHandler);

phame.devices((queryData, sessionIndex) => sessionIndex > 0).die(“some-event”); //Selector is a different function (although function body is identical)



//Right - will work

const allButFirst = (queryData, sessionIndex) => sessionIndex > 2;

phame.devices(allButFirst).live(“some-event”, someHandler);

phame.devices(allButFirst).die(“some-event”); //Will remove the event handler, because selector is a reference to the same function as was used with live()







# Build In Events

## Touch events

* phame-touchstart

* phame-touchmove

* phame-touchend



## Touch Gesture Events (require hammers)

* phame-tap

* phame-doubletap

* phame-press

* phame-pressup

* phame-pan

* phame-panstart

* phame-pan

* phame-press

* phame-press

* phame-press

*

*

*

*

*

*



## Device motion

* phame-devicemotion



## Device orientation

* phame-deviceorientation

* phame-top-up-start

* phame-top-up

* phase-top-up-end







Events

The event object
builtin events






#Selectors

When you have only a single controller device, your main device will interact with it through the phame.device object. When dealing with multiple controller devices, however, you need a way to express which controller device(s) you wish to target when setting up event listener, sending instructions etc. Selectors offer you a powerful way to control this.



Selectors are passed to the phame.device() and phame.devices() methods, which will return you a device object and a deviceGroup object, respectively. The selectors can take one of the following forms:



##Integer

If the selector is an integer it will match at most one device - the device whose session index (link) matches the selector. Since session indices uniquely identifies a controller device throughout a phame session, they often serve as convenient selectors.



Example:

phame.device(0); //Return the device with session index 0 (if it exists), or null

phame.devices(0); //Returns the deviceGroup for the group consisting of only the device with session index 0 (if it exists) or an empty deviceGroup



##String

Currently only a single string selector is supported; ‘all’ which matches all of the connected controller devices (both present and away link).



Examples:

phame.device(‘all’); //Will return the first device that matches the selector. Since every device matches the ‘all selector’, first device - that is the one with the lowest session index - is returned

phame.devices(‘all’); //Will return a deviceGroup consisting of all the controller devices in the session



##Object (key/value pairs)

Will be matched against the query data (link) attached to all devices. Each property from the given object is matched against the corresponding property from the devices’ query data objects (using ===). If they all match the device matches the selector. Note that if you need more advanced matching, such as matching nested properties or using < or > rather than ===, you can use a predicate function as explained below.



Examples (assuming 3 devices with session indices 0, 1, and 2 are connected):

phame.device(0).setQueryData({team: ‘blue', rank: ‘captain'});

phame.device(1).setQueryData({team: ‘red’, rank: ‘general’});

phame.device(2).setQueryData({team: ‘blue’, rank: ‘general'});



phame.devices({team: ‘blue’}); //Returns a deviceGroup with the devices with session indices 0 and 2 (both on the blue team)

phame.devices({team: ‘blue’, rank: ‘general’}); //Returns a deviceGroup with the device with session index 2 (general on the blue team)





##Predicate Function

Predicate function selectors allow you create implement custom selector logic. The function will be passed the query data and session index for each device and must return a boolean indicating whether or not the device matches the selector.



Example (assuming 3 devices with session indices 0, 1, and 2 are connected):

phame.device(0).setQueryData({team: ‘blue', points: 10});

phame.device(1).setQueryData({team: ‘red’, points: 4});

phame.device(2).setQueryData({team: ‘blue’, points: 7l});



phame.devices((queryData, sessionIndex) => queryData.points > 5); //Returns deviceGroup with the devices with session indices 0 and 2



##Device

A selector can be a device object. It makes little sense to use this selector type with the phame.device() method, as it would just return the device itself. It is however convenient if you wish to wrap a device in a deviceGroup or add it to/remove it from an existing deviceGroup using the deviceGroup’s and() and except() methods. 



Examples:

const myDevice = phame.device(0); //We assume this device with session index 0 exists



phame.devices(myDevice); //Returns a deviceGroup with the device with session index 0

phame.devices(‘all’).except(myDevice); //Returns a deviceGroup with all devices except device with session index 0




