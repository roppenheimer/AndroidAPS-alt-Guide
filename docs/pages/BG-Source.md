# BG sources

AndroidAPS needs a source of blood glucose information. This generally would be in the form on an app which links to your CGM device and sends the data to to AndroidAPS. There are a number of choices:

  * __xDrip+__ (most people's preferred option) can link to Dexcom G5 and G6 CGMs, the Freestyle Libre via a Bluetooth adapter such as MiaoMiao, the Medtronic 640G and the Medtrum A6. The quality of the data from these different devices varies and some of the advanced features of AndroidAPS can only be used with the "cleaner" CGM sources such as the Dexcom. xDrip+ also integrates well with Nightscout.
  * __Dexcom G5 app__ this is a modified version of the app provided by Dexcom which has been patched so that it will interface with AndroidAPS and pass the data on.
  * __Glimp__ is an app designed as an alternative reader for the Freestyle Libre using the NFC reader chip in an Android phone.
  * __Nightscout__ you can use BG values downloaded from Nightscout although this will obviously only work while you have a live internet connection to your Nightscout server. 





**For users of Dexcom:**<Br>
_If using xdrip…_<br>
* If not already set up then download [xDrip+](https://github.com/NightscoutFoundation/xDrip) and follow instructions on nightscout ([G4 without share](http://www.nightscout.info/wiki/welcome/nightscout-with-xdrip-wireless-bridge), [G4 share](http://www.nightscout.info/wiki/welcome/nightscout-with-xdrip-and-dexcom-share-wireless), [G5](http://www.nightscout.info/wiki/welcome/nightscout-with-xdrip-and-dexcom-share-wireless/xdrip-with-g5-support)).
* In xdrip go to Settings > Interapp Compatibility > Broadcast Data Locally and select ON.
* In xdrip go to Settings > Interapp Compatibility > Accept Treatments and select OFF.
* If you want to be able to use AndroidAPS to calibrate then in xdrip go to Settings > Interapp Compatibility > Accept Calibrations and select ON.  You may also want to review the options in Settings > Less Common Settings > Advanced Calibration Settings.
* Select xdrip in ConfigBuilder (setting in AndroidAPS).

_If using Dexcom G5 App..._<Br>
* Just dev.
* Download the apk from [here](https://github.com/dexcomapp/dexcomapp), just this versions works whether mg/dl or mmol/l.
* Deinstall original Dexcom app, if not allready done.
* Select Dexcom G5 App in ConfigBuilder

_If using OTG cable ('traditional' Nightscout)…_<br>
* If not already set up then download Nightscout Uploader app from the Play Store and follow instructions on [Nightscout](http://www.nightscout.info/wiki/welcome/basic-requirements).
* In AndroidAPS Preferences enter your nightscout website and API secret.
* Select NSClient in ConfigBuilder (setting in AndroidAPS).


**For users of Libre:**<br>

_If using xdrip..._<br>
* If not already set up then download xdrip and follow instructions on [LimiTTEer](https://github.com/JoernL/LimiTTer),  [Libre Alarm](https://github.com/pimpimmi/LibreAlarm/wiki) or [BlueReader](https://unendlichkeit.net/wordpress/?p=680&lang=en)([Hardware](https://bluetoolz.de/wordpress/)).
* In xdrip go to Settings > Interapp Compatibility > Broadcast Data Locally and select ON.
* In xdrip go to Settings > Interapp Compatibility > Accept Treatments and select OFF.
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
