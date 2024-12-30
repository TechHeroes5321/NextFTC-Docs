# Welcome to NextFTC

NextFTC is a simple but powerful library for FTC. It is a library for everyone, rookies and seasoned experts alike. It introduces command features similar to that of WPILib, the library used by nearly all FRC teams. With NextFTC, you can create cleaner, modular, and more efficient code.

## Features

-   **Easy to use:** NextFTC doesn't require you to write commands as classes, like you do in FTCLib.
-   **Gamepad binding:** Easy to bind commands to gamepad buttons
-   **Subsystems:** Subsystems help you organize your code. Additionally, they help you prevent problems by making sure that no two commands that use the same subsystem ever run at once.
-   **Commands:** Commands are units of code that can be executed. Each command is a bunch of tiny steps. Commands can be grouped into command groups, which allow commands to run together, either sequentially or at the same time.
-   **Premade Commands:** You almost never need to write your own commands, as there are dozens of commands already made for you. Examples are running a motor to a position using a custom PID controller, following a path, or driving during TeleOp.
-   **Pedro Pathing:** NextFTC has built in integration with [Pedro Pathing](https://pedropathing.com), an autonomous pathing library. Unlike Roadrunner, Pedro Pathing is faster, smoother, and easier to tune.

## A note on these docs

NextFTC was written in Kotlin, a JVM programming language. You can use either Kotlin or Java, but there are some small things in Kotlin that make your life slightly easier. Each section that gives code examples will have tabs for both Kotlin and Java. It is recommended to choose just one language for your project, to avoid having to figure out how to make your code compatible with itself.

If you will be using Kotlin, you must configure Kotlin in your project. I recommend using the Kotlin Gradle Plugin version `1.9.25`. (Kotlin comes preinstalled in the Quickstart)