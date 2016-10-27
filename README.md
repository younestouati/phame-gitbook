
# Key Concepts / Vocabulary






## Main Device

The main device the device running the web application that initiates the phame session. This will typically be a device with a large screen (smart tv, laptop, desktop, tablet), but could be any device that runs a modern browser. Typically the most action of your game/application will take place on the main device’s screen. For every Phame session there is exactly one main device.




## Controller Device



A controller device is either an iPhone, and Android Smartphone or (during development) and instance of the phame simulator, which runs the Phame App (app links). By default an arbitrary number of controller Devices can join a phame session, although it is possible to put restrictions on this number (both upper and lower limits). During a session each controller device is connect directly to main device, only. The controller devices do not interact directly with one another. 



Note that for controller devices are often referred to as simply devices, for short in documentation and in the names of the API methods. Whenever the term devices is used, if ALWAYS refers to the controller devices, never to the main device (which seldom needs to referred to explicitly. And when needed the term main device will be used)




## Primary (Controller) Device


The primary controller device (typically referred to as primary device for short) is the controller that currently is considered the ‘primary’ controller. This controller can be targeted from the main device in a particularly easy manner using the Phame API (through the phame.device property). In single controller device scenarios this gives you a convenient way of targeting the unambiguous controller device from your code running on the main device. In multi controller device scenarios you (the application developer) can chose to ignore the notion of a primary controller device altogether, or to assign special meaning/capabilities to this primary controller as appropriate in the context of the application.




## Controller



A controller (not to be confused with a controller device or device for short) is a web application running within a web view in the phame app/simulator on a controller device. During the cause of a phame session, many different controllers can run on the same controller device (one at a time). Phame offers a number of prebuilt controllers (link) and you can create your own custom controllers using the phameController API.




## Phame Session - maybe

TODO




## Device States



Upon joining a Phame session a controller device can be in one of three states: ‘present', ‘away', or ‘gone’, each of which is explained below:

present: Means that the device has joined the session and the the Phame app is currently in the foreground of the controller device and the screen is turned on.
away: Means that the device has joined the session, but the the Phame app is currently not running in the foreground of the controller device or the screen is turned off. The device is however still considered to be part of the session as the Phame app may come to back to the foreground at any time.
gone: The device has permanently been disconnected from the session, with no option of returning (The device can be connected to the session again, but at this point it would be considered a new controller device with a new session index).


STATE MACHINE DIAGRAM HERE!

-------------------->

|

|	 | ->          | ->

PRESENT     AWAY       GONE

     <—|









Random notes 2

