### What I wish I would have known about testing when I started...

When I first got involved in free and open source software, I had no clue what
tests were or why they were important. I had worked on some personal
programming projects before, but the first time I was actually working on a
project with others, i.e. got a commit bit, was Yacas, a computer algebra
system similar to Mathematica.

At this stage in my journey, tests were an afterthought. My general
meta-algorithm was to hack on code -> "see if it works" -> write a simple test
to show it works (optional).  If a test was challenging to write, it most
likely never got written.

This is the first step in the path to Test Driven Enlightment.

If I could open up a wormhole and tell my younger self one peice of wisdom
about testing, it would be:

    Tests are the proof that your code *actually* works, and they guide you to writing correct
    code as well as providing the flexibility to change code and know that features still work.

    Some tests, in the long-run, are more important than the code they test.

A few people right about now may be thinking that I put on my tinfoil testing
hat when I sit down to code. How can tests be *more* important than the code
they test?

Code either changes and evolves or bitrots. (Footnote: The term "bitrot" is
coder slang for the almost universal fact that if a piece of code doesn't
change but everything it relies on *does*, it "rots" and usually has very
little chance of working without modifications being made to accomodate newer
software and hardware.)

Very often, you will write tests once, but then totally refactor your
implementation or even rewrite it from scratch. Tests often outlive the code
they originally tested, i.e.  once set of tests can be used no matter how many
times your code is refactored. They are actually the litmus test that allows
you to throw away an old implementation and say "this newer implementation has
a much better design and passes our test suite." I have seen this happen many
times in the Perl and Parrot communities, where you can often find me.

Tests allow you to change things quickly and know if something is broken. They
are like jetpacks for devevlopers.

Carpenters have a bit of sage wisdom that goes like this:

    Measure twice, cut once.

Coding is like cutting and tests are like measuring.

Test Driven Enlightenment saves an enormous amount of time, because instead of
flailing around, fiddling with code, not having a direction, tests hone your
focus.

Tests also are very good positive feedback. Every time you make a new test
pass, you know that your code is better.

It is easy to think "I want to add 50 features" and spend all day fiddling with
code, constantly switching between working on different things. Most of the
time, very little will be accomplished.

But if you have a single failing test, you are a mission to make it pass. It
focuses your brain on something very specific, which very often has better
results than switching between tasks constantly.

Most information about being test-driven is very specific to a language or
situation, but that doesn't need to be the case. Here are some general
test-driven guidelines that will serve you well and apply in almost any
situation:

1) Understand the difference between what is being tested and what is being
used as a tool to test something else.

2) Fragile tests. You could write a test that makes sure an error message is
exactly correct. But what happens when the error message changes? What happens
when someone internationalizes your code to Catalan? What happens when someone
runs your code on an OS you've never heard of? The more resilient your test is,
the more valuable it will be.

Think about these things when you write tests. You want them to be resilient,
i.e.  tests, for the most part, should only have to change when functionality
changes. If you have to change your tests often, but functionality is not
changing, you are doing something wrong.

## Kinds of tests

Spectrum from Unit tests to Integration tests

The defintion of each change as you change the definition of unit.

Unit tests work on one "unit" of code.

Integration tests work on multiple units of code.

Unit test asserts that when f(x) is given a, it produces b, i.e f(a) = b.

An integration test asserts something similar to, f(g(a)) = b. An integration
test is testing how multiple things "integrate" or work together, instead of
how a single part works individually.

Most of the time it is good to have both. I often start with unit tests and add
integration tests as needed, since you will weed out the most basic bugs first,
then find more subtle bugs that are related to how peices don't quite fit
together, as opposed to the individual peices not working. Many people write
integration tests first and then delve into unit tests. Which you write first
isn't nearly as important as writing some of both.
