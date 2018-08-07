# Installing your software 

To build your rig there are a number of pieces of software that you will need, both to install on your phone, as online resources and on your PC.

## On your phone.
For a typical rig you will need the following:

* **xDrip+** to collect the blood glucose data from your sensor and share it with Android APS. For most people this is the collector of choice although there are others you can choose from. You can download it from here: [https://jamorham.github.io/#xdrip-plus](https://jamorham.github.io/#xdrip-plus)

* **AndroidAPS** the app itself which takes your blood glucose readings, your inputs in terms of carbs, boluses and your settings and which then controls your pump. It comes as a source code that you have to compile yourself (more about that later) and you can download it from here: [https://github.com/MilosKozak/AndroidAPS](https://github.com/MilosKozak/AndroidAPS)

* **Ruffy** if you are using a Combo pump this piece of software essentially mimics the Combo bluetooth handset and sends commands to the pump. It also allows you to bluetooth pair the pump with your phone. Once installed it acts as a driver for AndroidAPS to control the pump. Once again, it comes as a source code that you have to compile yourself. You can get it from here: [https://github.com/MilosKozak/ruffy](https://github.com/MilosKozak/ruffy) (Be careful that you choose this version as there are other versons around that won't do what you want.)

* **LineageOS** this is a modified version of Android which you need if you are using a Combo pump in order to get the bluetooth to pair. (If you have a phone with Android 8 (Oreo) or newer you shouldn't need this.) There are LineageOS downloads for different phones and you can get them from here: [https://download.lineageos.org/](https://download.lineageos.org/). LineageOS is quite a well known modification for Android phones so if you don't want to tackle this yourself you'll probably find an independent phone repairer happy to do it for you.


## On your PC

* **Android Studio** you will need this to compile the source code for AndroidAPS and Ruffy into .apk files that you can install on your phone. You can get it from here: [https://developer.android.com/studio/](https://developer.android.com/studio/)

* **GitHub Desktop** not essential but useful. It enables you to maintain a local copy of the AndroidAPS and Ruffy source codes on your PC and keep track of any changes. You can get it from here: [https://desktop.github.com/](https://desktop.github.com/)

## In the Cloud

* **Nightscout** this provides a comprehensive way of gathering your diabetes management data together in one place, managing your profiles, generating reports and so on. It's not essential but it is hugely advantageous. You can find out more here: [http://www.nightscout.info/](http://www.nightscout.info/). You can either build your own Nightscout instance following the instructions on the website of you can use a hosted service like that at [https://www.ns.10be.de](https://www.ns.10be.de). There are also Nightscout apps available from the AppStore and Google Play which can be used to follow blood glucose and the rest. Useful for parents monitoring their children.

* **Autotune** as you get into using your rig you will need to tune your profile in terms of carb ratios, sensitivity and basal rates. You can do this by hand of course but Autotune is a very effective tool for crunching the numbers and making recommendations. You can either do your own implementation using the information at [https://openaps.readthedocs.io/en/latest/docs/Customize-Iterate/autotune.html](https://openaps.readthedocs.io/en/latest/docs/Customize-Iterate/autotune.html) or you can use a hosted service like that at [https://autotuneweb.azurewebsites.net/](https://autotuneweb.azurewebsites.net/)
