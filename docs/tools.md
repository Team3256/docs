---
sidebar_position: 4
---

# FRC Software Tools

We got a lot of tools. Should be self-explanatory on why.

## [AdvantageScope](https://github.com/Mechanical-Advantage/AdvantageScope)

AdvantageScope is made by team 6328 Mechanical Advantage and is a popular software for viewing logs. It can read logs in WPILOG, DS log, Hoot (the format used for CTRE motors), and RLOG while also being able to view live data from NT4 (network tables) and RLOG streaming. AdvantageScope is separate from AdvantageKit (a logging framework).

### Setting Up

Click AdvantageScope->Settings to get to the below screen. Then change the roboRIO address to your team number (10.te.am.2). Ex. 10.32.56.2. See wpilib docs.

To connect to the robot go to file->connect to the robot. AdvantageScope also supports reading NT values from a simulator running locally on your computer. To view logs that were recorded onto the robot click file->download logs to download them locally onto your computer. You can open them via file->open and can compare those logs with another log file using file-> open multiple.

Once connected to the robot (in this example I’m running a simulator) you will see 6 tabs all from network tables. Most are self-explanatory. Shuffleboard is mostly empty and so is the live window. SmartDashboard will contain swerve values used to get can coder offsets and has the auto chooser. Pathplanner will contain info about the current path that is running.

And last but certainly not least we have AdvantageKit. AdvantageKit is where the bulk of most of the values that you want to log are. It is split by subsystem and has some additional tabs including DriverStation, PowerDistribution, RealOutputs, and SystemStats. Most of these are self-explanatory. Each subsystem logs values from the hardware including things such as position, velocity, and stator current. For vision, this could be the Tx and Ty values. All of these values are read straight from the hardware and are not processed at all. Any processed values go to RealOutputs. Think of these values as the “input” and after any processing, you can log the processed values into RealOutputs.

AdvantageScope has a huge array of methods to visualize data, with the most common being the line graph. Any numerical or boolean value can be dragged onto the line graph to be visualized and compared to other values. This is especially useful when comparing logs against each other. AdvantageScope also has the potential to have 3d simulated robots and mechanisms (you can still do 2d). It also has a better console compared to the NI driver station app (it's horrible).

## [Driver Station](https://docs.wpilib.org/en/stable/docs/software/driverstation/driver-station.html)

The docs will do a better job than I can. This is used to control the robot during competition and practice. You can view battery voltage, and enable/disable the robot. Press enter to disable the robot and spacebr to E-stop the robot (E-stop means you have to restart the robot once pressed).

## [Pathplanner](https://pathplanner.dev/gui-editing-paths-and-autos.html#paths)

Once again the docs are probably better + I don’t have too much experience. This is what we mainly used to make autos+paths but that could change with the recent release of choreo.

## [Choreo](https://sleipnirgroup.github.io/Choreo/)

Choreo is used to find time-optimized trajectories for swerve robots. It requires a decent amount of computing power to solve paths and therefore cannot be generated on the fly. Choreo generates realistic trajectories for swerve with trajopt. This reduces tuning time since all the generated trajectories are supposed to be realistic (compared to pathplanner which can generate impossible trajectories).

## [Shuffleboard](https://docs.wpilib.org/en/stable/docs/software/dashboards/shuffleboard/index.html)

Displays network table info with widgets that can be controlled within code. Slow and sometimes unreliable. Largly superseded by [Elastic](#elastic)

## [SmartDashboard](https://docs.wpilib.org/en/stable/docs/software/dashboards/smartdashboard/index.html)/[Glass](https://docs.wpilib.org/en/stable/docs/software/dashboards/glass/index.html)

SmartDashboard is a more lightweight version of Shuffleboard with not as many nice-to-have features. It displays network table info as widgets but doesn’t use a lot of resources.
Glass is a data visualization tool used for high-performance plotting. It has a bad UI similar to one of the robot simulators. When running `./gradlew simulateJava` a interface similar to Glass will show up.

## [Elastic](https://github.com/Gold872/elastic-dashboard)

Elastic is a modern dashboard alternative to Shuffleboard. It is not meant for data visualization or log playback but instead to serve as a driver dashboard during competition. It has the same widget style as Shuffleboard but is much faster and more efficient. At the current state (6/22/24) it is missing a few widget types from Shuffleboard. Elastic also allows for alerts, which can be used to display any errors on the driveerstation.

## [WIP] [Phoenix Tuner X](https://v6.docs.ctr-electronics.com/en/stable/docs/tuner/index.html)

Phoenix Tuner X is made by CTRE and is meant to be used to interface with their hardware and can be used for tuning PID or control motors/leds within the software. The docs will do a better job than me to explain how to use it, but this is a very critical app that software uses. Phoenix Tuner is used to update firmware, license products, and set device IDs. In addition to all of this, you can view device faults and graph status signals such as velocity and voltage. It can also be used to calibrate devices such as the Pigeon 2. While you can flash PID configs (and other configs as well) to tune, it is recommended to tune via [sysid](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/system-identification/introduction.html) (this is the link for using [ctre status signals with sysid](https://v6.docs.ctr-electronics.com/en/stable/docs/api-reference/wpilib-integration/sysid-integration/index.html)). Phoenix Tuner X also allows you to control TalonFX motors through the canivore via USB which is useful for testing. You can control all CTRE devices that are on the CAN chain, and even run motors with similar control requests that you would use in code.

## [REV Hardware Client](https://docs.revrobotics.com/rev-control-system/getting-started/rev-hardware-client)

We don't use REV hardware outside of the PDH, but this is a useful tool for configuring REV hardware. It can be used to update firmware, configure devices, and view device status, similar to Tuner X.

:::note

REV PSA: There are random configuration parameters that can cause motors to click on and off randomly that don't get cleared on `resetFactoryDefaults()` calls from [robot code], but do get reset from the REV Hardware Client.

[Source - Lyssia's Programming Server](https://discord.com/channels/1159743252505837601/1159743253491486812/1206827191393067078)

:::
