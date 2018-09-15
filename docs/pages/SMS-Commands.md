# SMS Commands

It is possible to control AndroidAPS remotely via SMS messages. This feature is intended so that parents, for example, can make remote interventions.

AndroidAPS will only accept SMS commands from designated phone numbers for security.

### To set up SMS commands on AndroidAPS

On your Android phone setting go to Applications > AndroidAPS > Permissions and enable SMS

In AndroidAPS go to Preferences > SMS Communicator and enter the phone number(s) that you will allow SMS commands to come from and also enable 'Allow remote commands via SMS'

Send a SMS to the phone with AndroidAPS running from your approved phone number(s) using any of the commands in the table below, the AndroidAPS phone will respond to confirm success of command or status request.

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
</style>

<table class="tg">
  <tr>
    <th class="tg-0pky">Command</th>
    <th class="tg-0pky">Response</th>
  </tr>
  <tr>
    <td class="tg-0pky">BG</td>
    <td class="tg-0pky">Last BG: 5.6 4min ago, Delta: -0,2 mmol, IOB: 0.20U (Bolus: 0.10U Basal: 0.10U)</td>
  </tr>
  <tr>
    <td class="tg-0pky">LOOP STOP/DISABLE</td>
    <td class="tg-0pky">Loop has been disabled</td>
  </tr>
  <tr>
    <td class="tg-0pky">LOOP START/ENABLE</td>
    <td class="tg-0pky">Loop has been enabled</td>
  </tr>
  <tr>
    <td class="tg-0pky">LOOP STATUS</td>
    <td class="tg-0pky">Loop is disabled<br>Loop is enabled<br>Suspended (10 min)</td>
  </tr>
  <tr>
    <td class="tg-0pky">LOOP SUSPEND 20</td>
    <td class="tg-0pky">Loop suspended for 20 minutes</td>
  </tr>
  <tr>
    <td class="tg-0pky">LOOP RESUME</td>
    <td class="tg-0pky">Loop resumed</td>
  </tr>
  <tr>
    <td class="tg-0pky">TREATMENTS REFRESH</td>
    <td class="tg-0pky">TREATMENTS REFRESH 1 receivers</td>
  </tr>
  <tr>
    <td class="tg-0pky">NSCLIENT RESTART</td>
    <td class="tg-0pky">NSCLIENT RESTART 1 receivers</td>
  </tr>
  <tr>
    <td class="tg-0pky">DANAR / PUMP</td>
    <td class="tg-0pky">Last conn: 1 minago Temp: 0.00U/h @11:38 5/30min IOB: 0.5U Reserv: 34U Batt: 100</td>
  </tr>
  <tr>
    <td class="tg-0pky">BASAL STOP/CANCEL</td>
    <td class="tg-0pky">To stop temp basal reply with code EMF</td>
  </tr>
  <tr>
    <td class="tg-0pky">BASAL 0.3</td>
    <td class="tg-0pky">To start basal 0.3U/h reply with code SWE<br>Remote basal setting is not allowed (if remote commands not allowed)</td>
  </tr>
  <tr>
    <td class="tg-0pky">BOLUS 1.2</td>
    <td class="tg-0pky">To deliver bolus 1.2U reply with code RRT<br>Remote bolus not allowed (if within 15 min after last bolus command or remote commands not allowed)</td>
  </tr>
  <tr>
    <td class="tg-0pky">CAL 5.6</td>
    <td class="tg-0pky">To send calibration 5.6 reply with code Rrt<br>Calibration sent (if xDrip is installed. Accepting calibrations must be enabled in xDrip+)</td>
  </tr>
</table>
