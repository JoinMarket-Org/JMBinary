# JMBinary
binary distribution of joinmarket, for testing.

This is with the intention of making it easier for users to run joinmarket; but it is only one step towards one possible way of achieving that. For a Windows user particularly, setting up joinmarket to run at all is too difficult for most. However, the fact that the interface is entirely command line is another big drawback; if we did create a standalone joinmarket GUI, this would allow us to package it.

Instructions: Just download zip for this whole repo (or git clone), and go to your OS, and try executing the programs from within there. A reminder that joinmarket.cfg with default settings (incl. mainnet) is created if you don't have one already. The intention is that you shouldn't need any other dependencies, and it should work on a fresh machine (but not sure if that's true!).

For TAILS, choose debian32.

About Ubuntu 32 bit, I am not sure.

Windows is 32 bit executables, but it seemed to run fine on a fresh install of Win7. I have no idea about Win8,10.

If you're wondering why yield-generator executables are not here, it's because it doesn't make much sense to package them into a binary, at least not today, as you'll generally want to edit the python scripts yourself (for fees etc.). Not to mention changing your maker algorithm.

This is very raw and basic, the main purpose is that people could try it out and report back any issues. Testing has been rudimentary, so don't use it with money on the line.
