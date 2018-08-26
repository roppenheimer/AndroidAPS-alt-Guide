# Autotune

Autotune is an application which will read the data that has accumulated in your Nightscout server and help you to analyse it, giving you suggestions as to how to adjust your basal rates, your sensitivity and your carb ratios. As with Nightscout you can either [set up your own](https://openaps.readthedocs.io/en/latest/docs/Customize-Iterate/autotune.html) using the freely available code or you can use a [ready built online service](https://autotuneweb.azurewebsites.net/)

> As Nightscout is the only source of data for Autotune, it's important that the data in Nightscout is complete. If you don't log all of your carb intake for example, Autotune is likely to produce misleading results.

## Profiles & Basals

Autotune does not use profile switch events from Nightscout. It assumes that the profile given as part of the input to Autotune was used for the entire period of data being considered.

If you are using a closed loop system this shouldn't be a problem, as most of the time you will have a temp basal rate running so even if the base profile basal rate was changed, the temp basal records in Nightscout will record what basal rate was running at the time. However, if you are running for most of the time using your standard profile basal, you may not get accurate records if your profile has changed significantly over the time period you are running Autotune over. You should therefore restrict Autotune to running only over a time period for which you were using the same basal profile.

Another point to note is how temp basal records are recorded in Nightscout. If you record temp basals as a percentage of the profile basal, those records will not be used by Autotune and it will assume that the profile basal was in force at the time. Your temp basal records should either be recorded as absolute basal values, or be percentages but with the additional `rate` property specified as part of the record as well.

## Autotune Process

Autotune will start with the profile that you give it, then run the following process on each day's worth of data in turn. The results from each day are used as the starting point for autotuning the next day's data, and so on. The results from the final day are given as the ultimate recommendation.

## Categorization

Autotune takes the BG entries from Nightscout, along with carbs and insulin (boluses and temp basal records). It looks at the difference between each consecutive BG entry and uses the associated carbs and insulin data to determine why the change between the two BG entries was what it was.

Each "deviation" (difference between two consecutive BG readings) is allocated to *one* of several different contributing factors:

* **CSF** - if there are COB, or while the deviations stay positive (BG keeps rising) after COB reaches 0, those deviations are logged against the carb sensitivity factor (CSF)
* **UAM** - if there is more IOB than the current hourly basal rate, or the deviation was more than 6 mg/dL, those deviations are logged against unannounced meals (UAM)
* **basal** - if the expected impact on BG of basal insulin is 4 or more times that of the net IOB, or the BG is rising, those deviations are logged against basals
* **ISF** - if the BG is falling and the the expected impact on BG of the net IOB is at least a quarter of the basal insulin, those deviations are logged against the insulin sensitivity factory (ISF)

After completing this process, some of the deviations will be moved to the other categories:

* If the `--categorize-uam-as-basal` option was specified on the Autotune command line, all the UAM deviations will be moved to the basal category. This is a useful option to specify if you have definitely entered all carbs, including rescue carbs, into Nightscout
* If there are more than twice as many deviations classified as UAM than basal, the lowest 50% of the UAM deviations will be moved to the basal category and the highest 50% to the ISF category. This is based on the assumption that the basal & ISF settings are too far out, causing large deviations that have been incorrectly classified as UAM
* If there were fewer than 10 ISF deviations, and more than 4 times as many CSF deviations than basal deviations, all the CSF deviations will be moved to the ISF category. This is based on the assumption that the carb absorption calculations are out

## Autotuning Carb Ratio

The carb ratio (CR) is autotuned by looking at the starting and ending BG for each meal (BG when carbs were entered and BG when COB reached zero and IOB was less than half the hourly basal rate). The CR is then calculated based on the IOB that was already present at the start of the meal and the insulin delivered during the absoption of the meal, plus/minus any additional insulin required to bring the ending BG back to the same level as the starting BG.

This is done for each meal and then averaged over the day to produce the suggested CR. Only 20% of the change to the CR is used in the output so it doesn't produce fast-changing recommendations.

## Autotuning Basals

The total basal deviations in each hour are summed, then the current ISF is applied to work out how much insulin would be needed to counteract those deviations. Only 20% of this change in insulin is considered for the later calculations so it doesn't produce fast-changing recommendations.

If more insulin is needed, the basal rates for the previous 3 hours are each increased by 1/3 of the required extra insulin.

If less insulin is needed, the basal rates for the previous 3 hours are reduced in proportion to remove the calculated amount of insulin.

Finally, some hours will not have any deviations in the basal category, e.g. because all the deviations were allocated to carb absorption. In these cases, the basal recommendation will be based on the first hour before and after that were adjusted, taking 80% of the current basal setting for this hour plus 10% of each adjacent autotuned hour.

> Note that this last point means that hours that are routinely dominated by carb absorption will over time have their basal rates set to an average of the surrounding hours. If you do have an underlying need for higher or lower basal rates than normal at these times as well, Autotune will combine these changes with your CSF. While those changes may work for these times, it can lead to a dangerously high carb ratio for other times.

## Autotuning ISF

The deviations allocated to ISF are compared to the expected deviation. These ratios between actual and expected deviations are sorted and the median one is taken. The new ISF is then calculated by multiplying this median ratio by the current ISF. Only 20% of the change to the ISF is used in the output so it doesn't produce fast-changing recommendations.

## Safety Limits

A minimum and maximum ratio for all changes is included as part of the Autotune input. The suggested settings for these are -30% and +20%. Any suggested changes will be limited to this range, including CR, ISF and basals.