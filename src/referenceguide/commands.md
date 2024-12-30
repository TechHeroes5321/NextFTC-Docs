# Commands

*Why use commands?* Commands allow you to **organize your code** much more efficiently than you could otherwise. They are an excellent alternative to *finite state machines,* but are a lot easier to create and modify, and can reach higher levels of complexity than a state machine can. 

## Parts of a Command

A command has four components: `isDone`, `start`, `update`, and `stop`.
- `isDone` is checked every loop. If it ever evaluates to `true`, the command will stop running.
- `start` is run once, when the command is scheduled. It is used for setting up starting states and doing other things that should only happen once.
- `update` runs every loop, many times per second. Because of this, it is crucial that it never takes more than a trivial amount of time to execute. You should be extremely careful of looping or doing anything else that could take significant amounts of time
- `stop` runs once when the command ends, and recieves a parameter of whether or not it was interrupted by a different command.
- `interruptible` determines whether or not the command is able to be interrupted. A command is interrupted when another command is scheduled that requires a subsystem the command is using. If a command is not interruptable, then the new command will not run.
- `subsystems` is a set of all the subsystems a command uses. This is used for determing when two commands requrie the same subsystem. This is passed to the constructor of most premade commands.
