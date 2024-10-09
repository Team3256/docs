---
sidebar_position: 4
---
# FRC Software Tools

We got a lot of tools. Should be self-explanatory on why.

## AdvantageScope

AdvantageScope is made by team 6328 Mechanical Advantage and is a popular software for viewing logs. It can read logs in WPILOG, DS log, Hoot (the format used for CTRE motors), and RLOG while also being able to view live data from NT4 (network tables) and RLOG streaming. AdvantageScope is separate from AdvantageKit (a logging framework).

### Setting Up

Click AdvantageScope->Settings to get to the below screen. Then change the roboRIO address to your team number (10.te.am.2). Ex. 10.32.56.2. See wpilib docs.

To connect to the robot go to file->connect to the robot. AdvantageScope also supports reading NT values from a simulator running locally on your computer. To view logs that were recorded onto the robot click file->download logs to download them locally onto your computer. You can open them via file->open and can compare those logs with another log file using file-> open multiple.
 
Once connected to the robot (in this example I’m running a simulator) you will see 6 tabs all from network tables. Most are self-explanatory. Shuffleboard is mostly empty and so is the live window.   SmartDashboard will contain swerve values used to get can coder offsets and has the auto chooser. Pathplanner will contain info about the current path that is running.

And last but certainly not least we have AdvantageKit. AdvantageKit is where the bulk of most of the values that you want to log are. It is split by subsystem and has some additional tabs including DriverStation, PowerDistribution, RealOutputs, and SystemStats. Most of these are self-explanatory. Each subsystem logs values from the hardware including things such as position, velocity, and stator current. For vision, this could be the Tx and Ty values. All of these values are read straight from the hardware and are not processed at all. Any processed values go to RealOutputs. Think of these values as the “input” and after any processing, you can log the processed values into RealOutputs.

AdvantageScope has a huge array of methods to visualize data, with the most common being the line graph. Any numerical or boolean value can be dragged onto the line graph to be visualized and compared to other values. This is especially useful when comparing logs against each other. AdvantageScope also has the potential to have 3d simulated robots and mechanisms (you can still do 2d). It also has a better console compared to the NI driver station app (it's horrible).

## Driver Station

The docs will do a better job than I can. This is used to control the robot during competition and practice.

## Pathplanner

Once again the docs are probably better + I don’t have too much experience. This is what we mainly used to make autos+paths but that could change with the recent release of choreo.

## Choreo

Choreo is used to find time-optimized trajectories for swerve robots. It requires a decent amount of computing power to solve paths and therefore cannot be generated on the fly. Here are the docs on how to use Choreo

## Shuffleboard

Displays network table info with widgets that can be controlled within code. Slow and sometimes unreliable. Largly superseded by [Elastic](#elastic)

## SmartDashboard/Glass

SmartDashboard is a more lightweight version of Shuffleboard with not as many nice-to-have features. It displays network table info as widgets but doesn’t use a lot of resources.
 Glass is a data visualization tool used for high-performance plotting. It has a bad UI similar to one of the robot simulators.

One of these (I don't remember is the default app that pops up when you run `./gradlew simulateJava`)

## Elastic

Elastic is a modern dashboard alternative to Shuffleboard. It is not meant for data visualization or log playback but instead to serve as a driver dashboard during competition. It has the same widget style as Shuffleboard but is much faster and more efficient. At the current state (6/22/24)  it is missing a few widget types from Shuffleboard.

## [WIP] Phoenix Tuner X

Phoenix Tuner X is made by CTRE and is meant to be used to interface with their hardware and can be used for tuning PID or control motors/leds within the software. The docs will do a better job than me to explain how to use it, but this is a very critical app that software uses. Phoenix Tuner is used to update firmware, license products, and set device IDs. In addition to all of this, you can view device faults and graph status signals such as velocity and voltage. It can also be used to calibrate devices such as the Pigeon 2. While you can flash PID configs (and other configs as well) to tune, it is recommended to tune via sysid (this is the link for using ctre status signals with sysid).
