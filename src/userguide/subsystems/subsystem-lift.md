# Lift Subsystem

A subsystem that is found on almost all FTC robots in most seasons is a linear slide, also known as a lift. Here, you will learn how to program your own lift Subsystem.

{{#tabs global="code" }}
{{#tab name="Kotlin" }}
## Step 1: Create your class

The first step to creating your Subsystem is setting up the structure for it. Subsystems should be created as `objects`, which are singleton classes. Here is the most basic structure, that can be copy+pasted to create all of your subsystems.

```kt
object MySubsystem: Subsystem() {

}
```

In this case, let's call our subsystem `Lift`.

## Step 2: Create your motor

Next, we need to set up a motor to power our lift. This is easy to do using the [`MotorEx` class](../referenceguide/motorex.md). Let's start by creating a variable to store our motor. It should be of type `MotorEx`. We're using `lateinit var` here because we can't initialize our motor until the OpMode has been initialized, since the hardware map isn't created until an OpMode has been initialized.

```kt
lateinit var motor: MotorEx
```

We also need a `Controller`, since we want to move our motor. Let's use a PID controller. I recommend using a P value of 0.005 to start, and leaving I and D at zero. You should come back and tune it later, though.

```kt
val controller = PIDController(PIDCoefficients(0.005, 0.0, 0.0))
```

Next, we need a name. This is the name specified in the hardwareMap, so that NextFTC can find the motor. I called mine `lift_motor`, so that's the name I'm using. Use whatever name you've set in your configuration:

```kt
val name = "lift_motor"
```

Finally, the last step to setting up a motor is creating the instance in the `initialize()` function. Let's do that now:

```kt
override fun initialize() {
    motor = MotorEx(name)
}
```

That's all you need to do to create a motor in NextFTC! To recap:
- Every motor needs to be stored in a variable
- Every motor needs a `Controller`
- Every motor needs a name
- Every motor must be instantiated in the `initialize` function.

We're not quite done, though. We still need to create our first commands!

## Step 3: Create commands

The last step when you create a Subsystem is to create the commands you'll be using. This process varies with each subsystem. Here, I'll walk you through creating three commands that each move the lift to a different height: `toLow`, `toMiddle`, and `toHigh`.

> [!TIP]
> It's recommended to create a variable to store each encoder position. However, that takes up more space, so I won't be doing that here.

To control our motor, we will be using the `RunToPosition` command. There are a few different ways to implement commands, but the cleanest and recommended way is using getter methods. We will create variables (of type `Command`) and return instances of classes whenever we reference those variables.

Let's create our first `RunToPosition` command:

```kt
val toLow: Command
        get() = RunToPosition(motor, // MOTOR TO MOVE
            0.0, // TARGET POSITION, IN TICKS
            controller, // CONTROLLER TO IMPLEMENT
            this) // IMPLEMENTED SUBSYSTEM
```

Note the last parameter: `subsystem`. This is what tells NextFTC which commands should be allowed to run at the same time. If it weren't set, `toLow` would be able to run at the same time as other commands that use the `Lift` subsystem -- so there would be multiple things fighting to set the motor's power. Generally, you just need to pass `this` as the subsystem -- there are exceptions with more complicated custom commands.

Pretty easy, right? Let's duplicate it and update our variable name and target position to create our other two commands:

```kt
val toMiddle: Command
        get() = RunToPosition(motor, // MOTOR TO MOVE
            500.0, // TARGET POSITION, IN TICKS
            controller, // CONTROLLER TO IMPLEMENT
            this) // IMPLEMENTED SUBSYSTEM

    val toHigh: Command
        get() = RunToPosition(motor, // MOTOR TO MOVE
            1200.0, // TARGET POSITION, IN TICKS
            controller, // CONTROLLER TO IMPLEMENT
            this) // IMPLEMENTED SUBSYSTEM
```

## Final result

That's it! You've created your first Subsystem! Here is the final result:

```kt
object Lift: Subsystem() {
    lateinit var motor: MotorEx

    val controller = PIDController(PIDCoefficients(0.005, 0.0, 0.0))

    val name = "lift_motor"

    val toLow: Command
        get() = RunToPosition(motor, // MOTOR TO MOVE
            0.0, // TARGET POSITION, IN TICKS
            controller, // CONTROLLER TO IMPLEMENT
            this) // IMPLEMENTED SUBSYSTEM

    val toMiddle: Command
        get() = RunToPosition(motor, // MOTOR TO MOVE
            500.0, // TARGET POSITION, IN TICKS
            controller, // CONTROLLER TO IMPLEMENT
            this) // IMPLEMENTED SUBSYSTEM

    val toHigh: Command
        get() = RunToPosition(motor, // MOTOR TO MOVE
            1200.0, // TARGET POSITION, IN TICKS
            controller, // CONTROLLER TO IMPLEMENT
            this) // IMPLEMENTED SUBSYSTEM

    override fun initialize() {
        motor = MotorEx(name)
    }
}
```
{{#endtab }}
{{#tab name="Java" }}
## Step 1: Create your class

The first step to creating your Subsystem is setting up the structure for it. There should only be one instance of each Subsystem class. To do this, there is a couple lines of boilerplate you will need to add to the top of every subsystem class you create. Here is the subsystem class template:

```java
public class MySubsystem extends Subsystem {
    // BOILERPLATE
    public static final MySubsystem INSTANCE = new MySubsystem();
    private MySubsystem() { }

    // USER CODE HERE
}
```

In this case, let's call our subsystem `Lift`.

## Step 2: Create your motor

Next, we need to set up a motor to power our lift. This is easy to do using the [`MotorEx` class](../referenceguide/motorex.md). Let's start by creating a variable to store our motor. It should be of type `MotorEx`. Note that we aren't setting the value immediately; this is because we don't have access to the hardware map until an OpMode is initialized.

```java
public MotorEx motor;
```

We also need a `Controller`, since we want to move our motor. Let's use a PID controller. I recommend using a P value of 0.005 to start, and leaving I and D at zero. You should come back and tune it later, though. 

```java
public PIDController controller = new PIDController(new PIDCoefficients(0.005, 0.0, 0.0));
```

Next, we need a name. This is the name specified in the hardwareMap, so that NextFTC can find the motor. I called mine `lift_motor`, so that's the name I'm using. Use whatever name you've set in your configuration:

```kt
public String name = "lift_motor";
```

Finally, the last step to setting up a motor is instantiating the motor in the `initialize()` function. Let's do that now:

```java
@Override
public void initialize() {
    motor = new MotorEx(name);
}
```

That's all you need to do to create a motor in NextFTC! To recap:
- Every motor needs to be stored in a variable
- Every motor needs a `Controller`
- Every motor needs a name
- Every motor must be instantiated in the `initialize` function.

We're not quite done, though. We still need to create our first commands!

## Step 3: Create commands

The last step when you create a Subsystem is to create the commands you'll be using. This process varies with each subsystem. Here, I'll walk you through creating three commands that each move the lift to a different height: `toLow`, `toMiddle`, and `toHigh`.

> [!TIP]
> It's recommended to create a variable to store each encoder position. However, that takes up more space, so I won't be doing that here.

To control our motor, we will be using the `RunToPosition` command. In Java, the best way to create commands is by making functions that return instances of command classes.

Let's create our first `RunToPosition` command:

```java
public Command toLow() {
    return new RunToPosition(motor, // MOTOR TO MOVE
            0.0, // TARGET POSITION, IN TICKS
            controller, // CONTROLLER TO IMPLEMENT
            this); // IMPLEMENTED SUBSYSTEM
}
```

Note the last parameter: `subsystem`. This is what tells NextFTC which commands should be allowed to run at the same time. If it weren't set, `toLow` would be able to run at the same time as other commands that use the `Lift` subsystem -- so there would be multiple things fighting to set the motor's power. Generally, you just need to pass `this` as the subsystem -- there are exceptions with more complicated custom commands.

Pretty easy, right? Let's duplicate it and update our function name and target position to create our other two commands:

```java
public Command toMiddle() {
    return new RunToPosition(motor, // MOTOR TO MOVE
            500.0, // TARGET POSITION, IN TICKS
            controller, // CONTROLLER TO IMPLEMENT
            this); // IMPLEMENTED SUBSYSTEM
}

public Command toHigh() {
    return new RunToPosition(motor, // MOTOR TO MOVE
            1200.0, // TARGET POSITION, IN TICKS
            controller, // CONTROLLER TO IMPLEMENT
            this); // IMPLEMENTED SUBSYSTEM
}
```

## Final result

That's it! You've created your first Subsystem! Here is the final result:

```java
public class Lift extends Subsystem {
    // BOILERPLATE
    public static final Lift INSTANCE = new Lift();
    private Lift() { }

    // USER CODE
    public MotorEx motor;

    public PIDController controller = new PIDController(new PIDCoefficients(0.005, 0.0, 0.0));

    public String name = "lift_motor";

    public Command toLow() {
        return new RunToPosition(motor, // MOTOR TO MOVE
                0.0, // TARGET POSITION, IN TICKS
                controller, // CONTROLLER TO IMPLEMENT
                this); // IMPLEMENTED SUBSYSTEM
    }

    public Command toMiddle() {
        return new RunToPosition(motor, // MOTOR TO MOVE
                500.0, // TARGET POSITION, IN TICKS
                controller, // CONTROLLER TO IMPLEMENT
                this); // IMPLEMENTED SUBSYSTEM
    }

    public Command toHigh() {
        return new RunToPosition(motor, // MOTOR TO MOVE
                1200.0, // TARGET POSITION, IN TICKS
                controller, // CONTROLLER TO IMPLEMENT
                this); // IMPLEMENTED SUBSYSTEM
    }
    
    @Override
    public void initialize() {
        motor = new MotorEx(name);
    }
}
```
{{#endtab }}
{{#endtabs }}