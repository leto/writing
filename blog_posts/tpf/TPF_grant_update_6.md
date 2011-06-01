# Grant Update

I am excited to announce that I have completed my next grant milestone!  I
recently increased test coverage of extend_vtable.c to at least 95.5%, achiving
the milestone with a half percent buffer. It definitely wasn't easy, but I
changed the way I was approaching writing tests and it resulted in a huge burst
of productivity.

I went through a test coverage report and wrote down, on an actual peice of
*paper*, every function that had no test coverage. This allowed me to circle
the functions that I thought would be easiest to write tests for, and quickly
got those out of the way. I then went for uncovered functions that were similar
to already covered functions, and then finally I got to the hard functions.

This was a fruitful exercise, because it was decided by Parrot developers that
some VTABLE functions escaped accidentally and were removed from the public API.
Whiteknight++ removed Parrot_PMC_destroy, which I was using incorrectly in the
extend_vtable tests and which was actually coredumping Parrot, but only on certain
platforms. I then asked
