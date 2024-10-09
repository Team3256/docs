# Tips & Tricks

A random assortment of tips and tricks that don't fit anywhere else, but are still useful.

## Regenerative Braking

Regenerative braking is a feature that is built-in by default on Kraken X60 and Falcon 500 (so any motors that use a TalonFX built-in controller).

Regenerative braking is a feature that allows the motor to act as a generator when the motor is trying to slow down. This is _very_ useful, given the fact that FRC Robots can use a lot of power.

Regenerative braking is enabled by default when braking in either StaticBrake or Coast mode, but you can deliberately create a "regenerative brake" using `VelocityTorqueCurrentFOC`. Your P will directly affect the amount of current going back into the motor, so start with a low P and work your way up. Ensure you have TorqueCurrentLimits set up to prevent the motor & hardware from being damaged.

Watch the supply voltage to ensure you don't _generate_ too much voltage, say around 14V or 15V

[Source - Discord](https://discord.com/channels/992980517769183314/1232197660358737921/1237259181678002218)

## VisualVM

See the [documentation](https://docs.wpilib.org/en/stable/docs/software/advanced-gradlerio/profiling-with-visualvm.html)

VisualVM should be used often when the robot is running to ensure robot code is running well.

## Supply vs Stator Current Limits

Supply current: Limiting the current that flows into the whole thing

Stator current: Limiting the current through the stator

A motor is a fancy buck regulator with a massive inductor, so its actually inducing a voltage change internally (sort of, its due to the way bemf interacts with the coils, but this is outside the scope of this discussion)

Because you cannot lose power, if your voltage drops due to running at 50% duty cycle, your current needs to double, in order to maintain power, so your stator current is now 2x supply current.

## Git - Avoiding Merge Conflicts

Not targeting [member], but just a PSA for all software members. if you don't want to accidentally create merge commits when you pull in someone's work while you have commits locally, you can

```bash
git config --global pull.rebase true
```

"now wait a minute" you may say. "REBASE??"

a git rebase simply means putting your commits on top of the previous commits. in this case, if there were any potential merge conflicts, git won't let you finish rebasing until you fix the conflicts.

it's equivalent to uncommitting your local commits, stashing them, pulling in new changes, and unstashing (and resolving the conflicts after the unstash). it's literally just less work
