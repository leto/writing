### What I wish I would have known about testing when I started...

When I first got involved in free and open source software, I had no clue what
tests were or why they were important. I had worked on some personal
programming projects before, but the first time I actually worked on a
project with others, i.e. got a commit bit, was Yacas, a computer algebra
system similar to Mathematica (Yet Another Computer Algebra System).

At this stage in my journey, tests were an afterthought. My general
meta-algorithm was to hack on code -> "see if it works" -> write a simple test
to show it works (optional).  If a test was challenging to write, it most
likely never got written.

This is the first step in the path to Test Driven Enlightment. You know tests
are probably a good idea, but one hasn't seen the benefit of them clearly, so
they are only written occasionally.

If I could open up a wormhole and tell my younger self one peice of wisdom
about testing, it would be:

    Some tests, in the long-run, are more important than the code they test.

A few people right about now may be thinking that I put on my tinfoil testing
hat when I sit down to code. How can tests be *more* important than the code
they test?  Tests are the proof that your code *actually* works, and they guide
you to writing correct code as well as providing the flexibility to change code
and know that features still work. The larger your codebase becomes, the more
valuable your tests are, because they allow you to change one part of your code
and still be sure that the rest of it works.

Another vital reason to write tests is because it indicates that something is
explicitly desirable, rather than an unintended side-effect or oversight.  If
you have a specification, you can use tests to verify that you meet it, which
is very valuable, and in some industries, necessary. A test is just like
"telling a story", where the story is how you think code should work.

Code either changes and evolves or bitrots. (Footnote: The term "bitrot" is
coder slang for the almost universal fact that if a piece of code doesn't
change but everything it relies on *does*, it "rots" and usually has very
little chance of working without modifications being made to accomodate newer
software and hardware.)

Very often, you will write tests once, but then totally refactor your
implementation or even rewrite it from scratch. Tests often outlive the code
they originally tested, i.e.  one set of tests can be used no matter how many
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
pass, you know that your code is better and it has one more feature or one
less bug.

It is easy to think "I want to add 50 features" and spend all day fiddling with
code, constantly switching between working on different things. Most of the
time, very little will be accomplished. Test Driven Enlightment guides one
to focus on making one test pass at a time.

If you have a single failing test, you are a mission to make it pass. It
focuses your brain on something very specific, which very often has better
results than switching between tasks constantly.

Most information about being test-driven is very specific to a language or
situation, but that doesn't need to be the case.  Here is the how to approach
adding a new feature or fixing a bug in any language:

    1) Write a test that fails, which you think will pass when the feature
    is implemented or bug is fixed. 
    
    Advanced: As you write the test, run it occasionally, even if it is not
    done yet, and guess the actual error message that the test will give. The
    more tests you write and run, the easier this will become.)

    2) Hack on the code.

    3) Run the test. If it passes, go to #4, otherwise go to #2.

    4) You are done! Do a happy dance :)

This method works for any kind of test and any language. If there is only
one thing about testing that you remember from this essay, let it be the steps above.

Here are some more general test-driven guidelines that will serve you well and apply
in almost any situation:

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

Many people start to get confused when people speak of "integration tests",
"unit tests", "acceptance tests" and many other flavors of tests. One shouldn't
worry too much about these terms. The more tests you write, the more nuances
you will see and the differences between tests will become more apparent. Every
one does not have the same definition for what these tests are, but the terms
are still useful to describe kinds of tests.

## Unit Tests vs. Integration Tests

Unit tests and integration tests form a spectrum. Unit tests test small bits of
code, and integration tests verify how more than one "unit" fit together.  The
test writer gets to decide what comprises a "unit" is, but most often it is at
the level of a "function" or "method", although some languages call those
things by different names.

To make this a little more concrete, we will give a basic analogy using functions.
Imagine that f(x) and g(x) are two functions which represent two "units" of code.
For concreteness, let's assume they represent two specific functions in your
favorite Free/Open Source projects' codebase.

An integration test asserts something like function composition, i.e. f(g(a)) =
b. An integration test is testing how multiple things "integrate" or work
together, instead of how a single part works individually. If algebra isn't
your thang, another way to look at it is unit tests only test one part of the
machine at a time, but integration tests very many parts work in unison. A
great example of an integration test is "test driving" a car. You aren't
checking the air pressure, or measuring voltage of the spark plugs. You are
making sure the vehicle works as a whole.

Most of the time it is good to have both. I often start with unit tests and add
integration tests as needed, since you will weed out the most basic bugs first,
then find more subtle bugs that are related to how peices don't quite fit
together, as opposed to the individual pieces not working. Many people write
integration tests first and then delve into unit tests. Which you write first
isn't nearly as important as writing both kinds.

