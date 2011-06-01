# Yet Another TPF Parrot Embed/Extend Grant Update

I am excited to announce that I have completed my next grant milestone!  I
recently increased test coverage of extend\_vtable.c to over 95% (
[95.5%](http://tapir2.ro.vutbr.cz/cover/latest-c_cover/src-extend_vtable-c.html) to be
exact), achieving the milestone with a half percent buffer. It definitely
wasn't easy, but I changed the way I was approaching writing tests and it
resulted in a [huge burst of
productivity](https://github.com/parrot/parrot/compare/5dd8c543ab...8c04cc3e66).

I went through a test coverage report and wrote down, on an actual piece of
*paper*, every function that had no test coverage. This allowed me to circle
the functions that I thought would be easiest to write tests for, and quickly
got those out of the way. I then went for uncovered functions that were similar
to already covered functions, and then finally I got to the hard functions.

This was a fruitful exercise, because it was decided by Parrot developers that
some VTABLE functions escaped accidentally and that they should be removed from the public API.
Whiteknight++ [removed Parrot\_PMC\_destroy (extra points for humor)](https://github.com/parrot/parrot/commit/cbfc76e64acf9f0a526b5f7da0e4c6c4ec0d1189), which I was using incorrectly in the
extend\_vtable tests and which was actually coredumping Parrot, but only on certain
platforms. I then removed [Parrot\_PMC\_mark](https://github.com/parrot/parrot/commit/cd1edef38c9f7d4af8ec3229fa166e4fe92d21f6) and [Parrot\_PMC\_invoke](https://github.com/parrot/parrot/commit/44a9634f2764ccccfd7a5cbad1552159fc73bff8), the first being
an implementation detail of the garbage collector, and Parrot\_PMC\_invoke because
it was the only function that returned a '''Parrot\_Opcode\_t*''' and basically
not fit for public consumption.

I also [created a ticket (TT#2126)](http://trac.parrot.org/parrot/ticket/2126)
for a bug in the Parrot\_PMC\_morph function, which
has some possibly buggy but definitely unspecified behavior.

The remaining, untested functions in extend\_vtable are clone\_pmc, cmp\_pmc,
get\_pointer\_keyed\_int, get\_pointer\_keyed\_str, remove\_vtable\_override,
set\_pointer\_keyed and set\_pointer\_keyed\_str. I leave the testing of these
functions as an exercise to the interested reader :)

## Grant Refactoring

This reminds me of a saying, I can't remember it exactly, but it is something
about the best laid plans of camels and butterflies often taste like onions.
Anyway, since I wrote my grant, the Parrot Embed API was deprecated and replaced
with a shinier and better documented system. After talking with cotto++ and
whiteknight++ on IRC, it was decided that working on test coverage for the new
embed API was a better use of resources than writing tests for the old embed
API that my original grant referred to, which will most likely be removed from
Parrot soon.

The new embed API is called [src/embed/api.c](https://github.com/parrot/parrot/blob/master/src/embed/api.c)
and the plan is to replace my grant milestone of 95% coverage of embed.c with 95% coverage
of embed/api.c, which is currently at [72%](http://tapir2.ro.vutbr.cz/cover/latest-c_cover/src-embed-api-c.html) coverage.

To summarize, I have two grant milestones left, increasing extend.c (currently
at [61%](http://tapir2.ro.vutbr.cz/cover/latest-c_cover/src-extend-c.html) )
and embed/api.c to 95% coverage.

Given the lessons learned from testing extend\_vtable and based on the fact that
I have already [made some
headway](https://github.com/parrot/parrot/commit/b59b869c9dd6f51109aa41e495082e09844ba348),
my new estimate for these milestones is three weeks each. To make this more
definite, I plan to be done with this grant work by July 15th.

This is the home stretch! I can feel it in my bones.
