# Yet Another TPF Parrot Embed/Extend Grant Update

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
platforms. I then asked ...

The remaining, untested functions in extend_vtable are clone_pmc, cmp_pmc,
get_pointer_keyed_int, get_pointer_keyed_str, remove_vtable_override,
set_pointer_keyed and set_pointer_keyed_str. I leave the testing of these
funcitons as an exercise to the interested reader :)

## Grant Refactoring

This reminds me of a saying, I can't remember it exactly, but it is something
about the best laid plans of camels and butterflies often taste like onions.
Anyway, since I wrote my grant, the Parrot Embed API was deprecated and replaced
with a shinier and better documented system. After talking with cotto++ and
whiteknight++ on IRC, it was decided that working on test coverage for the new
embed API was a better use of resources than writing tests for the old embed
API that my original grant referred to, which will most likely be removed from
Parrot soon.
