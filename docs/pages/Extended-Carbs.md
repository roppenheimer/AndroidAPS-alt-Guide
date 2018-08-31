# Extended carbs / "eCarbs"

With a regular pump therapy, extended boluses are a good way to deal with fatty or otherwise slowly-absorbed meals which increase blood glucose for longer than the insulin is normally active. In a loop context, however, extended boluses don't make as much sense (and pose technical difficulties), since they're basically a fixed high temporary basal rate, which goes against how the loop works, which is adjusting the basal rate dynamically.
The need to deal with such meals still exists though. Which is why AndroidAPS as of version 2.0 supports so called __extended carbs__ or __eCarbs__.

In AndroidAPS extended carbs are simulated by issuing small increments of carbs every few minutes over the chosen duration. This has the effect of maintaining the COB figure at a suitably high level over the required time period. With this information, the loop can administer SMBs every few minutes, thereby simulating an extended bolus, but dynamically. (This should also work without SMBs, but is probably less effective because the scope for delivering extra insulin via TBRs is limited.)

To enter eCarbs, set a duration in the _Carbs_ dialog on the overview tab, the total carbs and optionally a time shift (this is a delay before the eCarb starts):

<img src="https://1.bp.blogspot.com/-gnWKSBIBO2g/WuTPV0Rya3I/AAAAAAAAAEg/BvqiZYrsuKcgbny5t1sHWlPS6feWq-xEwCLcBGAs/s1600/Screenshot_20180427-144305.png" width=400>


On the overview screen the future carbs are shown in brackets in the COB field and on the timeline you can see a series of small carb entries stretching into the future:

<img src="https://4.bp.blogspot.com/-sgc9XdUeaoQ/WuTPXxfaIuI/AAAAAAAAAEk/p7toa_aq_oIWWTnzoQFUPHt4JdPkaXrwwCLcBGAs/s1600/Screenshot_20180427-144324.png" width=400>

In the treatments tab the future carb entries are shown in dark orange. You also have the option to delete all entries that occur in the future if you need to do so:

<img src="https://user-images.githubusercontent.com/1732305/38613978-e6d1748e-3d8b-11e8-9d62-154fe73443da.png" width=400>



The recommended setup is to use the OpenAPS SMB APS plugin, with SMBs enabled as well as the _Enable SMB with COB_ preference enabled.

> __Example__ for a Pizza might be to give a (partial) bolus up front via the _calculator_ and then use the _carbs_ button to enter the remaining carbs for a duration of 4-6 hours, starting after 1 or 2 hours. Clearly you will need to experiment ot find what works best for you.


> __Hint:__ You can affect how aggressively the algorithm issues SMB corrections by adjusting the setting _"max minutes of basal to limit SMB to"_ to make the algorithm more or less responsive.

With low carb, high fat/protein meals it may be enough to only use eCarbs without manual boluses. [You can read a blog post about the eCarb function in action here](
 https://adriansloop.blogspot.co.at/2018/04/page-margin-0.html)
