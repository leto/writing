I met with allison++, cotto++ and chromatic++ recently in Portland, OR (it was
jokingly called YAPC::OR on IRC) to talk about what we call M0. M0 stands for
"magic level 0" and it is a refactoring of Parrot internals in a fundamental
way.

cotto++ and I have been hacking on a detailed spec (over 35 pages now!) and a
"final prototype" in Perl 5 in the last few weeks. M0 is as "magic level 0",
which means it consists of the most basic building blocks of a virtual machine,
which the rest of the VM can be built with. The term "magic" means high-level
constructs and conveniences, such as objects, lexical variables, classes and
their associated syntax sugar. M0 is not meant to be written by humans, except
during bootstrapping. In the future, M0 will be probably be generated from
PIR/NQP or other HLL's.

The most important reason for M0 is to correct the fact tha too much of Parrot
internals are written in C. Parrot internals is constantly switching between
code written in PIR, other HLL's such as NQP and C. Many types of optimizations
go right out the window when you cross a language boundary. It is best for a
virtual machine to minimize crossing language boundaries if an efficient JIT
compiler is wanted, which we definitely desire. Since many hotpaths in Parrot
internals cross between PIR and C, they can't be inlined or optimized as much
as we would like.

A few years back, Parrot had a JIT compiler, from which many lessoned were learned.
I am sure some people were frustrated when we removed it in 1.7.0. but sometimes,
it is best to start from a clean slate with many more lessons learned under your
belt. I will venture to say that M0 is the culmination of the lessons learned from
our failed JIT. I should note that "failure" does not have a negative connotation
in my mind. Indeed, only through failure are we truly learning. If you do something
absolutely perfectly, you aren't learning.

We are at an exciting time in Parrot's history, in that for a long time, we wanted
an elegant JIT, using all the latest spiffy techniques, but it was always an abstract
idea, "just over there", but not enough to grab ahold of. A new JIT that meets these
goals absolutely requires something like M0, and is the driving force for its design.
M0 will pave the way for an efficient JIT to be implemented on Parrot.

M0 currently consists of under 40 opcodes from which (we wager) all the rest of Parrot
can be built upon. This is radically different from how Parrot currently works, where
all of the deepest internals of Parrot are written in heavily macroized ANSI 89 C.

M0 has a source code, i.e. textual form and a bytecode form. chromatic++ brought up
a good point at the beginning of the meeting about the bytecode file containing a
cryptographic hash of the bytecode. This will allow one to distribute bytecode which
can then be cryptographically verified by whoever eventually runs the bytecode. This
is a very "fun" application of cryptography that I will be looking into further.

allison++ brought up some good questions about how merging bytecode files would be
done. We hadn't really thought about that, so it lead to some fruitful conversation
about how PBC is currently merged, what it does wrong, and how M0 can do it less
wronger.

We then talked about what exactly a "Continuation" in M0 means, and tried to clear
up some definitions between what is actually meant by Context, State and Continuation.

chromatic++ also mentioned that an optional optimization for the GC would be
for it to create a memory pool specificically to store Continuations, since
they will be heavily used and many of them will be short-lived. We are filing
this under "good to know and we will do that when we get there."

Next we turned to concurrency, including how we would emulate the various concurrency
models of the languages we want to support, such as Python's GIL. We decided that M0
will totally ignorant of concurrency concepts, since it is a "magical" concept that
will be implemented at a higher level. We have started to refer to the level above
M0 as M1 and everything above M0 as M1+.

Allison also mentioned that their were many innovations and optimizations possible
in storing isolated register sets for each Continuation (a.k.a call frame). This
area of Parrot may yield some interesting surprises and perhaps some publishable
findings.

We all agreed that M0 should be as ignorant about the GC as possible, but the GC
will most likely learn about M0 as optimizations are implemented. The pluggability
of our GC's were also talked about. allison++ raised the question "Are pluggable
GC's easier to maintain/implement if they are only pluggable at compile-time?" Indeed,
they probably are, but then we run into the issue that our current "make fulltest"
runs our test suite under differnt GC's, which would require multiple compiles for a
single test suite. chromatic++ made a suggestion that we could instead make GC's
pluggable at link-time (which would require a decent about of reorganization) which
would still allow devs to easily test different GC's without recompiling all of Parrot.
chromatic++'s estimate is that removing runtime pluggability of GC's would result
in an across the board speed improvement of 5%.

This conversation then turned toward the fact that M0 bytecode might depend on what
GC was used when it was generated, because of 

