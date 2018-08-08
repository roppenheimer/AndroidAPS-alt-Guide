The AndroidAPS alt-Guide
==============================================

*An unofficial guide to AndroidAPS: the Artificial Panchreas system for Android*

If you're reading this you are almost certainly familiar with insulin pumps and continuous glucose monitors (CGMs) and you will have asked the very obvious question: "Why can't these talk to each other and manage my blood glucose?"

Well, they can. AndroidAPS provides a way of taking the data from a Bluetooth enabled CGM, doing the necessary processing and then controlling a Bluetooth enabled insulin pump to emulate what a panchreas would do. It's not a plug an play solution, it can never do as well as a natural pancreas would but with careful setting up and usage it can deliver results that are not far behind.

A natural panchreas can react very quickly to changes in blood glucose, and the insulin that it puts directly into the blood stream takes effect in minutes. By comparison CGM readings are typically be about 10-15 minutes behind, insulin infused into the subcutaneous tissue takes around an hour to reach full effect and the infused insulin takes a long time to die away compared to that from a natural panchreas.

Therefore an artificial panchreas system is to a large extent dependent on being able to make predictions of what your blood glucose is likely to do in the next few hours. It does this using much the same type of calculations as you would normally when working out your dosages, but crucially, it updates these forecasts every five minutes when it gets a fresh reading from your CGM. You still need to tell it about carbs you are about to eat, about if you are about to exercise and so on. You also need to configure it carefully with your basal profile, your various ratios and test them carefully. If you make the effort to understand all these things you can reasonably expect to see blood sugar results which approach those that a non-diabetic person would see.

**About AndroidAPS**

AndroidAPS is an app that runs a version of `OpenAPS "oref0" and "oref1" algorithm <https://openaps.readthedocs.io/en/latest/>`_ and can communicate with bluetooth-enabled insulin pumps and CGMs. OpenAPS is an algorithm that has been developed independently of any pump or CGM manufacturer and whose underlying premise is that any manufacturer's CGM and any manufacturer's pump, given a suitable interface, could be used to build an artificial panchreas. You can read more about how the OpenAPS model works at the `OpenAPS Reference Design <https://openaps.org/reference-design/>`_


Before  you start you should read your way through the information on this site and link up with some of the social media groups where you will be able to discuss with other people using AndroidAPS and ask for help if you need it.

**Tell us you are looping**

Once you have got your loop working, please let us know by `filling in the online form <pages/tell_us_looping>`_. You can find out more `here <pages/tell_us_looping>`_

.. note:: 
	**Disclaimer And Warning**

	* All information, thought, and code described here is intended for informational and educational purposes only. Nightscout currently makes no attempt at HIPAA privacy compliance. Use Nightscout and AndroidAPS at your own risk, and do not use the information or code to make medical decisions.

	* Use of code from github.com is without warranty or formal support of any kind. Please review this repository's LICENSE for details.

	* All product and company names, trademarks, servicemarks, registered trademarks, and registered servicemarks are the property of their respective holders. Their use is for information purposes and does not imply any affiliation with or endorsement by them.

	Please note - this project has no association with and is not endorsed by: `SOOIL <http://www.sooil.com/eng/>`_, `Dexcom <http://www.dexcom.com/>`_, `Accu-Chek, Roche Diabetes Care <http://www.accu-chek.com/>`_.


.. toctree::
   :maxdepth: 3
   :glob:
   
   Before-you-start
   Understanding_AndroidAPS
   Designing_Your_Rig
   Nightscout
   AndroidAPSsoftware
   Configuration
   Troubleshooting
   Where-To-Go-For-Help