# mathd

## Summary
This project is partially a result of the construction of my latest Arch Linux build. I wanted to be able to run various Wolfram Language expressions from the command line without having to re-launch the kernel each time. 

## Setup & Execution
The daemon script requires the `pexpect` python library in order to function properly. Copy the `mathd` and `mathc` scripts into `/usr/bin/` (or another similar location in your `$PATH`) and execute `mathd start` to instantiate the Wolfram Kernel. After that, you can send expressions to the kernel like so:

    mathc 'Integrate[Sin[x], x]'

If the expression returns a value, `mathc` will print the value to stdout. Otherwise, it will just exit. As you would expect, since all expressions are executed on the same Kernel instance, you are free to define functions and store values to symbols.

## Arguments
`mathd` supports a number of handy command line arguments. Here they are described in detail:

    Usage: mathd [OPTIONS] {start,stop,status}
     
     
    Positional Arguments:
    {start,stop,status}     : Whether to start, stop, or check the status of the mathd daemon.
     
     
    Optional Arguments:
    -h, --help              : Prints an argument/usage guide and exits.
     
    -e EXPR, --eval EXPR    : Evaluates the specified expression after the Wolfram kernel is finished initializing.
     
    --history-length        : Sets the $HistoryLength of the initialized Wolfram kernel to the specified value.
                            : Default -> "Infinity"
     
    -t SEC, --timeout SEC   : Sets the evaluation timeout, in seconds.
                            : Default -> "300"
     
    --log-evals             : Enables logging of all In/Out data.
     
    --enable-graphics       : Enables graphical output via JavaGraphics.


## How it works
Upon executing `mathd start`, the daemon script forks itself and launches a Wolfram Kernel, with which it communicates using the `pexpect` library. The daemon script then waits for input by attaching itself to a FIFO (`~/.cache/mathd/pipe.in`). When an input arrives, it forwards the incoming expression to the kernel process for evaluation and then back to another FIFO (`~/.cache/mathd/pipe.out`) when the result it received. The `mathc` script simply reads and writes to these FIFO files.

## Changelog

    05/03/17 - Added multiline output support.
    05/11/17 - Added initial evaluations, history length setting, In/Out clearing, 
               timeout specification, In/Out logging, and graphics support.
