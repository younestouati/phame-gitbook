#Development Workflow
Developing phame applications is a little different from developing 'normal' applications, in that the app is effectively spread across multiple devices. Technically these are of course separate web apps that talk to each other, but the intention with phame is to create a platform that enables apps that are so integrated and seamless, that they feel like distributed, multi device games and applications, to the end user as well as to the developer.

This guide explains how the phame platform aims to provide a development experience that is as close to what you are used to from single device development as possible. Furthermore you find tips and pointers to a recommended development workflow. OMFORMULER!



###Remember it is just web applications 
Initially it is important to remember, that at its core, phame is web based platform. Everything you - as the game/application developer - do, is done using standard web technology (html, css, and javascript). The master library is meant for web applications running in the browser, where as the controller library is used to develop web applications that will run inside a web view in the phame native apps for Android and iOS (and inside the simulator during development). Both master application and controllers are regular single page web applications that can be developed independently using your preferred frameworks and libraries. They both have full access to the internet during the session, and can load stuff runtime (whether additional code, youtube videos, up-to-date quiz questions or what not). RECOMMEND. DEVELOP INDEPENDENTLY FIRST, USUAL SETUP AND TECHNOLOGY. TALK ABOUT USING FAVORITE BROWSER ETC.

###Use the simulator
Developing your master application and your controllers independently will only take you so far. At some point they will have to talk to each other (the controllers will send events and the master application invoke remote procedure calls).  MORE HERE

* Saves you the trouble of dealing with several physical devices at the same time
* You can continue to develop, inspect, test and debug in the browser using the developer tools
* Easy to fire up a new device. Particularly convenient when develop multiplayer games, where you would otherwise need multiple smartphone to test properly
* The simulator auto types session id for you, if open with proper query parameter, e.g.: "?sessionId=123456789”

Note, however, that not all events can be triggered by the simulator. Specifically if you make use of `devicemotion` events you will need to use a real device.

###Use `phameController.log()`
The `phameController` library provides a log message. It is a wrapper around the console.log that provides the added feature of prepending each message with a colored device identifier making it easy to spot which controller is pertains to, when multiple simulators are running in the same window (thus sharing the same console). 

###Use `phame.restore()`
You'll likely start a new session (`phame.start()` or `phame.startAndShowModal()`) as a response to some user action, like click a button, rather than as soon as your web application loads. If this is true, it is a good idea to call `phame.restore()` when you web application loads, instead. This way, if there is an ongoing session from a previous load of your application (in the same browser tab), this session will be restored, and the connected controller devices will remain connected. This is very convenient during development, where you will likely reload you app frequently as you update the code, as it saves you the trouble of constantly reconnecting your controller devices. It is also convenient for your end users, as they may accidentally reload your application or navigate away.

Note, if you already call `phame.start()` or `phame.startAndShowModal()` when you application loads, there is no point in calling restore(), as the start methods will automatically restore an ongoing session, if any, rather than starting a new one.


REMEMBER TO PRESERVE STATE YOURSELF!!

REMEMBER TO CATCH FAILED PROMISE


###Use `phame.device.setProps()` to transfer state to the controllers
As explained in the guide about dealing with state (link), the recommended way of transferring state from the master application to the controllers, is through the built-in remote procedure call `setProps()`. Props are managed by phame, and will automatically be (re)transferred to controller device whenever it returns (the user reopens the phame app). It turns out that this is very convenient during development as well, as it ensures that the controller can be reloaded (manually or by live reload or similar) and still have the newest state.


###Test on real devices
Before releasing anything, you will always want to test your application/game using real devices (preferable multiple Android and iOS devices) to make sure you catch any platform specific peculiarities and test on different screen sizes. You may also want to test under different network conditions (on WiFi, 3G etc).



WARNING: At the time of writing Chrome’s developer tools’ throttling features do not work for data transferred using web sockets and webRTC data channel, so it won’t be of any use in a phame context.