I met with allison++, cotto++ and chromatic++ recently in Portland, OR (it was
jokingly called YAPC::OR on IRC) to talk about what we call M0. M0 stands for
"magic level 0" and it is a refactoring of Parrot internals in a fundamental way.

cotto++ and I have been hacking on a detailed spec (over 35 pages now!) and a
"final prototype" in Perl 5 in the last few weeks. M0 is as "magic level 0",
which means it consists of the most basic building blocks of a virtual machine,
which the rest of the VM can be built with. The term "magic" means high-level
constructs and conveniences, such as objects, lexical variables, classes and their
associated syntax sugar. M0 is not meant to be written by humans, except during
bootstrapping. In the future, M0 will be generated from 
