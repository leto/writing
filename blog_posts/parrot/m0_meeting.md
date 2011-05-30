I met with fellow [Parrot](http://parrot.org) hackers
[allison++](http://allisonrandal.com), [cotto++](http://reparrot.blogspot.com)
and [chromatic++](http://www.wgz.org/~chromatic/) recently in Portland, OR (it was jokingly called YAPC::OR on
IRC) to talk about what we call
[M0](https://github.com/parrot/parrot/blob/m0-spec/docs/pdds/draft/pdd32_m0.pod).
M0 stands for "magic level 0" and it is a refactoring of Parrot internals in a
fundamental way.

cotto++ and I have been hacking on a detailed spec (over 35 pages now!) and a
["final prototype"](https://github.com/parrot/parrot/compare/master...m0-prototype)
in Perl 5 in the last few weeks. M0 is as "magic level 0",
which means it consists of the most basic building blocks of a virtual machine,
which the rest of the VM can be built with. The term "magic" means high-level
constructs and conveniences, such as objects, lexical variables, classes and
their associated syntax sugar. M0 is not meant to be written by humans, except
during bootstrapping. In the future, M0 will be probably be generated from
Parrot Intermediate Representation (PIR), Not Quite Perl 6 (NQP) or other High Level Languages (HLLs).

The most important reason for M0 is to correct the fact that too much of Parrot
internals are written in C. Parrot internals is constantly switching between
code written in PIR, other HLL's such as NQP and C. Many types of optimizations
go right out the window when you cross a language boundary. It is best for a
virtual machine to minimize crossing language boundaries if an efficient 
[JIT compiler](https://secure.wikimedia.org/wikipedia/en/wiki/Just-in-time_compilation)
is wanted, which we definitely desire. Since many hotpaths in Parrot
internals cross between PIR and C, they can't be inlined or optimized as much
as we would like.

A few years back, Parrot had a JIT compiler, from which many lessoned were
learned.  I am sure some people were frustrated when we removed it in 1.7.0 but
sometimes, it is best to start from a clean slate with many more lessons
learned under your belt. Our old JIT only worked on x86 architectures and
required maintaining a "JIT version" of every opcode on each architecture
supported.  Clearly, this method was not going to scale or be maintainable.

I will venture to say that M0 is the culmination of the lessons learned from
our failed JIT. I should note that "failure" does not have a negative
connotation in my mind. Indeed, only through failure are we truly learning. If
you do something absolutely perfectly, you aren't learning.

We are at an exciting time in Parrot's history, in that for a long time, we
wanted an elegant JIT, using all the latest spiffy techniques, but it was
always an abstract idea, "just over there", but not enough to grab a-hold of. A
new JIT that meets these goals absolutely requires something like M0, and is
the driving force for its design.  M0 will pave the way for an efficient JIT to
be implemented on Parrot.

M0 currently consists of under 40 opcodes from which (we wager) all the rest of
Parrot can be built upon. This is radically different from how Parrot currently
works, where all of the deepest internals of Parrot are written in heavily
macroized ANSI 89 C.

M0 has a source code, i.e. textual form and a bytecode form. chromatic++
brought up a good point at the beginning of the meeting about the bytecode file
containing a cryptographic hash of the bytecode. This will allow one to
distribute bytecode which can then be cryptographically verified by whoever
eventually runs the bytecode. This is a very "fun" application of cryptography
that I will be looking into further.

allison++ brought up some good questions about how merging bytecode files would
be done. We hadn't really thought about that, so it lead to some fruitful
conversation about how Parrot Bytecode (PBC) is currently merged, what it does
wrong, and how M0 can do it less wronger.

We then talked about what exactly a "Continuation" in M0 means, and tried to clear
up some definitions between what is actually meant by Context, State and Continuation.

chromatic++ also mentioned that an optional optimization for the garbage
collector (GC) would be for it to create a memory pool solely to store
Continuations, since they will be heavily used and many of them will be
short-lived and reference each other, so having them in a small confined memory
region will reduce cache misses. We are filing this under "good to know and we
will do that when we get there."

Next we turned to concurrency, including how we would emulate the various
concurrency models of the languages we want to support, such as Python's Global
Interpreter Lock (GIL).  We decided that M0 will totally ignorant of
concurrency concepts, since it is a "magical" concept that will be implemented
at a higher level. We have started to refer to the level above M0 as M1 and
everything above M0 as M1+.

allison++ also mentioned that many innovations and optimizations are possible in
storing isolated register sets for each Continuation (a.k.a call frame). This
area of Parrot may yield some interesting surprises and perhaps some
publishable findings.

We all agreed that M0 should be as ignorant about the GC as possible, but the
GC will most likely learn about M0 as optimizations are implemented. The
pluggability of our GC's were also talked about. allison++ raised the question
"Are pluggable GC's easier to maintain/implement if they are only pluggable at
compile-time?" Indeed, they probably are, but then we run into the issue that
our current "make fulltest" runs our test suite under different GC's, which
would require multiple compiles for a single test suite run. chromatic++ made a
suggestion that we could instead make GC's pluggable at link-time (which would
require a decent about of reorganization) which would still allow developers to
easily test different GC's without recompiling all of Parrot.  chromatic++'s
estimate is that removing runtime pluggability of GC's would result in an
across the board speed improvement of 5%.

This conversation then turned toward the fact that M0 bytecode might depend on
what GC was used when it was generated, i.e. the same M0 source code run under
two different GC's would generate two different bytecode representations. This
would happen if the M0 alloc() opcode assumes C calling conventions. This was
generally deemed distasteful, so our alloc() opcode will not "bake in C
assumptions", which is a good general principle, as well. This will be a fun
test to write.

allison++ brought up the fact that we may need a way to tell the GC "this is
allocated but uninitialized memory", a.k.a solve the "infant mortality"
problem.  chromatic++ suggested that we could add some kind of lifespan flag to
our alloc opcode (which currently has an arbitrary/unused argument, since all
M0 opcodes take 3 arguments for symmetry and performance reasons). This could
be as simple as hints that a variable is local or global, or a more detailed
delineation using bit flags.

It was also decided that we didn't need an invoke opcode and that invoke properly
belongs as a VTABLE method on invokables.

We also talked about the fact that register machines greatly benefit from
concentrating VM operations on either the caller or the callee side. Looking
for more references about this. It seems that the callee side seems to be
what we will try for, but I am not quite sure why.

We finally talked about calling conventions and decided that goto_chunk should
roughly be equivalent to a jmp (assembly unconditional jump to address) and
the invoke VTABLE would setup a return continuation (i.e. make a copy of the
program counter), do a goto_chunk, and let the callee handle the rest, such
as looking up a return continuation and invoking it.

After the main M0 meeting, cotto++, allison++ and I sat down at a coffee shop
and came up with a list of next actions for M0:

+ Write a recursive version of 'calculate the n-th Fibonacci number' in M0
+ Write a simple checksum algorithm in M0 (suggestions?)
+ Create a working PMC in M0
+ M0 disassembler
+ Create a "glossary brochure for [Github](https://github.com) cruisers"
+ Implement function calls and returns
+ Make sure each M0 opcode is tested via [Devel::Cover](http://search.cpan.org/dist/Devel-Cover/)
+ Convert the [M0 assembler](https://github.com/parrot/parrot/blob/m0-prototype/src/m0/m0_assembler.pl) to C
+ Convert the [M0 interpreter](https://github.com/parrot/parrot/blob/m0-prototype/src/m0/m0_interp.pl) to C
+ Link M0 into libparrot (no-op integration)

I have been talking to cotto++ on IRC while typing up these notes and we have
come to the conclusion that a ["bytecode verifier"](https://secure.wikimedia.org/wikipedia/en/wiki/Java_Virtual_Machine#Bytecode_verifier)
should also be put on that
list. A verifier is a utility that detects invalid bytecode and prevent attacks
via malicious bytecode. This is something that happens at runtime, where as a
bytecode checksum happens *before* runtime, or at the end of compile time.
They provide different kinds of insurance. The bytecode checksum feature will
be an instrinsic feature that is not optional, since it prevents Parrot from
running known-bad bytecode. But a bytecode verifier adds a significant amount
of overhead. This overhead is reasonable if you are running untrusted code, but
it is unreasonable when your are running trusted bytecode (i.e. bytecode that
you created), so the verifier will have an option to be turned off.

We obviously have a lot of fun stuff to work on, so if any of it sounds fun,
come ask cotto++ or me (dukeleto) on #parrot on irc://irc.parrot.org for some M0
stuff to do. We especially need help with writing tests and documentation.

There will be a Parrot hackathon at [YAPC::NA](http://www.yapc2011.us/yn2011/)
2 this year, where I am sure some M0-related hacking will be happening. If you
have never been to a hackathon before, I highly recommend them as a way to join
a project and/or community.  Meatspace is still the best medium for some things
:)
