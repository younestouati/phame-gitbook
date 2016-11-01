#Triggering events

The most basic way of triggering custom events is by using the method phameController.trigger()`. The method takes an `eventName` and some optional `eventData`. When you use this method, you are guaranteed that the event will be received by the main device (except if rate limiting reasons). Basically phame will make sure to transfer the event reliably over the network connection, and automatically retransmit in case it is lost on the way (think TCP (LINK)). You are also guaranteed that your events will be received in the order in which you triggered them.

```javascript
phameController.trigger('my-custom-event', {data: 42});
```

You will want to use this way of triggering events for any one-off events, button clicks etc.

###Triggering a sequence of events
When triggering a sequence of similar events, ```phameController.trigger()``` may not be the preferred option. Reliable, ordered data transfer, while convenient, suffers from the head-of-line blocking problem (LINK), which may occasionally slow down the data transfer an cause small peaks in the latency. When triggering a lot of similar events in quick succession you are often more interest in it working as fast as possible, than having guarantees that all of the events make it all the way to the main device. For this purpose `phameController` provides the method `phameController.triggerFastButUnreliably()`. It works exactly like `phameContorller.trigger()`, except it provides more stable and, slightly lower on average, latency, at the cost of not guaranteeing that all of the events are transferred (almost all of them typically will, though). Note, that even though the data transfer isn't reliable, you are still guaranteed that the events that are transferred, will be received in the same order they were sent in.

```javascript
phameController.triggerFastButUnreliably('my-high-frequency-event', {foo: 'bar'})
```
You would want to use `phameController.triggerFastButUnreliably()` whenever you are sending a sequence of similar events in quick succession, and when it isn't a problem if an event gets lost once in a while.

Note, that unreliable data transfer is not always supported (depends on browser LINK), but you can always use phameController.triggerFastButUnreliably. If unreliable data transfer isn't supported, it will automatically fall back to reliable data transfer (essentially use `phameController.trigger()`)

###Event Groups
A typical pattern when working with events is the start/main-action/end families of events, that represent a single, continuous gesture. One example would be if you move you finger across a touch device. This will result in a `touchstart` event, followed by a number of `touchmove` events, and finished of by a `touchend` (or `touchcancel` event). When creating these kinds of sequences, you would want to 

* make sure that the start event is reliably transferred
* make sure that the 'main' events (e.g. `touchmove`) are transferred as quickly as possible (using `phame.triggerFastButUnreliably()`)
* make sure that the 'end' or 'cancel' events are transferred reliably
* Make sure that all 'main' events are received after the the 'start' event and before the 'end' event or the 'cancel' event

To help you achieve that, `phameController` allows you to create an `event group`, like this:

```javascript
/*Creates a an event group named customevent. Used to fire the events
customeventstart, customevent, customeventend and customenvetcancel*/ 
const myEventGroup = phameController.createEventGroup('customevent');
```


You can then trigger events using the methods `triggerStart(data)`, `triggerMain(data)`, `triggerEnd(data)`, and `triggerCancel(data)`, and be sure that the criteria listed above are met. Note, that you must call `triggerStart()` before you trigger any of the other methods. Example:

```javascript
/* Will trigger the event customeventstart. Transferred reliably to master, 
   and carries the data {value: 1}*/
myEventGroup.triggerStart({value: 1});

/* Will periodically trigger event customevent. Transferred fast but unreliably to master, 
   and carrying the data {value: 2}*/
const interval = setInterval(() => myEventGroup.triggerMain({value: 2}), 50); 

/* Will trigger the event customeventend. Transferred reliably to main side with no data, in the this case
*/
setTimeout(() => myEventGroup.triggerEnd(), 1000);
```

Notice that we never stopped the interval, so the code will keep calling `triggerMain()` every 50ms. Disregarding the fact, that you would probably never trigger events periodically in a `setInterval()` like that in a real application in the first place, we should really have stopped the interval when calling `triggerEnd()`. All the main events will however be ignored, after the `triggerEnd()` function has been called - the `EventGroup` automatically takes care of that.

The main point with `eventGroups`, however, is that they allow you to 'mix' reliable and unreliable data transfer while still upholding certain guarantees about the events, such as the fact that the start event arrives first, and the end event arrives last.

You would want to use `EventGroups` whenever you have this pattern, where a gesture can be described by a 'start' event, and sequence of 'main' events, and an 'end' or a 'cancel' event to finish the sequence.


###Events Carrying Delta Values (i.e ensuring fixed event frequency)

Events can carry two kinds of data. Absolute data or relative data (delta values). Consider the `device-orientation` event. It carries a data object describing the current 3D orientation of your device as Euler angles, e.g: {alpha: 1, beta: 2, gamma: 3}. You could use these values directly to - say - control the orientation of an airplane in a game. The event carries complete and absolute information about the 3D orientation, and a single event is all you would need to determine the current orientation of the airplane - you wouldn't need to have any knowledge about any events that were transferred previously. Also, sending the same event twice in a row shouldn't change any thing, your application will just reapply the same orientation to the airplane.

Contrast this with the events triggered by the joystick of the classic standard controller. The joystick can be pulled to any side and triggers 'move' events carrying as data an object with the properties angle and magnitude. These indicate in which direction the joystick is pulled, and how 'hard'. These events are intended to be interpreted a 'deltas'; that is values relative to the current state. If used to control the position of a character on screen, they should be interpreted as 'relative to the characters current position, move it in the direction indicated by the angle property, by an amount proportional to the magnitude property'. In this case, we cannot tell what the current position of the character is, without knowledge about the prior position, which in turn depends on how many, and which order, move events have been triggered previously. 

When working with events carrying delta values, the frequency by which they are triggered becomes important. If the frequency by which the classic controller emits the move events doubles, the character will move at double speed. Also, if any event is delayed or never arrives, this has a permanent impact of the end state of the application (the character won't move as far as intended, as quickly as intended). No subsequent events will override the state and fix that. 

CONTINUE THIS!!





###Rate Limiting

TODO