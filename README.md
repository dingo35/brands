
# Note
<span style="color:red">
This fork is the next generation of the SmartEVSE-v3 firmware.<br/>
Feel free to use this repository to build it yourself or to use the latest on from the *releases* folder <b>but this is on your own risk</b>.
</span>
<br />
<br />

# All menu options on the LCD screen:
```
MODE    (only appears when a MAINSMET is configured):
        Per default you are in Normal EVSE mode, but if a MAINSMET is configured you can
        also choose Smart Mode or Solar Mode:
  <Normal>	The EV will charge with the current set at MAX
  <Smart>	The EV will charge with a dynamic charge current, depending on MAINSMET
                data, and MAINS, MAX, MIN settings
  <Solar>       The EV will charge on solar power

CONFIG  Configure EVSE with Type 2 Socket or fixed cable:
  <Socket>      Your SmartEVSE is connected to a socket, so it will need to sense the
                cable used for its maximum capacity
  <Fixed>       Your SmartEVSE is connected to a fixed cable, so MAX will determine your
                maximum charge current

LOCK    (only appears when CONFIG is set to <Socket>)
        Enable or disable the locking actuator (config = socket)
  <Disabled>    No lock is used
  <Solenoid>	Dostar, DUOSIDA DSIEC-ELB or Ratio lock
  <Motor>	Signal wire reversed, DUOSIDA DSIEC-EL or Phoenix Contact

MULTI   (only appears when a MAINSMET is configured); formerly known as LOADBALANCING.
        2 to 8 EVSE’s can be connected via modbus, and their load will be balanced
  <Disabled>	Single SmartEVSE
  <Master>	Set the first SmartEVSE to Master. Make sure there is only one Master.
  <Node1-7>	And the other SmartEVSE's to Node 1-7.

MAINS	(only appears when a MAINSMET is configured):
        Set Max Mains current: 10-200A (per phase)

MIN     (only appears when a MAINSMET is configured):
        Set MIN charge current for the EV: 6-16A (per phase)

MAX	Set MAX charge current for the EV: 10-80A (per phase)
        If CONFIG is set to <Fixed>, configure MAX lower or equal to the maximum current
        that your fixed cable can carry.

CIRCUIT	(only appears when MULTI set to <Master>, or when MULTI set to <Disabled>
        and Mode is Smart or Solar):
        Set the max current the EVSE circuit can handle (load balancing): 10-200A
        (see also subpanel wiring)

SWITCH  Set the function of an external switch connected to pin SW
  <Disabled>    A push button on io pin SW can be used to STOP charging
  <Access B>    A momentary push Button is used to enable/disable access to the charging station
  <Access S>    A toggle switch is used to enable/disable access to the charging station
  <Sma-Sol B>   A momentary push Button is used to switch between Smart and Solar modes
  <Sma-Sol S>   A toggle switch is used to switch between Smart and Solar modes

RCMON   RCM14-03 Residual Current Monitor is plugged into connector P1
  <Disabled>    The RCD option is not used
  <Enabled>     When a fault current is detected, the contactor will be opened

RFID    use a RFID card reader to enable/disable access to the EVSE
        A maximum of 20 RFID cards can be stored.
                <Disabled> / <Enabled> / <Learn> / <Delete> / <Delete All>

MAINSMET Set type of MAINS meter
  <Disabled>    No MAINS meter connected; only Normal mode possible, no MULTIple SmartEVSE's possible
  <Sensorbox>   the Sensorbox will send measurement data to the SmartEVSE
  <API>         The MAINS meter data will be fed through the REST API or the MQTT API.
  <Phoenix C> / <Finder> / <...> / <Custom> a Modbus kWh meter is used

  Note that Eastron1P is for single phase Eastron meters, Eastron3P for Eastron three phase
  meters, and InvEastron is for Eastron three phase meter that is fed from below (inverted).
  If MAINSMET is not <Disabled> and not <API>, these settings appear:
  MAINSADR  Set the Modbus address for the kWh meter
  GRID      (only appears when Sensorbox with CT’s is used)
            3 or 4 wire
  CAL	    Calibrate CT1. CT2 and CT3 will use the same cal value.
	    6.0-99.9A	A minimum of 6A is required in order to change this value.
            Hold both ▼and ▲ buttons to reset to default settings.

EV METER Set type of EV kWh meter (measures power and charged energy)
  <Disabled>  No EV meter connected.
  <API>         The EV meter data will be fed through the REST API or the MQTT API.
  <Phoenix C> / <Finder> / <...> / <Custom> a Modbus kWh meter is used

  Note that Eastron1P is for single phase Eastron meters, Eastron3P for Eastron three phase
  meters, and InvEastron is for Eastron three phase meter that is fed from below (inverted).
  If EV METER is not <Disabled> and not <API>, this setting appears:
  EV ADR   Set the Modbus address for the EV Meter

WIFI          Enable wifi connection to your LAN
  <Disabled>  No wifi connection
  <SetupWifi> The SmartEVSE presents itself as a Wifi Acces Point "smartevse-xxxx";
              connect with your phone to that access point, goto http://192.168.4.1/
              and configure your Wifi password
  <Enabled>   Connect to your LAN via Wifi.

MAX TEMP      Maximum allowed temperature for your SmartEVSE; 40-75C, default 65.
              You can increase this if your SmartEVSE is in direct sunlight.

SUMMAINS      Maximum allowed current summed over all phases: 10-600A
              This is used for the EU Capacity rate limiting, currently only in Belgium

MODEM         If a modem that can communicate with your EV is connected
  <Disabled>          No modem connected
  <Experimental>      Only for expert use,
                      if you have a modem connected AND compile the firmware with MODEM=1.

The following options are only shown when Mode set to <Solar> and
Multi set to <Disabled> or <Master>:
START         set the current on which the EV should start Solar charging:
              -0  -48A (sum of all phases)
STOP          Stop charging when there is not enough solar power available:
              Disabled - 60 minutes (Disabled = never stop charging)
IMPORT        Allow additional grid power when solar charging: 0-20A (summed over all phases)
CONTACT2      One can add a second contactor (C2) that switches off 2 of the 3 phases of a
              3 phase Mains installation; this can be usefull if one wants to charge of off
              Solar; EV's have a minimal charge current of 6A, so switching off 2 phases
              allows you to charge with a current of 6-18A, while 3 phases have a
              minimum current of 3x6A=18A.
              This way you can still charge solar-only on smaller solar installations.
  <Not present> No second contactor C2 is present (default)
  <Always Off>  C2 is always off, so you are single phase charging
  <Always On>   C2 is always on, so you are three phase charging (if your Mains are three phase and your EV supports it)
  <Solar Off>   C2 is always on except in Solar Mode where it is always off
  <Auto>        (not implemented yet, current behaviour is Always On)


```
# Webserver
After configuration of your Wifi parameters, your SmartEVSE will present itself on your LAN via a webserver. This webserver can be accessed through:
* http://ip-address/
* http://smartevse-xxxx.local/ where xxxx is the serial number of your SmartEVSE. It can be found on a sticker on the bottom of your SmartEVSE. It might be necessary that mDNS is configured on your LAN.
* http://smartevse-yyyy.lan/ where yyyy is a derivative of the MAC addresss, that was shown to you when you configured your Wifi parameters.

# Multiple SmartEVSE controllers on one mains supply
Up to eight SmartEVSE modules can share one mains supply.
  - Hardware connections
    - Connect the A, B and GND connections from the Master to the Node(s).
    - So A connects to A, B goes to B etc.
    - If you are using Smart/Solar mode, you should connect the A, B , +12V and GND wires from the sensorbox to the same screw terminals of the SmartEVSE! Make sure that the +12V  wire from the sensorbox is connected to only  -one– SmartEVSE.

  - Software configuration
    - Set one SmartEVSE MULTI setting to MASTER, the others to NODE 1-7. Make sure there is only one Master, and the Node numbers are unique.
    - On the Master configure the following:
      - MODE	  Set this to Smart if a Sensorbox (or configured kWh meter) is used to measure the current draw on the mains supply.
      It will then dynamically vary the charge current for all connected EV’s.  If you are using a dedicated mains supply for the EV’s you can leave this set to Normal.
      - MAINS Set to the maximum current of the MAINS connection (per phase).
      If the sensorbox or other MainsMeter device measures a higher current then this value on one of the phases, it will immediately reduce the current to the EVSE’s
      - CIRCUIT Set this to the maximum current of the EVSE circuit (per phase).
      This will be split between the connected and charging EV’s.
      - MAX 		 Set the maximum charging current for the EV connected to -this- SmartEVSE (per phase).
      - MIN		 Set to the lowest allowable charging current for all connected EV’s.
    - On the Nodes configure the following:
      - MAX 		 Set the maximum charging current for the EV connected to -this- SmartEVSE (per phase).
# Error Messages
If an error occurs, the SmartEVSE will stop charging, and display one of the following messages:
* ERROR NO SERIAL COM	  CHECK WIRING<br>No signal from the Sensorbox or other SmartEVSE (when load balancing is used) has been received for 11 seconds. Please check the wiring to the Sensorbox or other SmartEVSE.
* ERROR NO CURRENT<br>There is not enough current available to start charging, or charging was interrupted because there was not enough current available to keep charging. The SmartEVSE will try again in 60 seconds.
* ERROR	HIGH TEMP<br>The temperature inside the module has reached 65º Celsius. Charging is stopped.
Once the temperature has dropped below 55ºC charging is started again.
* RESIDUAL FAULT CURRENT DETECTED<br>An optional DC Residual Current Monitor has detected a fault current, the Contactor is switched off.
The error condition can be reset by pressing any button on the SmartEVSE.



# Changes with regards to the original firmware
* New Status page using the Rest API
* Home battery integration
* Endpoint to send L1/2/3 data, this removed the need for a SensorBox
  * Note: Set MainsMeter to the new 'API' option in the config menu when sending L1/2/3
* Endpoint to send EvMeter L1/2/3 data (and energy/power)
  * Note: Set EvMeter to the new 'API' option in the config menu when sending L1/2/3
* Callable API endpoints for easy integration (see [API Overview](#API-Overview) and [Home Assistant Integration](#Integration-with-Home-Assistant))
  * Change charging mode
  * Override charge current
  * Pass in current measurements (p1, battery, ...) - this eliminates having to use additionalhard
  * Switch between single- and three phase power (requires extra 2P relais on the 2nd output)
* Added "Inverted Eastron" kWh, so that polarity is reversed when power is supplied to meter from below (like in most Dutch power panels)
* Added current-limiting functionality if a subpanel is used, example:

                             mains
                               |
                        [main breaker 25A]
                               |
                        [kWh meter "Mains"]
                               |
            -----------------------------------
            |            |                    |
                [group breaker 16A]   [subpanel breaker 16A]
                                              |
                                       [kWh meter "EV"]
                                              |
                                        ----------------
                                        |              |
                            [washer breaker 16A]  [smartevse breaker 16A]

   In this example you configure Mains to 25A, MaxCircuit to 16A; the charger will limit itself so that neither the 25A mains nor the 16A from the subpanel will be
   exceeded...
   Note that for this functionality you will need to be in Smart or Solar mode; it is no longer necessary to enable Load Balancing for this function.

* Added wifi-debugging: if compiled in, you can debug SmartEVSE device by telnetting to it over your wifi connection
* Added EXPERIMENTAL use of Contactor 2 (C2);
    - one can add a second contactor (C2) that switches off 2 of the 3 phases of a three-phase Mains installation; this can be usefull if one wants to charge of off
      Solar; EV's have a minimal charge current of 6A, so switching off 2 phases allows you to charge with a current of 6-18A, while 3 phases have a minimum current
      of 3x6A=18A. This way you can still charge solar-only on smaller solar installations.
    - one should wire C2 according to this schema:

            N    L1   L2   L3
            |    |    |    |
          --------------------
          | 4-p contactor C1 |
          --------------------
            |    |    |    |
            |    | ------------------
            |    | |2-p contactor C2|
            |    | ------------------
            |    |    |    |
          --------------------
          |    EV-cable      |
          --------------------

      This way the (dangerous) situation is avoided that some Phases are switched ON, and Neutral is switched OFF.
      Note that it is important that you actually DO NOT switch the L1 pin of the CCS plug with the C2 contactor; some cars (e.g. Tesla Model 3) will go into error;
      they expect the charging phase to be on the L1 pin when single-phase charging...
      Note also that in case the phases cannot be detected automatically (especially when no EVmeter is connected), and SmartEVSE _knows_ it is charging at a single
      phase (e.g. because Contact2 is at "Always Off"), it assumes that L1 is the phase we are charging on!!

    - by default C2 is switched OFF ("Not present"); if you want to keep on charging on 3 phases after installing C2, you should change the setting Contact2 in the
      Setup Menu.

    - There is a bug in the original firmware, and in the serkri firmware up until this version, that makes charging in Solar mode on a 3phase instalation,
      with a 3phase car toggle into an infinite start/stop/start.... sequence when not enough sun is available (e.g. when you only have a 1x16A solar feed).
      In order to fix this bug the behaviour of SmartEVSE is adapted:
    - When EVMeter and/of MainsMeter enabled, AND when in Smart or Solar mode, SmartEVSE now starts charging at MinCurrent (usually 6A); in the first 7 seconds it
      detects on which phases it is charging through EVMeter or MainsMeter (might be a Sensorbox)
    - if it is in Solar mode, and Contact2 is on "AUTO", and has not enough current to stay within ImportCurrent limits, it will switch
      C2 off to 1 phase charging. It will then move up the charging current to whatever is suitable.
      If current goes up again, it will NOT switch back to 3 phase charging since this is known to give problems with certain EVs.
    - if it is in Smart mode, it will now correctly limit its currents to the phases it is actually charging
    - This means the bug mentioned above is solved ONLY for people having Sensorbox and/or kWh-meter enabled.

    - Charging in Normal mode has not changed, and charging in Smart and Solar mode has not changed if you have no Sensorbox and/or kWh-meter enabled.


# Home Battery Integration
In a normal EVSE setup a sensorbox is used to read the P1 information to deduce if there is sufficient solar energy available. This however can give unwanted results when also using a home battery as this will result in one battery charging the other one. <br/>

For this purpose the settings endpoint allows you to pass through the battery current information:
* A positive current means the battery is charging
* A negative current means the battery is discharging

The EVSE will use the battery current to neutralize the impact of a home battery on the P1 information.<br>
**Regular updates from the consumer are required to keep this working as values cannot be older than 60 seconds.**

### Example
* Home battery is charging at 2300W -> 10A
* P1 has an export value of 230W -> -1A
* EVSE will neutralize the battery and P1 will be "exporting" -11A

The sender has several options when sending the home battery current:
* Send the current AS-IS -> EVSE current will be maximized
* Only send when battery is discharging -> AS-IS operation but EVSE will not discharge the home battery
* Reserve an amount of current for the home battery (e.g. 10A) -> Prioritize the home battery up to a specific limit

# API Overview
View API <a href="https://swagger-ui.serkri.be/" target="_blank">https://swagger-ui.serkri.be/</a>


Have an idea for the API? Edit it here <a href="https://swagger-editor.serkri.be/" target="_blank">https://swagger-editor.serkri.be/</a> and copy/paste it in a new issue with your request (https://github.com/serkri/SmartEVSE-3/issues)

# MQTT support
Your SmartEVSE can now export the most important data to your MQTT-server. Just fill in the configuration data on the webserver and the data will automatically be announced to your MQTT server.

# Integration with Home Assistant
There are three options to integrate SmartEVSE with Home Assistant:
* through the HA-integration - the easy way<br />

    If you want to integrate your SmartEVSE with Home Asisstant, please have a look at [the SmartEVSE `custom_component` for Home Assistant](https://github.com/dingo35/ha-SmartEVSEv3). This `custom_component` uses the API to share data from the SmartEVSE to Home Assistant, and enables you to set SmartEVSE settings from Home Assistant. You will need firmware version 1.5.2 or higher to use this integration.

* by manually configuring your configuration.yaml<br />

    Its a lot of work, but you can have everything exactly your way. See examples in the integrations directory of our github repository.

* by MQTT<br />

    If you don't like the integration, e.g. because it only updates its data every 60 seconds, you might like to interface through MQTT; updates are done as soon as values change.... you can even mix it up by using both the integration AND the MQTT interface at the same time!


# Simple Timer

There is a simple timer implemented on the webserver, for Delayed Charging.
* Upon refreshing your webpage, the StartTime field (next to the Mode buttons) will be filled with the current system time.
* If you press any of the Mode buttons, your charging session will start immediately;
* If you choose to enter a StartTime that is in the future, a StopTime field will open up;
  If you leave this to the default value it is considered to be empty; now if you press Normal, Solar or Smart mode
    - the StartTime will be registered,
    - the mode will switch to OFF,
    - a charging session will be started at StartTime, at either Normal or Smart mode;
    - the SmartEVSE will stay on indefinitely.
* If you enter a StopTime, a checkbox named "Daily" will open up; if you check this, the startime/stoptime combination will be used on a daily basis,
  starting on the date you entered at the StartTime.
* To clear StartTime, StopTime and Repeat, refresh your webpage and choose either Normal, Solar or Smart mode.
* Know bugs:
    - if your NTP time is not synchronized yet (e.g. after a reboot), results will be unpredictable. WAIT until time is settled.
    - if your StopTime is AFTER your StartTime+24Hours, untested territories are entered. Please enter values that make sense.

# Improved starting/stopping through the LCD screen
* When pressing o button longer then 2 seconds you will enter the Menu screen
* When pressing < button longer then 2 seconds the access will be denied, i.e. the mode will be set to "Off" and charging will stop
* When pressing > button longer then 2 seconds the access will be granted, i.e. the previously set mode will be activated and charging will start

# EU Capacity Rate Limiting
An EU directive gives electricity providers the possibility to charge end consumers by a "capacity rate", so consumers will be stimulated to flatten their usage curve.
Currently the only known country that has this active is Belgium.
For more details see https://github.com/serkri/SmartEVSE-3/issues/215

* In the Menu screen an item "SumMains" is now available, default set at 600A
* This setting will only be of use in Smart or Solar mode
* Apart from all other limits (Mains, MaxCirCuit), the charge current will be limited so that the sum of all phases of the Mains currents will not be exceeding the SumMains setting
* If you don't understand this setting, or don't live in Belgium, leave this setting at its default value

# Building the firmware

* Install platformio-core https://docs.platformio.org/en/latest/core/installation/methods/index.html
* Clone this github project, cd to the smartevse directory where platformio.ini is located
* Compile firmware.bin: platformio run
* Compile spiffs.bin: platformio run -t buildfs

If you are not using the webserver /update endpoint:
* Upload via USB configured in platformio.ini: platformio run --target upload
