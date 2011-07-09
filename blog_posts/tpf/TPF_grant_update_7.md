# A Final TPF Parrot Embed/Extend Grant Update

## Really TLDR: The Parrot has landed.

It brings me great joy to announce that I have completed all milestones for my
[TPF](http://perlfoundation.org) [grant regarding the Parrot Embed/Extend subsystems]()!

The actual TLDR of this update is "many tests were written, code coverage is
above 95% for all systems described in the grant, docs were improved, many
Parrot Trac tickets were created and many a blarg toast was cooked.

For those of you that have a thirst for knowledge unquenched (I know who you are),
you are welcome to pondiferously peruse the Impending Technical Details.

## The Deets

The last portion of this grant definitiely challenged me to think in new ways
about testing and I am now only beginning to reap the benefits. I was charged
with adding code coverage a few rarely-if-ever-used C functions in Parrot's
embed/exted subsystem, which allows you embed Parrot into other applications
and other funky stuff.

Whiteknight++ greatly helped me write a test for Parrot_sub_from_c XXX which
does YYY.

I also learned many lessons about code coverage during the final stage of this
grant, even though I thought I was at such a level of expertness that it would
be hard to learn drastically new and important perspectives on testing. This
pattern of thinking is always wrong.

### Lesson 1

Sometimes you are the underdog and you have to interpret the rules in a new way
in order to have a chance at winning. You need to be Ender Wiggins from Ender's
Game: continually inventing new tactics to keep a winning edge over the
competition.

I noticed that a large portion (about 80%) of the uncovered code in one file
was a macro that was copy-and-pasted into two places. I refactored this into a
single macro called XXX, which reduced the total number of lines in the file by
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

Finally, I would like to thank my grant manager Makamoto XXXX for providing
lots of feedback, support and encouragement, as well as everyone else at the
The Perl Foundation for funding this grant.
