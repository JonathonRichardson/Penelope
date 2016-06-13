# Penelope
[MUMPS](http://71.174.62.16/Demo/AnnoStd) implementation written in Rust, that is meant to run on Gringotts DB.

## Etymology
Penelope, as an implementation of MUMPS (the programming language), is named after "Penelope", the name of the manifestation of the Mumps (the disease) on the neck of Captain Holt in "9 Days", an episode of Brooklyn 99 (S03-E12).

## Project Plan
Most MUMPS implementations can both run in both a compiled and interpretted context.  Initially, I only plan to write an interpretter, and graduate later to a compiler.  This is due to the fact that MUMPS supports a GOTO command, which can jump into any routine/subroutine/function at any line start, which will require some thought on my part as to how to efficiently compile the code, while allowing that sort of invasion.  More so, since the GOTO can be executed from any other routine, including an interactive prompt, there is no way to "outsmart" it by detecting all possible jump in points.

I'm developing **mmpmgm** in parallel with Penelope, which I'll also be testing on GT.M to ensure compliance with the standard.

MUMPS is rather large, so I plan to implement it in increments, culminating in a version 1.0.0, that will have full compliance (but not necessarily be very optimized, and won't include a compiler).  I plan to implement it in the following waves:

* **0.0.1:** Support global variable commands **SET**, **MERGE**, **WRITE**, and **KILL**.
* **0.0.5:** Support MUMPS operators: 
   * _  + - * / \ # **  = < > ] [ ]] ' & ! and ().
* **0.1.0:** Support local variable commands **S/M/W/K**, as well as **NEW**.
   * Support special variables, such as **$SYSTEM**, and **$HOROLOG**
* **0.2.0:** Support stack/branch commands **DO**, **QUIT**, **FOR**, **GOTO**, **HALT**, **IF**, **ELSE**.
   * This includes special variables $TEST, $QUIT, and $STACK.
   * This includes being able to support $$fn() calls.
* **0.3.0:** Support IO commands OPEN, USE, CLOSE, READ, WRITE.
  * This includes special variables $KEY, $DEVICE, $X, $Y, $IO, $PRINCIPLE and $DEVICE
* **0.4.0:** Support utility command HANG
   * Due to the relatively easy implementation of HANG, this will also be the bulk native function implementation
      * $ASCII(), $CHAR(), $DATA(), $EXTRACT(), $FIND(), $FNUMBER(), $GET(), $JUSTIFY(), $LENGTH(), $NAME(), $PIECE(), $QLENGTH(), $QSUBSCRIPT(), $QUERY(), $RANDOM(), $REVERSE(), $SELECT(), $STACK(), $TEXT(), $TRANSLATE()
      * Basically, everything but $VIEW() (for which the standard is very loose, so implementation is not as necessary for M compliance) and $ORDER(), since that will need to be implemented in Gringotts first.
* **0.5.0:** Support **BREAK**
* **0.6.0:** Support **JOB** and **XECUTE**
   * This includes special variable $JOB
* **0.7.0:** Support VIEW
   * This will include $VIEW().
   * The standard is pretty vague about what this means.

Still undetermined are when the following will be implemented:
* Error handling (including $ECODE, $ESTACK, and $ETRAP) 
* $STORAGE
* ^$GLOBAL, ^$JOB, ^$LOCK, ^$ROUTINE, ^$SYSTEM global system variables
* LOCK command and subsystem.
* Indirection: @
* Pattern matching
* Postconditionals

There are also other aspects to the standard that I have not laid out a plan for, that I will likely tackle in 0.8 to 1.0, including, but no limited to, Event Processing, Transactions, Character Sets/Collations, future features/reserves.  The standard is a large beast, but a lot of it is essentially ignored by today's M programmer, in my experience, which is why I choose to tackle it in order of how commonly used features are.  Features that I have never heard of or have never used, or whose definitions in the standard are so vague as to make their use risky ($VIEW/VIEW command) are saved for the end.
