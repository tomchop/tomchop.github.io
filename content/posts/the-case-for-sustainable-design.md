---
title: The case for sustainable design
date: 2023-04-22
summary:
  Lessons learned of taking up an open source project and reflections on bad
  design decisions.
tags:
  - open source
  - yeti
  - tool
draft: true
---

Yesterday, Seb and I
[released a brand new Alpha-2.0 version of Yeti](https://github.com/yeti-platform/yeti/releases/tag/2.0-Alpha),
the threat intel platform I initially publicly released in 2017. This first
release is the culmination of **a lot** of work, which doesn't necessarily show
in the amount of commits or the number of lines changed.

There are many more changes that we want to bring to the platform, but we'll
leave that to the main [Yeti blog](https://yeti-platform.github.io) (which we're
also migrating to Hugo... soon™️). This post will be more on reflections around
building and maintaining a free and open source project.

## The poisoned project

After I changed jobs, Yeti was not a part of my daily focus anymore. Lots of
things were exciting in my life and I didn't have as much time or energy to
allocate to it, nor a compelling reason to do so.

But much more importantly, I had unknowingly _shot myself in the foot_ by making
a series of choices that made popping the hood of that beast to fix even the
smallest of bugs seem like an unsurmountable task.

The rare times where I found sudden spurs of motivation to actually do some
work, I would open my editor and slowly remember every single piece of technical
debt or poor design choice that made the thing I wanted to do more complicated
than it should (or that I wanted it to) have been. Whole fields of yaks needing
to be shaved were standing between me and my goal. This made me drop the work
much more quickly than I usually would have, allowing for only minute progress
on the task at hand at a time. A friend of mine liked using the term "wall of
awful" to describe this feeling of emotional dread when facing a daunting task,
obliterating our executive function.

Imagine the scene of
[Hal fixing a lightbulb](https://www.youtube.com/watch?v=AbSehcT19u0), but with
much less enthusiasm.

## The road to hell is paved with good intentions

The exercise of revisiting a codebase written by a 7-year-younger version (and
less experienced) version of yourself is an interesting one.

- **Tight code coupling**: We wanted to move away from MongoDB to another
  storage system with a more flexible license. This should've been as simple as
  rewriting a couple of queries, but the API code was coupled so tightly with
  the database code that it was impossible to do the switch without giving the
  API code a whole makeover.

- **Implicit code / design**: We did come up with some elegant solutions every
  now and then. but most were left undocumented (implicit), and wouldn't make
  any sense when 'diving back' into the codebase. I'd think "it seems I did this
  for a reason, but why?", then try to fix a few things, just to realize a
  couple hours later that I had already been down that rabbit hole before
  (unsuccessfully).

- **Early over-optimization**: Some of our designs decisions weren't really
  justified. To factor some common code and stay DRY, we came up with an
  abstraction layer to create new API endpoints for each "object family". The
  design was quite elegant, but it made diving back into the codebase so much
  harder because of all the extra levels of indirection. To make things worse,
  the third-party library we used for this isn't maintained anymore, and has
  some even older dependencies, making Yeti impossible to upgrade to newer tech.

## Breaking the cycle

The main result of this is that I wasn't improving anything that was broken in
the tool, but rather chasing the next new shiny thing (in this case, re-writing
the whole UI).

It was hard to break this cycle, but Seb kept believing in the project and
ultimately convinced me to showcase our new UI at a conference. This gave me the
motivation that I needed to _finally_ start breaking the major blocks apart and
give me a fresh start where all that technical debt isn't that impactful.

For those who are thinking that I've fallen into the "let's redesign everything
from scratch" trap—you may be right. But at least I'm having much more fun
exploring modern technology stacks and see how much I've learned since I last
worked on Yeti that much. ¯\\\_(ツ)\_/¯

## Lessons learned

The biggest lesson I've learned here is to have a **sustainable coding /
designing approach**. Taking shortcuts will come back to bit me in the ass, even
almost a decade later. Now, I factor in how long is it going to take me to dive
back into this codebase, fix bugs in this function, add a new feature, if I
haven't been around for some time. In fact, I realized that after not touching a
codebase for extended periods of time, my level of familiarity with it is almost
indistinguishable from someone who's never seen it before (at least in the
subtle ways that matter).

Here are some lessons that I learned while revisiting this whole thing.

- **Perfect is the enemy of good**: Don't wait until every last piece of UI is
  completely polished to release your tool. Release early, iterate quickly, fix
  bugs as they come. That feature you spent two weeks implementing might not be
  as major as you think it is.
- **Explicit design**: Design your system and document your code like a
  sleep-deprived version of you is going to have to work on it. It's OK to
  repeat yourself to some degree if it means that things are clearer.
- **Be demanding with contributors**: Your project will eventually attract
  contributors, and any code they merge is now _your_ code to deal with until
  the end of times (or until you remove it from the codebase). If they implement
  larger / core features, make sure they are high-quality and make sure to push
  back on design decisions you don't agree with. _Don't accept the PR just
  because it was opened._
- **Be minimalist in your dependencies**: Just because a library that does
  something you need exists doesn't mean it's right for your project. If you can
  work around the feature they're providing with a 10-line function, then it may
  be worth to not add the bloat.
