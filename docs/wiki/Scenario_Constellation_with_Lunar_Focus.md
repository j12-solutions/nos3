# Scenario - Constellation with Lunar Focus

This scenario was developed to demonstrate running NOS3 in a configuration which has multiple spacecraft in a constellation.  For this scenario, these spacecraft are in an orbit about the Earth-Moon L2 point.

This scenario was last updated on 12/10/2025 and leveraged the `802-constellation-scenario-with-lunar-focus` branch at the time [c415ff7].

## Learning Goals
By the end of this scenario you should be able to:
* Command multiple spacecraft from a single ground station
* Receive telemetry from multiple spacecraft in a single ground station

## Prerequisites
Before running the scenario, complete the following steps:

* [Getting Started](./NOS3_Getting_Started.md)
  * [Installation](./NOS3_Getting_Started.md#installation)
  * [Running](./NOS3_Getting_Started.md#running)

## Walkthrough
For this scenario, you will need to switch to the `802-constellation-scenario-with-lunar-focus` branch. Once you do that, you can do a typical `make` and `make launch`.
You will notice that three flight software windows are opened with titles `sc0N - NOS3 Flight Software`, where `N` is either 1, 2, or 3. Once you start COSMOS, you will similarly see three telemetry debug interfaces in the command and telemetry server (`DEBUG_1`, `DEBUG_2`, and `DEBUG_3`).  This is shown in the figure below.

 ![MultipleSpacecraft](_static/scenario_multiple_spacecraft/MultipleSpacecraft.png)

You can verify that telemetry is being received by examining the `Bytes Rx` column in the COSMOS Command and Telemetry Server window.

Next we will show that we can command each of the three flight software instances.
In the Packet Viewer, select target `SAMPLE_1` and packet `SAMPLE_HK_TLM`.
The current command count `CMD_COUNT` should be 0.
Now in the Command Sender window, select target `SAMPLE_1` and command `SAMPLE_NOOP_CC`.
Click `Send` in the Command Sender window.
You should now see `SAMPLE: NOOP command received` in the `sc01 - NOS3 Flight Software` window and the `CMD_COUNT` increase to 1 in the Packet Viewer.
This is shown below:

![SendingCommand](_static/scenario_multiple_spacecraft/SendingCommand.png)

Now verify the same command and telemetry for spacecraft 2.  Send the command `SAMPLE_NOOP_CC` to target `SAMPLE_2`, and view the `sc02 - NOS3 Flight Software` window and the Packet Viewer, with target set to `SAMPLE_2` and packet set to `SAMPLE_HK_TLM`.
Send the command three times; verify that flight software receives it three times and that the `CMD_COUNT` increases to three.  This is shown below:

![SendingCommand2](_static/scenario_multiple_spacecraft/SendingCommand2.png)

Finally, verify the same thing for spacecraft 3 in the same way.  Send the command 5 times and verifying that it is received five times and that the `CMD_COUNT` increases to five.
This is shown below:

![SendingCommand3](_static/scenario_multiple_spacecraft/SendingCommand3.png)

This concludes the Multiple Spacecraft Constellation scenario.

Note that this is presently only in the branch mentioned above; however, the NOS3 team expects to incorporate support for multiple spacecraft into a future release.

