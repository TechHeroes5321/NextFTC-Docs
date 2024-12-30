# Claw Subsystem

Another common subsystem in FTC is a claw. Generally, a claw is powered by one servo, generally with an `open` and a `closed` position.

This will assume you have already read the [Lift Subsystem](./subsystem-lift.md) guide. Let's get started!

{{#tabs global="code" }}
{{#tab name="Kotlin" }}

## Step 1: Create your class

Just like with the Lift Subsystem, we need to start by creating our object:

```kt
object Claw: Subsystem() {

}
```

## Step 2: Create your servo

Now, since we're using a servo, instead of a motor, let's create a servo variable. Just like our motor variable from the lift subystem, it needs to be a `lateinit` variable. Currently, there is no wrapper class for Servos, so you will be using the Qualcomm servo class.

```kt
lateinit var servo: Servo
```

Just like our motors, we also need a name for our servo. This is the name specified in the hardwareMap. I called mine `claw_servo`:

```kt
val name = "claw_servo"
```

Finally, we need to initialize our servo in the `initialize()` function. Because there is no wrapper class, you will need to access the hardwareMap yourself. The easiest way to do this is by using `OpModeData.hardwareMap`. NextFTC will automatically set the hardwareMap in each of your OpModes (as long as you extend `NextFTCOpMode` or `PedroOpMode`):

```kt
override fun initialize() {
    servo = OpModeData.hardwareMap.get(Servo::class.java, name)
}
```

To recap how you create servo-based Subsystems in NextFTC:
- Create a variable to store your Servo instance
- Create a variable to store the name
- In `initialize()`, get the Servo instance from the hardwareMap using your name variable.

## Step 3: Create commands

Programming servo commands is very easy in NextFTC. Just like your Lift, you will be creating variables that return instances of Commands. 

> [!TIP]
> It's recommended to create a variable to store each servo position. However, that takes up more space, so I won't be doing that here.

For servos, the command you will be using is `ServoToPosition`. You will pass your servo, a target position, and your subsystem (just like the Lift):

```kt
val open: Command
    get() = ServoToPosition(servo, // SERVO TO MOVE
        0.1, // POSITION TO MOVE TO
        this)  // IMPLEMENTED SUBSYSTEM
```

Nice! Let's do the same with the `close` command:

```kt
val close: Command
    get() = ServoToPosition(servo, // SERVO TO MOVE
        0.2, // POSITION TO MOVE TO
        this) // IMPLEMENTED SUBSYSTEM
```

## Final result

You've successfully created your claw subsystem! Here's the final result:

```kt
object Claw: Subsystem() {
    lateinit var servo: Servo

    val name = "claw_servo"

    val open: Command
        get() = ServoToPosition(servo, // SERVO TO MOVE
            0.9, // POSITION TO MOVE TO
            this)  // IMPLEMENTED SUBSYSTEM

    val close: Command
        get() = ServoToPosition(servo, // SERVO TO MOVE
            0.2, // POSITION TO MOVE TO
            this) // IMPLEMENTED SUBSYSTEM

    override fun initialize() {
        servo = OpModeData.hardwareMap.get(Servo::class.java, name)
    }
}
```

{{#endtab }}
{{#tab name="Java" }}

## Step 1: Create your class

Just like with the Lift Subsystem, we need to start by creating our class. Remember the boilerplate!

```java
public class Claw extends Subsystem {
    // BOILERPLATE
    public static final Claw INSTANCE = new Claw();
    private Claw() { }

    // USER CODE
    
}
```

## Step 2: Create your servo

Now, since we're using a servo, instead of a motor, let's create a servo variable. Just like our motor variable from the lift subystem, we can't initialize it right away. Currently, there is no wrapper class for Servos, so you will be using the Qualcomm servo class.

```java
public Servo servo;
```

Just like our motors, we also need a name for our servo. This is the name specified in the hardwareMap. I called mine `claw_servo`:

```java
public String name = "claw_servo";
```

Finally, we need to initialize our servo in the `initialize()` function. Because there is no wrapper class, you will need to access the hardwareMap yourself. The easiest way to do this is by using `OpModeData.INSTANCE.getHardwareMap()`. NextFTC will automatically give you the active hardwareMap from your running OpMode (as long as you extend `NextFTCOpMode` or `PedroOpMode`):

```java
@Override
public void initialize() {
    servo = OpModeData.INSTANCE.getHardwareMap().get(Servo.class, name);
}
```

To recap how you create servo-based Subsystems in NextFTC:
- Create a variable to store your Servo instance
- Create a variable to store the name
- In `initialize()`, get the Servo instance from the hardwareMap using your name variable.

## Step 3: Create commands

Programming servo commands is very easy in NextFTC. Just like your Lift, you will be creating functions that return instances of Commands. 

> [!TIP]
> It's recommended to create a variable to store each servo position. However, that takes up more space, so I won't be doing that here.

For servos, the command you will be using is `ServoToPosition`. You will pass your servo, a target position, and your subsystem (just like the Lift):

```java
public Command open() {
    return new ServoToPosition(servo, // SERVO TO MOVE
            0.9, // POSITION TO MOVE TO
            this); // IMPLEMENTED SUBSYSTEM
}
```

Nice! Let's do the same with the `close` command:

```java
public Command close() {
    return new ServoToPosition(servo, // SERVO TO MOVE
            0.2, // POSITION TO MOVE TO
            this); // IMPLEMENTED SUBSYSTEM
}
```

## Final result

You've successfully created your claw subsystem! Here's the final result:

```java
public class Claw extends Subsystem {
    // BOILERPLATE
    public static final Claw INSTANCE = new Claw();
    private Claw() { }

    // USER CODE
    public Servo servo;
    
    public String name = "claw_servo";

    public Command open() {
        return new ServoToPosition(servo, // SERVO TO MOVE
                0.9, // POSITION TO MOVE TO
                this); // IMPLEMENTED SUBSYSTEM
    }

    public Command close() {
        return new ServoToPosition(servo, // SERVO TO MOVE
                0.2, // POSITION TO MOVE TO
                this); // IMPLEMENTED SUBSYSTEM
    }

    @Override
    public void initialize() {
        servo = OpModeData.INSTANCE.getHardwareMap().get(Servo.class, name);
    }
}
```

{{#endtab }}
{{#endtabs }}