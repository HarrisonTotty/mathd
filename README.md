# mathd
Note: This project is still WIP (although it does work)!

## Summary
This project is partially a result of the construction of my latest Arch Linux build. I wanted to be able to run various Wolfram Language expressions from the command line without having to re-launch the kernel each time. 

## Setup & Execution
The daemon script requires the `pexpect` python library in order to function properly. Copy the `mathd` and `mathc` scripts into `/usr/bin/` (or another similar location in your `$PATH`) and execute `mathd start` to instantiate the Wolfram Kernel. After that, you can send expressions to the kernel like so:

    mathc 'Integrate[Sin[x], x]

If the expression returns a value, `mathc` will print the value to stdout. Otherwise, it will just exit. As you would expect, since all expressions are executed on the same Kernel instance, you are free to define functions and store values to symbols.

## How it works
Upon executing `mathd start`, the daemon script forks itself and launches a kernel under a `pexpect` container.
