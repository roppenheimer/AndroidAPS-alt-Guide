Autotune
========

Autotune is an application which will read the data that has accumulated in your Nightscout server and help you to analyse it, giving you suggestions as to how to adjust your basal rates, your sensitivity and your carb ratios. As with Nightscout you can either `set up your own <https://openaps.readthedocs.io/en/latest/docs/Customize-Iterate/autotune.html>`_ using the freely available code or you can use a `ready built online service <https://autotuneweb.azurewebsites.net/>`_

.. warning::

   As Nightscout is the only source of data for Autotune, it's important that the data in Nightscout is complete. If you don't log all of your carb intake for example, Autotune is likely to produce misleading results.

Profiles & Basals
-----------------

Autotune does not use profile switch events from Nightscout. It assumes that the profile given as part of the input to Autotune was used for the entire period of data being considered.

If you are using a closed loop system this shouldn't be a problem, as most of the time you will have a temp basal rate running so even if the base profile basal rate was changed, the temp basal records in Nightscout will record what basal rate was running at the time. However, if you are running for most of the time using your standard profile basal, you may not get accurate records if your profile has changed significantly over the time period you are running Autotune over. You should therefore restrict Autotune to running only over a time period for which you were using the same basal profile.

Another point to note is how temp basal records are recorded in Nightscout. If you record temp basals as a percentage of the profile basal, those records will not be used by Autotune and it will assume that the profile basal was in force at the time. Your temp basal records should either be recorded as absolute basal values, or be percentages but with the additional ``rate`` property specified as part of the record as well.

Autotune Process
----------------

Autotune will start with the profile that you give it, then run the following process on each day's worth of data in turn. The results from each day are used as the starting point for autotuning the next day's data, and so on. The results from the final day are given as the ultimate recommendation.

Categorization
--------------

Autotune takes the BG entries from Nightscout, along with carbs and insulin (boluses and temp basal records). It looks at the difference between the actual and expected changes between each consecutive BG entry and uses the associated carbs and insulin data to determine why the change between the two BG entries was what it was.

Deltas and Deviations
~~~~~~~~~~~~~~~~~~~~~

In a sequence of BG readings, the delta is the difference between a BG reading and the preceding reading.

For each reading it is also possible to calculate the amount of insulin that would have taken effect since the preceding reading. Multiplying this amount of insulin activity by the insulin sensitivity factor (ISF) gives the expected change in BG (called by Blood Glucose Impact, or BGI).

The deviation is the difference between the actual BG delta and the BGI.

.. raw:: html
   
   <table class="wy-table wy-table-striped">
       <thead>
           <tr>
               <th>Time</th>
               <th>10:00</th>
               <th>10:05</th>
               <th>10:10</th>
               <th>10:15</th>
               <th>10:20</th>
           </tr>
       </thead>
       <tbody>
           <tr>
               <th style="text-align: left">BG</th>
               <td>100</td>
               <td>102</td>
               <td>104</td>
               <td>103</td>
               <td>100</td>
           </tr>
           <tr>
               <th style="text-align: left">
                   Delta<br />
                   <code>Curr. BG - Prev. BG</code>
               </th>
               <td>0</td>
               <td>2</td>
               <td>2</td>
               <td>-1</td>
               <td>-3</td>
           </tr>
           <tr>
               <th style="text-align: left">IOB</th>
               <td>1.0</td>
               <td>0.99</td>
               <td>0.97</td>
               <td>0.95</td>
               <td>0.93</td>
           </tr>
           <tr>
               <th style="text-align: left">
                   Insulin Activity (IA)<br />
                   <code>Prev. IOB - Curr. IOB</code>
               </th>
               <td>0.00</td>
               <td>0.01</td>
               <td>0.02</td>
               <td>0.02</td>
               <td>0.02</td>
           </tr>
           <tr>
               <th style="text-align: left">
                   BGI<br />
                   <code>-IA * ISF</code><br />
                   Assuming ISF of 180
               </th>
               <td>0.00</td>
               <td>-1.8</td>
               <td>-3.6</td>
               <td>-3.6</td>
               <td>-3.6</td>
           </tr>
           <tr>
               <th style="text-align: left">
                   Deviation<br />
                   <code>Delta - BGI</code>
               </th>
               <td>0.0</td>
               <td>3.8</td>
               <td>5.6</td>
               <td>2.6</td>
               <td>0.6</td>
           </tr>
       </tbody>
   </table>

Each deviation is allocated to *one* of several different contributing factors:

.. raw:: html
   <script type="text/javascript" src="https://www.google.com/jsapi?autoload={'modules':[{'name':'visualization','version':'1','packages':['corechart']}]}"></script>
   <script type="text/javascript">
   var drawChart = function(chartId, carbs, bolus, bg) {
       var data = new google.visualization.DataTable();
       data.addColumn('timeofday', 'Time');
       data.addColumn('number', 'BG (mg/dL)');
       data.addColumn('number', 'IOB (U)');
       data.addColumn('number', 'COB (g)');
       data.addColumn('number', 'Expected BG');
       data.addColumn('number', 'Dev');
       data.addColumn({ type: 'string', role: 'tooltip', p: { html: true } });
       // Taken from https://github.com/Perceptus/GlucoDyn/blob/master/js/glucodyn/algorithms.js
       //scheiner gi curves fig 7-8 from Think Like a Pancreas, fit with a triangle shaped absorbtion rate curve
       //see basic math pdf on repo for details
       //g is time in minutes,gt is carb type
       function cob(g,ct) {  
         if(g<=0) {
           tot=0.0
         } else if (g>=ct) {
           tot=1.0
         } else if ((g>0)&&(g<=ct/2.0)) {
           tot=2.0/Math.pow(ct,2)*Math.pow(g,2)
         } else 
           tot=-1.0+4.0/ct*(g-Math.pow(g,2)/(2.0*ct))
           return(tot);
       }
       //g is time in minutes from bolus event, idur=insulin duration
       //walsh iob curves
       function iob(g,idur) {  
         if(g<0.0) {
           tot=0.0  
         } else if(g==0.0) {
           tot=100.0
         } else if (g>=idur*60.0) {
           tot=0.0
         } else {
           if(idur==3) {
             tot=-3.203e-7*Math.pow(g,4)+1.354e-4*Math.pow(g,3)-1.759e-2*Math.pow(g,2)+9.255e-2*g+99.951
           } else if (idur==4) {
             tot=-3.31e-8*Math.pow(g,4)+2.53e-5*Math.pow(g,3)-5.51e-3*Math.pow(g,2)-9.086e-2*g+99.95
           } else if (idur==5) {
             tot=-2.95e-8*Math.pow(g,4)+2.32e-5*Math.pow(g,3)-5.55e-3*Math.pow(g,2)+4.49e-2*g+99.3
           } else if (idur==6) {
             tot=-1.493e-8*Math.pow(g,4)+1.413e-5*Math.pow(g,3)-4.095e-3*Math.pow(g,2)+6.365e-2*g+99.7
           } 
         }          
         return(tot);
       }
       var isf = 18;
       var expectedBG = bg[0];
       var basal = 5;
       var csf = false;
       var uam = false;
       var addRow = function(i) {
           var b = bg[i];
           var bPrev = i == 0 ? b : bg[i-1];
           var delta = b - bPrev;
           var c = i < carbs.time ? 0 : carbs.carbs * (1 - cob((i - carbs.time) * 5, 90));
           var ins = bolus.insulin * iob((i - bolus.time) * 5, 3) / 100;
           if (i == bolus.time)
               ins -= bolus.insulin;
           var insPrev = bolus.insulin * iob((i - bolus.time - 1) * 5, 3) / 100;
           var bgi = -(insPrev - ins) * isf;
           var dev = delta - bgi;
           var tt = "<p>Dev: <b>" + (Math.round(dev * 100) / 100) + "</b>";
           if (c > 0) {
               tt += "<br/>Classification: <b>CSF</b></p><hr/><p>COB &gt; 0";
               csf = true;
           }
           else if (csf && dev > 0) {
               tt += "<br/>Classification: <b>CSF</b></p><hr/><p>COB = 0 but Dev &gt; 0 so continuing previous CSF";
           }
           else {
               csf = false;
               if (ins > basal || dev > 6 || uam) {
                   if (dev > 0) {
                       uam = true;
                   }
                   else {
                       uam = false;
                   }
                   tt += "<br/>Classification: <b>UAM</b></p><hr/><p>";
                   if (ins > basal)
                       tt += "IOB &gt; basal";
                   else if (dev > 6)
                       tt += "Dev &gt; 6";
                   else if (uam)
                       tt += "Dev &gt; 0 so continuing previous UAM";
                   else
                       tt += "Dev &lt;= 0 so finishing UAM";
               }
           }
           tt += "</p>";
           expectedBG += bgi;
           if (expectedBG < 0)
               expectedBG = 0;
           var time = 10 * 60 + i * 5;
           var hour = Math.floor(time / 60);
           var minutes = time % 60;
           data.addRow([ [ hour, minutes, 0 ], b, ins, c, expectedBG, dev, "<div style='padding: 4px'>" + tt + "</div>" ])
       }
       for (var i = 0; i < bg.length; i++)
           addRow(i);
       var options = {
           width: 900,
           height: 500,
           series: {
             0: {targetAxisIndex: 0},
             1: {targetAxisIndex: 1, lineWidth: 1},
             2: {targetAxisIndex: 1, lineWidth: 1},
             3: {targetAxisIndex: 0, lineDashStyle: [4, 4]},
             4: {targetAxisIndex: 1}
           },
           vAxes: {
             // Adds titles to each axis.
             0: {title: '', minValue: 0},
             1: {title: '', minValue: 0}
           },
           tooltip: {isHtml: true}
       };
       var chart = new google.visualization.LineChart(document.getElementById(chartId));
       chart.draw(data, options);
   }
   </script>

* **CSF** - if there are COB, or while the deviations stay positive (BG is rising quicker or not falling as fast as expected based on IOB) after COB reaches 0, those deviations are logged against the carb sensitivity factor (CSF)

  In the example below, carbs and insulin are delivered at 10:05. All carbs are absorbed by 11:35 but deviations stay positive until 12:00, so all deviations from 10:05 to 11:55 are classed as CSF.

.. raw:: html
   <div id="csf_chart"></div>
   
   <script type="text/javascript">
   drawChart("csf_chart", { time: 1, carbs: 20 }, { time: 1, insulin: 10 }, [ 100, 102, 110, 120, 135, 140, 143, 144, 130, 118, 112, 102, 98, 96, 95, 94, 95, 97, 100, 102, 105, 104, 101, 98, 92, 90, 89, 86, 85, 84, 82, 80 ]);
   </script>

* **UAM** - if there is more IOB than the current hourly basal rate, or the deviation was more than 6 mg/dL, those deviations are logged against unannounced meals (UAM)
* **basal** - if the expected impact on BG of basal insulin is 4 or more times that of the net IOB, or the BG is rising, those deviations are logged against basals
* **ISF** - if the BG is falling and the the expected impact on BG of the net IOB is at least a quarter of the basal insulin, those deviations are logged against the insulin sensitivity factory (ISF)

After completing this process, some of the deviations will be moved to the other categories:

* If the ``--categorize-uam-as-basal`` option was specified on the Autotune command line, all the UAM deviations will be moved to the basal category. This is a useful option to specify if you have definitely entered all carbs, including rescue carbs, into Nightscout
* If there are more than twice as many deviations classified as UAM than basal, the lowest 50% of the UAM deviations will be moved to the basal category and the highest 50% to the ISF category. This is based on the assumption that the basal & ISF settings are too far out, causing large deviations that have been incorrectly classified as UAM
* If there were fewer than 10 ISF deviations, and more than 4 times as many CSF deviations than basal deviations, all the CSF deviations will be moved to the ISF category. This is based on the assumption that the carb absorption calculations are out

Autotuning Carb Ratio
---------------------

The carb ratio (CR) is autotuned by looking at the starting and ending BG for each meal (BG when carbs were entered and BG when COB reached zero and IOB was less than half the hourly basal rate). The CR is then calculated based on the IOB that was already present at the start of the meal and the insulin delivered during the absoption of the meal, plus/minus any additional insulin required to bring the ending BG back to the same level as the starting BG.

This is done for each meal and then averaged over the day to produce the suggested CR. Only 20% of the change to the CR is used in the output so it doesn't produce fast-changing recommendations.

Autotuning Basals
-----------------

The total basal deviations in each hour are summed, then the current ISF is applied to work out how much insulin would be needed to counteract those deviations. Only 20% of this change in insulin is considered for the later calculations so it doesn't produce fast-changing recommendations.

If more insulin is needed, the basal rates for the previous 3 hours are each increased by 1/3 of the required extra insulin.

If less insulin is needed, the basal rates for the previous 3 hours are reduced in proportion to remove the calculated amount of insulin.

Finally, some hours will not have any deviations in the basal category, e.g. because all the deviations were allocated to carb absorption. In these cases, the basal recommendation will be based on the first hour before and after that were adjusted, taking 80% of the current basal setting for this hour plus 10% of each adjacent autotuned hour.

.. warning::
   
   Note that this last point means that hours that are routinely dominated by carb absorption will over time have their basal rates set to an average of the surrounding hours. If you do have an underlying need for higher or lower basal rates than normal at these times as well, Autotune will combine these changes with your CSF. While those changes may work for these times, it can lead to a dangerously high carb ratio for other times.

Autotuning ISF
--------------

The deviations allocated to ISF are compared to the expected deviation. These ratios between actual and expected deviations are sorted and the median one is taken. The new ISF is then calculated by multiplying this median ratio by the current ISF. Only 20% of the change to the ISF is used in the output so it doesn't produce fast-changing recommendations.

Safety Limits
-------------

A minimum and maximum ratio for all changes is included as part of the Autotune input. The suggested settings for these are -30% and +20%. Any suggested changes will be limited to this range, including CR, ISF and basals.