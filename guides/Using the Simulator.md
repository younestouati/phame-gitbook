#Using the Simulator

The [Phame Simulator](https://www.phame.io/simulator) is a web application that allows you to develop and test your applications/games and controllers, without having to use a smartphone. The simulator is able to load and run your controllers (LINK) the same way the native phame apps are. Using the simulator offers a number of advantages:

* You will be ready to develop multi device applications in a matter of seconds
* Allows you to easily develop and debug using the web development tools you are used to, including your browser dev tools, without having to connect your smartphone to your development machine or similar.
* Saves you from constantly having to switch between working on your development machine and picking up your smartphone and interact with that.
* Saves you the trouble of having to access localhost on your development machine from another device. If you run your controller (assuming it is served from localhost on your development machine) on your smartphone, you need to do port forwarding (link to chrome) or similar.
* Trivial to launch multiple devices to test multiplayer scenarios


Note however:

Certain features (link) of the native phame apps, are not supported by the simulator. In particular those related to use the camera and device motion events.
You would always want to test on physical devices before going to production.


With the exception of few unsupported features, the Phame simulator works the exact same way as the Phame Native apps (as a matter of fact, for the most part the simulator runs the exact same code as the Native apps which are primarily javascript based). That means, that just like the Native Apps, the simulator connects to the main device (link) over the Internet, and thus require network connectivity to run. Given that you would probably run the simulator on the same machine that runs the main application (although, technically you would\t have to), you will typically experience very low latencies (1-2ms). The latency between two different devices will typically be higher (link to, what about latency?).



###Browser Support

Currently the simulator works in Chrome and Firefox



Features





###Query Parameters

You can have the simulator auto type the sessionId if you pass it in through the query parameters sessionId when opening the application. You can for example use this to auto launched a simulator, with the session id pretyped, whenever you start a new session, which may be convenient during development. It would look something like this:




```javascript
//TODO: Remove this before going to production
phame.on('phame-session-started', window.open('https://www.phame.io/simulator?sessionId=' + phame.getSessionId() + ));
```


You can also have the simulator startup with multiple BLA BLA




Great for easy development and development of games that require many controllers
The same thing as the real devices, not cheating when communicating. Can fire it up on another device if you like.
Lower latency when on same device, though
Requires firefox or chrome
Allows you to use the web development tools that you know and love
Document the queryParams (sessionId and numOfSimulators)
Not all events can be simulated (device motion and vision related events)
Multitouch (requires changes to controller!)
Orientation change
Lock screen orientation
Reload button
Scale to adjust size
3D orientation, trigger device orientation events (keyboard to rotate)
Touch events
Use keyboard to enter session code. If multiple simulators, explain selection logic
Minimize app/reopen app to trigger away/gone
Vibration
Open phame menu button
Group actions (global toolbar)
Set number of simulators
Using the console (phameController.log) (A, B, C, D and colors)
Detecting that a device is in simulator (both from the controller and from main side)


