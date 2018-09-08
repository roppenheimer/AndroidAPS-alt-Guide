# Sensitivity Detection and COB options

### Understanding how Autosense sensitivity works

Sensitivity is an advanced feature of OpenAPS which enables the loop to respond more or less aggressively depending on how blood glucose is responding to the insulin inputs. Generally it looks at the response over a period of time (hours) and responds accordingly.

Autosense only includes data points where there are no active carbs, and so part of the calculation is to determine when carbs are active so that these points can be excluded.

Sensitivity in this context is viewed from the point of view of the loop's response - if Autosense is less than 100% you are more sensitive to insulin than usual and the loop needs to be less aggressive than usual (<100%). If the figure is more than 100% then you are more resistant and the loop will react more strongly (>100%). To make the loop respond more strongly Autosense makes the ISF number smaller, increases your basals and also reduces your target BG all by the same factor. This causes the rig to deliver more insulin than usual. The reverse is also true, a sensitivity figure of less than than 100% makes the ISF number bigger, reduces the basals and raises the target BG.

__Example__
> Your sensitivity is shown as 70% meaning that you are more sensitive to insulin than usual. So your ISF which is normally 3.5 is now divided by 0.7 and becomes 5.0. This means that the loop expects more response from each unit of insulin than it normally would and so to achieve the desired effect it delivers a smaller dose by a factor of 0.7. Your BG target is also raised by the same factor. So if your normal target is 5.5 mmol/l this is now increased to 7.8mmol/l so that the loop will respond more gently.


In order to calculate this ratio Autosense looks at each BG data point for the last 24 hours and calculates the delta (actual observed change) over the last 5 minutes. It then compares it to “BGI” (blood glucose impact, which is how much *it expects* BG to be dropping based on insulin alone), and assesses the “deviations” (differences between the delta and BGI)

Each deviation is then classified as follows:

“x” : deviation is excluded because it is unexpectedly high - normally because of carb absorbtion. All deviations where COB > 0 are excluded until such time as the COB drops back to zero (carbs are fully absorbed) and deviations go negative once again. This is intended to eliminate the impact of rising BG due to carb absorption from sensitivity calculations and not falsely attribute it to insulin resistance. Deviations may also be excluded because of an unexpectedly high deviation (site failure, etc).

“+” : +ve deviation - BG was above what was expected (i.e. actual BG > estimated BG)

“-” : -ve deviation - BG was below what was expected. If a high temp target is running (i.e. activity mode), a negative deviation is also inserted every 5 minutes, this is to nudge the sensitivityRatio downward to reflect the increased sensitivity likely to result from physical activity.

“=” : neutral deviation - BG is doing what we expect. These neutral deviations are also inserted in every 2 hours to help decay the sensitivity ratio back to 100% in the event that all the deviation data in the calculation period has been excluded due to there being carbs on board or the deviations otherwise being unexpectedly high. (See above.)

Normally the sensitivityRatio == 100% - if it increases then temporary basal rates (TBR) will be increased and the Insulin Sensitivity Factor (ISF) is reduced to make the loop respond more aggressively.

Because times when there are carbs on board cannot be included in the sensitivity calculation we need a way of excluding those times. When all carbs have been absorbed BG data can once more be included into the sensitivity calculations.

There are four sensitivity detection modes which can be selected. Each has a different way of assessing sensitivity and deciding which data to exclude because carbs are present:

  * Sensitivity Oref0
  * Sensitivity AAPS
  * Sensitivity WeightedAverage
  * Sensitivity Oref1

### Sensitivity Oref0

This works as per the Oref0 model as described in [Oref0 documentation](https://openaps.readthedocs.io/en/latest/docs/Customize-Iterate/autosens.html#auto-sensitivity-mode-autosens). Basically sensitivity is calculated from the previous 24 hours worth of data. To exclude times where there are carbs on board a maximum life for carbs is set in the preferences and after that time all carbs are assumed to have been absorbed. The two settings here are:

  * min_5min_carbimpact - this is the assumed minimum impact that carb absorption has on BG in 5 minutes. This is effectively a safety setting and it allows carbs to "leak" down to zero over time so that the loop does not wrongly assume that there are still carbs present and respond accordingly.
  * Meal max absorption time (h) - this is the time by which all carbs are assumed to have been absorbed.

Upper and lower limits to the sensitivityRatio can also be set - (default 0.7 - 1.2)

#### Example - Oref0

> Oref0 - the assumed effect of unabsorbed carbs is truncated after a specified maximum time

![COB from oref0](../images/cob_oref0.png)

### Sensitivity AAPS

Sensitivity is calculated the same way as Oref0 but you can specify how far back in time the algorithm will look rather than having to accept the 24hr figure. The minimum rate of  carbs absorption is calculated from the maximum carbs absorption time specified in the preferences. So our two settings are:

  * Meal max absorption time (h)
  * Interval for autosense (h)
  
#### Example - Sensitivity AAPS

> Here the effect of COB is tapered down over time such that `COB == 0` after the specified Meal max absorption time

![COB from AAPS](../images/cob_aaps.png)

If the minimum carb absorption rate is used instead of the value calculated from deviations a green dot appears on the COB graph


### Sensitivity WeightedAverage

This method is the fastest way to track sensitivity changes.

Sensitivity is calculated based on a weighted average of recent deviations. Deviations are calculated as the difference between the actual BG and that predicted by the algorithm.  Newer deviations carry a greater weight in determining the sensitivity. The minimum rate of carb absorption is calculated from max carbs absorption time specified in the preferences. 


  * Meal max absorbtion time (h)
  * Interval for autosense (h)

You can read a fuller description here: [OpenAPS decision inputs](https://openaps.readthedocs.io/en/latest/docs/While%20You%20Wait%20For%20Gear/Understand-determine-basal.html#openaps-decision-inputs).

### Sensitivity Oref1
