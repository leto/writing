# A Final TPF Parrot Embed/Extend Grant Update

## Really TLDR: The Parrot has landed.

It brings me great joy to announce that I have completed all milestones for my
[TPF](http://perlfoundation.org) grant regarding the Parrot [Embed/Extend subsystems](http://news.perlfoundation.org/2010/11/2010q4-grant-proposal-improve.html)!
Not only that, but all of my grant work was included in the most recent release of
Parrot, [3.5.0 "Menelaus"](http://parrot.org/news/2011/Parrot-3.5.0).

The actual TLDR of this update is "many tests were written, code coverage is
above [95%](http://tapir2.ro.vutbr.cz/cover/latest-c_cover/src-embed-api-c.html)
[for all](http://tapir2.ro.vutbr.cz/cover/latest-c_cover/src-extend-c.html)
[systems described]( http://tapir2.ro.vutbr.cz/cover/latest-c_cover/src-extend_vtable-c.html)
in the grant, docs were improved, many
Parrot Trac tickets were created and [many](http://leto.net/dukeleto.pl/2010/12/parrot-embed-grant-update-1.html) [a](http://leto.net/dukeleto.pl/2011/01/parrot-embed-grant-update-2.html) [blarg](http://leto.net/dukeleto.pl/2011/02/parrot-embed-grant-update-3-now-with-dragons.html) [toast](http://leto.net/perl/2011/04/parrot-embed-grant-update-4-the-journey-continues.html) [was](http://leto.net/dukeleto.pl/2011/04/parrot-embed-grant-update-5-zen-pebbles.html) [cooked](http://leto.net/dukeleto.pl/2011/05/parrot-embed-grant-update-6-still-hackin-less-slackin.html).

For those of you that have a thirst for knowledge unquenched (I know who you are),
you are welcome to pondiferously peruse the Impending Technical Details.

## The Deets

The last portion of this grant definitiely challenged me to think in new ways
about testing and I am now only beginning to reap the benefits. I was charged
with adding code coverage a few rarely-if-ever-used C functions in Parrot's
embed/exted subsystem, which allows you embed Parrot into other applications
and other funky stuff.

[Whiteknight++](http://whiteknight.github.com) greatly helped me write a test for [Parrot\_sub\_new\_from\_c\_func](https://github.com/parrot/parrot/blob/master/src/extend.c#L700) which
takes a C function and a string that describes the function signature of the C
function and returns a [NCI PMC](https://github.com/parrot/parrot/blob/master/src/pmc/nci.pmc), which can be invoked.

I also learned many lessons about code coverage during the final stage of this
grant, even though I thought I was at such a level of expertness that it would
be hard to learn drastically new and important perspectives on testing. This
pattern of thinking is always wrong.

### Lesson 1

Sometimes you are the underdog and you have to interpret the rules in a new way
in order to have a chance at winning. You need to be Ender Wiggins from [Ender's
Game](https://secure.wikimedia.org/wikipedia/en/wiki/Ender's_Game): continually inventing new tactics to keep a winning edge over the
competition.

I noticed that a large portion (about 80%) of the uncovered code in one file
was a macro that was copy-and-pasted into two places. I refactored this into a
single macro called [POP\_CONTEXT](https://github.com/parrot/parrot/blob/master/src/extend.c#L331), which reduced the total number of lines in the file by
roughly 10, while simultaneously decreased the number of uncoverd lines in the
file by ~20 lines, which had a combined effect of pushing the code coverage
over the necessary 95% mark.

This change definitely increases the maintainability and modularity of the
code, but it feels a bit like gaming the system. Nonetheless, it saved the day.

### Lesson 2

The simplest useful test that you are avoiding is the most valuable next test to
write, because it has the best ROI (Return on Investment, where investment is the
time it takes to write the test, and the return is having an automated way of
verifying that the feature works.

### Lesson 3

Software developers are very optimistic about time estimates. We forget about all
the possible things that could go wrong and often quote estimates on something
approaching "base case scenario". As a rule of thumb, I think all software developers
should think long and hard about a time estimate for a given project, write down
the estimate, then multiply that time estimate by pi for a REAL estimate.

I theorize that pi is the factor of time it takes to debug and write tests for
behavior of hard-to-recreate edge cases.

I originally thought my grant would take about 3 months, but it ended up taking about
9 or ten. QED.

Finally, I would like to thank my grant manager Makoto Nozaki for providing
lots of feedback, support and encouragement, as well as everyone else at the
The Perl Foundation for funding this grant.
