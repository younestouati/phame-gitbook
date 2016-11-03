#Dealing with state
If your controllers are stateless, state handling in a Phame application is no different from state handling in any other application. All state will reside on Master, and the controllers are just 'dumb' devices that emit events, but don't change depending on the state of the application.

Often, however, you will want you controllers to change depending on the current application state. In a card game, for example, you'll want the controller to reflect the players current hand. This can get a little tricky, if you aren't careful, as you may end up with application state spread across different devices, making it hard to keep it consistent and in sync. Phame does not strictly enforce any particular way of handling state, but strongly encourages an approach where all state is owned and controlled by `Master` and - on a need to know basis - transferred as read only properties to the controllers, thus ensuring a unidirectional data flow.



###Recommended approach

* All state on Master (single state tree as `redux`, or however you like)
* Only master can update the state (follows directly from all state residing on it)
* Master will transfer subpart of state to relevant controllers, when needed (see below for how this is done)
* While controllers can't manipulate the application state directly, they can impact it indirectly by emitting events. It is however up the `Master` application that determines which events to listen for, and how to update the state in response to those event. This approach makes it much easier to reason about the application, since only a single actor has the ability to update the state.


This recommended approach is reflected in the asynchronous nature of the Phame APIs, where controllers only means of communication to Master is through events, whereas `Master` communicates to the controller through direct function invokations (remote procedure calls). This underscores the idea that master is in control, and can impact the controllers directly and determines which of the controller events it wishes to subscribe to.



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