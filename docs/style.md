---
sidebar_position: 3
---
# Code Style Guide

## General

Use [Spotless](https://github.com/diffplug/spotless). Run it on your code with `./gradlew spotlessApply`.

## Naming Conventions

Historically our naming conventions have not been good. This aims to fix that with standardization.

### Constants

Constants are decentralized with each subsystem containing one. Every constant variable should start with the letter “k” (stands for constant) and help maintain readability. All constants are `public static final`. Constant classes should not contain a constructor and should be `final`.

- We have our subsystem-specific constants (e.g. `SubsystemConstants.java` for a subsystem named `Subsystem`) and configs decentralized (because we historically had major merge conflict issues when we put it in a single file). This includes
  - Motor configs (e.g. PID gain values and current limits)
  - Subsystem characteristics (for physics simulation and gear ratios)
  - Position/velocity presets
  - `kUse___` constants: enables/disables a specific feature in the subsystem
- We have our feature flags and otherwise global constants (e.g. logging and controller constants) in the `Constants.java` file
  - `k___Enabled` constants: enables/disables a subsystem
  - Enabling features of a single subsystem should be within the decentralized subsystem constants file (unless it is a feature that spans across multiple subsystems)
- Don't write feature flags for things that you could just straight up not use. Feature flags is the non-scuffed way of "temporarily disabling this code by commenting it out"
  - If you ever find yourself needing to comment some code out, consider creating a feature flag
- Have the feature flag-checking logic (the if statement) outside of your function (or, if it's a subsystem, passed in as a boolean value of `enabled` for that subsystem's `DisabledSubsystem` constructor)

See [here](https://github.com/Team3256/Kirby_Bot_2024/blob/5518d6e71b4e049649abfa2ad3065cdd2de0cefd/src/main/java/frc/robot/Constants.java) for a good `Constants.java` example or [here](https://github.com/Team3256/Kirby_Bot_2024/blob/5518d6e71b4e049649abfa2ad3065cdd2de0cefd/src/main/java/frc/robot/subsystems/ampevator/AmpevatorConstants.java) for an example of a decent subsystem-specific constants file

### Subsystems

What defines a subsystem is mainly up to design unless combining two subsystems makes the code more readable and makes more sense. The command base framework makes it so that two commands that require or use the same subsystem cannot run at the same time. Any subsystem that has separate parts that need to be accessed by separate commands that have the potential to be running at the same time should be split. One example of software combining a subsystem is the 2024 passthrough and intake. Although design considers them as separate subsystems, in code there were no instances in which both motors would not be running at the same time allowing them to be combined. Subsystems should be named whatever design names them to maintain some form of standardization.

### Commands

A command should be named what it does. Command factories within subsystems do not need to mention the subsystem name within them. For example, a command factory could be named pivot.setPivotPosition or it could be named pivot.setPosition. The second example is correct because when called it is obvious that it is setting the position of the pivot and does not need to be mentioned twice. However complex commands that require their file (highly unlikely that it is just one subsystem) should contain the subsystem name within them. Complex commands that require multiple subsystems whether they are files or just command groups, should be named based on the goal that they accomplish or a sequence name. For example 2024: intake sequence (was the sequence ran every time we took a piece).

### General Variables

Variables should never be a single letter no matter how much of a rush you are in because it will probably end up costing you more time. Don’t abbreviate since it reduces readability, especially for newer members who might have trouble with FRC vocabulary or just general abbreviations. Instead of commenting the units next to a variable name (or just excluding it) put the unit name inside the variable. Ex. `delaySeconds` instead of `delay` or `motorPositionRotations` instead of `motorPosition`. Variables also follow camelCase in which the first letter of a variable is lowercase and all subsequent words have their first letter capitalized.

### Autos

#### Paths

Because the auto changes every year, the locations of game pieces and the starting location of the robot vary. Every year software should meet with strategy to find concise names for each key location that a robot will be traveling to. Paths should describe the node that they start from, and the node that they end at, including any nodes that they pass by in the middle. Ex. 2024 C2-C1 would describe moving from center note 2, to center note 1

#### Autos (paths combined with commands)

While paths describe every node that the robot goes to, an auto should not because this would dramatically increase the length of the name and make readability hard, putting more pressure on the drive team. A good auto name should give the general strategy of the auto and can include some of the main nodes it goes to. The path names (unless it's a special path) should just be which location the robot is going to/from (labelled with English letters; the exact letters representing what will depend on the year/game).

One example is Spectrum 2024 which had a decent auto naming convention that was concise while being descriptive. One problem with their auto naming convention is that it heavily relied on their strategy to always rush for 2 center notes. They might have a different naming convention for their wing notes, but it seems like the auto names focus on just the center notes. An auto could just be the name of all the game pieces that it goes towards to intake with some prefix, but this introduces the problem of having to decipher what the auto means every time you read it. This should be mostly whatever the preference of the drive coach wants/strat.

If we aren’t building simple autos within the path planner, then autos should be contained within their folder. Within autos/ should contain an AutoConstants file, a commands folder to house any auto-specific commands, and a folder named routines which contains a command file used to run a full auto routine.

#### Named Commands

~~Named commands for autos should be exactly what the command is named in the code. They should contain the subsystem name (assuming it's a singular subsystem command) as a prefix and the rest of the command name written in camel case. Any values being passed into the command should be separated by a space. Ex. `shooter.setVelocity(100)` would be shooter setVelocity 100.~~ We now have code to do it for us. (if you really want to, [check it out](https://github.com/Team3256/Kirby_Bot_2024/blob/main/src/main/java/frc/robot/utils/NamedCommands.java))

## File Structure

Any files that are only used within one subsystem should be moved into that subsystem’s folder as a helper file. This includes functions that contain physics models of a subsystem or math stuff, that instead of being part of the subsystem file should be part of a helper class. Ex. ShooterHelper. Any custom frameworks that we make that are not specific to one year and are planned to be reused in future years should be put under `lib/` (although we aren't very good with doing this lol).

## Comments

Comment **why** you are doing something, not what. The code should be easy enough to write that they are self-documenting (if not, split your code into more functions) and writing comments that can further explain. Comments can contain specific reasonings etc. but should be concise since any extra details can be put into the design doc of the feature (i.e. the issue or PR the issue was implemented in).

## Code Style

- Don’t nest too deep (beyond 3-4 blocks is too much). Abstraction for robot code can make it hard to read and can be detrimental if you are asking for help from a CSA student or another team since they have a harder time following what the code is doing.
- Run spotless (linter) before committing to formatting your code. If everyone does this consistently it looks good. Otherwise, if you have huge spotless applications occasionally it becomes hard to understand what exactly you changed in that commit since GitHub shows that a lot of files were changed by the spotless application.
  - If remembering is a hard thing, and doing it before every commit is unrealistic, at least try to run spotless before every feature branch is merged in.
- Follow DRY (don’t repeat yourself). If you are duplicating code a lot try creating a utility class within utils/. There should be no exposed numbers since they should all be contained within a constant file.
- Try making your code modular and reusable for future years.
- Not too related but avoid putting expensive tasks within periodic. Command loop overruns were a big thing toward the end of the 2024 season for us and it's hard to debug what’s causing it.

## Command-based

(Most of this documentation is stolen from Oblarg.)
Commands are a really powerful way to manage the state of the robot. In short, each subsystem can only have one command that requires it to run. Commands can then be made into groups to provide a lot of functionality and customizability. I won’t go into how command based works, but I will go over some of the more niche features and some general structure  guidelines.
Commands should be returned from factory functions from a specific function. This improves readability and ensures that a new instance is created every time we want to bind it to something.

A factory function can look something like the above. However when put inside a subsystem, it removes the need to put “intake” within the name, since its given (intake.runCommand can be read the same as intake.runIntakeCommand).
Commands that require more than one subsystem can use static command factories that are not within a subsystem.
This can be used to group commands together that are part of a sequence or are similar. One problem with command groups including the sequential command group is that it requires the subsystem the whole time while anything in the command group is running and not when the command runs. This can be a problem when a command group is waiting for a condition to come true to move on. This can be solved with .asProxy() which schedules the command “as a proxy”. While it can be very useful it will lead to unexpected behaviors when used incorrectly and should be using sparingly. Another method too loosely coupling commands is using triggers to run commands when the state of the robot changes. One command could cause a state change which in turn causes a trigger to trip and run another command.
