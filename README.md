
# Note
<span style="color:red">
This fork is the next generation of the SmartEVSE-v3 firmware.<br/>
Feel free to use this repository to build it yourself or to use the latest on from the *releases* folder <b>but this is on your own risk</b>.
</span>
<br />
<br />

[Installation](docs/installation.md)<br>
[Configuration](docs/configuration.md)<br>
[Operation](docs/operation.md)<br>




# Changes with regards to the original firmware
* Endpoint to send L1/2/3 data, this removed the need for a SensorBox
  * Note: Set MainsMeter to the new 'API' option in the config menu when sending L1/2/3
* Endpoint to send EvMeter L1/2/3 data (and energy/power)
  * Note: Set EvMeter to the new 'API' option in the config menu when sending L1/2/3
* Callable API endpoints for easy integration (see [API Overview](#API-Overview) and [Home Assistant Integration](#Integration-with-Home-Assistant))
  * Change charging mode
  * Override charge current
  * Pass in current measurements (p1, battery, ...) - this eliminates having to use additionalhard
  * Switch between single- and three phase power (requires extra 2P relais on the 2nd output)

