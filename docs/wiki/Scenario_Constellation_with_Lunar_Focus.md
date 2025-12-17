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

Note that this is presently only in the branch mentioned above; however, the NOS3 team expects to incorporate support for multiple spacecraft into a future release.

## Background

The following is a description of the files in a base NOS3 scenario that need modified in order to have a constellation with lunar focus scenario.

These files fall into three categories:  top level configuration, 42 configuration, and simulation configuration.

### Top Level Configuration
The top level configuration file that needs modified is `cfg/nos3-mission.xml`.
Changes that need to be made include the following.
First, change the `<scenario>` from "STF1" to "Gateway".
This will cause the scenario to use `cfg/InOut/Inp_Sim_Gateway.txt` as the `cfg/InOut/Inp_Sim.txt` file and `cfg/InOut/Inp_Graphics_Gateway.txt` as the `cfg/InOut/Inp_Graphics.txt` file for 42.
Next change `<number-spacecraft>` from "1" to "3".
Finally, change `<sc-1-cfg>` from "spacecraft/sc-mission-config.xml" to "spacecraft/sc-minimal-config.xml" and add `<sc-2-cfg>` and `<sc-3-cfg>` with values "spacecraft/sc-minimal-config.xml".

### 42 Configuration
The first file that needs changed is to make `cfg/InOut/Inp_Sim_Gateway.txt` based on `cfg/InOut/Inp_Sim.txt`.
Line 11 should be changed to "Orb_NRHO.txt" so that the Lunar near rectilinear halo orbit (NRHO) is used as the orbit for the spacecraft.
Note that the file `cfg/InOut/Orb_NRHO.txt` is already provided with NOS3 and has been modified to specify a Lunar NRHO orbit.
Line 13 should be changed from "1" to "3" and lines 14, 15, and 16 should have "SC_Gateway.txt", "SC_Gateway2.txt", and "SC_Gateway3.txt".
Note that the files `cfg/InOut/SC_Gateway.txt`, `cfg/InOut/SC_Gateway2.txt`, and `cfg/InOut/SC_Gateway3.txt` are already provided with NOS3 and have been modified to specify orbit offsets and different spacecraft models.
If needed the parameters for the spacecraft bodies and for various sensors and actuators on the spacecraft can also be changed in these files.

The next file that needs changed is to make `cfg/InOut/Inp_Graphics_Gateway.txt` based on `cfg/InOut/Inp_Graphics.txt`.
The main thing to change here is line 16 to specify a reasonable POV range for the modified spacecraft model.

Finally, the file `cfg/InOut/Inp_IPC.txt` needs changed.
It needs changed to add IPC connections for the simulators for spacecraft 2 and 3.
This involves duplicating the first 15 blocks of IPC parameters for spacecraft 2 and 3 and then changing the appropriate spacecraft prefixes from "SC[0]" to "SC[1]" or "SC[2]" as appropriate.
In addition, the server ports need changed so that they are unique and correspond to the port numbers specified in the simulation configuration files `sc-1-nos3-simulator.xml`, `sc-2-nos3-simulator.xml`, and `sc-3-nos3-simulator.xml`.

### Simulation Configuration
The simulation configuration files that need modifed are to make `cfg/sims/sc-1-nos3-simulator.xml`, `cfg/sims/sc-2-nos3-simulator.xml`, and `cfg/sims/sc-3-nos3-simulator.xml` all based on `cfg/sims/nos3-simulator.xml`.
`cfg/sims/sc-1-nos3-simulator.xml` should be a copy of `cfg/sims/nos3-simulator.xml`.
`cfg/sims/sc-2-nos3-simulator.xml` should be a copy of `cfg/sims/nos3-simulator.xml` except that the simulator data provider ports should be changed so that they are unique and correspond to the port numbers specified in the 42 configuration file `Inp_IPC.txt`.
`cfg/sims/sc-3-nos3-simulator.xml` should also be a copy of `cfg/sims/nos3-simulator.xml` except that the simulator data provider ports should be changed so that they are unique and correspond to the port numbers specified in the 42 configuration file `Inp_IPC.txt`.
