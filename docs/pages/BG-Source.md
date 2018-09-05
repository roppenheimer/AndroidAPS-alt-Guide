# BG sources

AndroidAPS needs a source of blood glucose information. This generally would be in the form of an app which links to your CGM device and sends the data to to AndroidAPS. There are a number of choices, but bear in mind that the quality of your BG will significantly affect how well the loop performs:

  * __xDrip+__ (most people's preferred option) can link to Dexcom G5 and G6 CGMs, the Freestyle Libre via a Bluetooth adapter such as MiaoMiao, the Medtronic 640G and the Medtrum A6. The quality of the data from these different devices varies and some of the advanced features of AndroidAPS can only be used with the "cleaner" CGM sources such as the Dexcom. xDrip+ also integrates well with Nightscout.
  * __Dexcom G5 app__ this is a modified version of the app provided by Dexcom which has been patched so that it will interface with AndroidAPS and pass the data on.
  * __Glimp__ is an app designed as an alternative reader for the Freestyle Libre using the NFC reader chip in an Android phone.
  * __Nightscout__ you can use BG values downloaded from Nightscout although this will obviously only work while you have a live internet connection to your Nightscout server. 



## Setting up your Blood Glucose source ##

**Dexcom users:**<Br>
_If you are using xDrip+…_<br>
* Download [xDrip+](https://jamorham.github.io/#xdrip-plus) if you have not already done so and follow the instructions on Nightscout ([G4 without share](http://www.nightscout.info/wiki/welcome/nightscout-with-xdrip-wireless-bridge), [G4 share](http://www.nightscout.info/wiki/welcome/nightscout-with-xdrip-and-dexcom-share-wireless), [G5](http://www.nightscout.info/wiki/welcome/nightscout-with-xdrip-and-dexcom-share-wireless/xdrip-with-g5-support)). The Dexcom G6 works basically the same as the G5 but you have to tick the box to say you are using a G6 sensor.
* So that xDrip+ will transmit its readings to AndroidAPS you need to go to __Settings > Inter-app settings > Broadcast Locally__ and select __ON__.
* Whilst you are in this section __Accept Treatments__ will cause xDrip+ to display and boluses that AndroidAPS issues. Normally you can select OFF.
* If you want to be able to enter calibratons via AndroidAPS then __Accept Calibrations__ needs to be ON.  You may also want to review the options in Settings > Less Common Settings > Advanced Calibration Settings.
* In AndroidAPS select xDrip+ as your BG source in the ConfigBuilder.

_If you are using the patched Dexcom G5 app..._<Br>

* If you have the original Dexcom app on your phone you will need to uninstall it.
* Download the .apk file from [here](https://github.com/dexcomapp/dexcomapp). This version works whether you are using mg/dl or mmol/l.
* Select Dexcom G5 App in ConfigBuilder

_If you are using a G4 with a USB OTG (On-The-Go) cable and Nightscout uploader ('traditional' Nightscout)…_<br>

This option relies on the Nightscout uploader loading your BG values to the Nightscout server and then configuring AndroidAPS to draw its BG data from Nightscout. Clearly for this to work requires a live internet connection.
* Download and set up the Nightscout Uploader app from the Play Store and follow instructions on [Nightscout](http://www.nightscout.info/wiki/welcome/basic-requirements).
* In the AndroidAPS Preferences enter your Nightscout website URL and API secret.
* In AndroidAPS select NSClient as the BG source.


**For Libre users:**<br>

_If using xDrip+..._<br>
* If not already set up then download xdrip and follow instructions on [LimiTTEer](https://github.com/JoernL/LimiTTer),  [Libre Alarm](https://github.com/pimpimmi/LibreAlarm/wiki) or [BlueReader](https://unendlichkeit.net/wordpress/?p=680&lang=en)([Hardware](https://bluetoolz.de/wordpress/)).
* In xdrip go to Settings > Interapp Compatibility > Broadcast Data Locally and select ON.
* In xDrip+ go to Settings > Interapp Compatibility > Accept Treatments and select OFF.
* If you want to be able to use AndroidAPS to calibrate then in xdrip go to Settings > Interapp Compatibility > Accept Calibrations and select ON.  You may also want to review the options in Settings > Less Common Settings > Advanced Calibration Settings.
* Select xdrip in ConfigBuilder (setting in AndroidAPS).

_If using Glimp..._<br>
* If not already set up then download Glimp and follow instructions on [nightscout](http://www.nightscout.info/wiki/welcome/nightscout-for-libre).
* Select Glimp in ConfigBuilder (setting in AndroidAPS).

**For users of MM640g or MM630g:**<br>
* If not already set up then download [600 Series Android Uploader](http://pazaan.github.io/600SeriesAndroidUploader/) and follow instructions on [Nightscout](http://www.nightscout.info/wiki/welcome/nightscout-and-medtronic-640g).
* In 600 Series Uploader go to Settings > Send to xdrip+ and select ON (tick).
* Select MM640g in ConfigBuilder (setting in AndroidAPS).


**For users of other CGM uploaded to nightscout:**<br>
If you have any other CGM set up that sends your data to [Nightscout](http://www.nightscout.info) then<br>
* In AndroidAPS Preferences enter your nightscout website and API secret.
* Select NSClient in ConfigBuilder (setting in AndroidAPS).
