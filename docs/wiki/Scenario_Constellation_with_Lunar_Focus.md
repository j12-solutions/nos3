# Scenario - Constellation with Lunar Focus

This scenario was developed to demonstrate having multiple spacecraft in a constellation in NOS3.  In addition, these spacecraft are in an orbit about the Earth-Moon L2 point.

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
You will notice that three flight software windows get opened with titles `sc0N - NOS3 Flight Software`, where `N` is 1, 2, and 3. Once you start COSMOS, in the command and telemetry server you will see three debug interfaces, `DEBUG_1`, `DEBUG_2`, and `DEBUG_3`.  This is shown in the figure below.

 ![MultipleSpacecraft](_static/scenario_multiple_spacecraft/MultipleSpacecraft.png)

You can verify that telemetry is being received by examining the `Bytes Rx` column.

Next we will show that we can command each of the three flight software instances.
In the packet viewer, select target `SAMPLE_1` and packet `SAMPLE_HK_TLM`.
The current command count `CMD_COUNT` should be 0.
Now in the command sender window, select target `SAMPLE_1` and command `SAMPLE_NOOP_CC`.
Click `Send` in the command sender window.
You should now see `SAMPLE: NOOP command received` in the `sc01 - NOS3 Flight Software` window and the `CMD_COUNT` increase to 1 in the packet viewer.
This is shown below:

![SendingCommand](_static/scenario_multiple_spacecraft/SendingCommand.png)

Now verify the same command and telemetry for spacecraft 2 by sending the command to target `SAMPLE_2` command `SAMPLE_NOOP_CC` and by viewing the `sc02 - NOS3 Flight Software` window and the target `SAMPLE_2` packet `SAMPLE_HK_TLM` packet viewer window.
Send the command three times and verify flight software receives it three times and the `CMD_COUNT` increases to three.
This is shown below:

![SendingCommand2](_static/scenario_multiple_spacecraft/SendingCommand2.png)

Finally verify the same thing for spacecraft 3 by sending the command 5 times and verifying that it is received five times and the `CMD_COUNT` increases to five.
This is shown below:

![SendingCommand3](_static/scenario_multiple_spacecraft/SendingCommand3.png)
