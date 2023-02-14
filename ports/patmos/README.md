Patmos port
=
Patmos port of micropython, to build simply run:
`make`
To build it with a python REPL, open `mpconfigport.h` and set `MICROPY_HELPER_REPL` to `1` and run:
`make FROZEN=0`

In any case to run the build Patmos file type:
`make patmosRun`
