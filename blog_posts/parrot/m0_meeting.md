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





