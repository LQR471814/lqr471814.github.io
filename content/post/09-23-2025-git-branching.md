---
title: Git Branching
author: LQR471814
date: 2025-09-23
categories:
- tutorials
tags:
- programming
---

I think git branches are a concept that can be easily
misunderstood by people just getting into programming or version
control.

I know for a fact that I misunderstood and subsequently misused it
(I once used it to store multiple projects in a single repo...)

So I think it would be good to create a short introduction to what
branches are and why they exist.

Let's start with some basics.

### Branching

Every single version of the software that you store in version
control is called a "commit" in git.

*Branches* do not store version data, they are actually just a
label for a particular commit.

When you *commit to a branch*, what you are doing is creating a
new commit and updating the given branch to point to the new
commit.

When you *create a new branch*, you are simply creating a new
label pointing to the current commit you are on.

Now what makes branches powerful is the ability to *merge*.

Merging is an operation that takes in: 1) the current branch 2) an
"other" branch and uses fancy heuristics to consolidate changes
introduced in the "other" branch into that of the current.

Sometimes this process will have conflicts:

- Current branch: foo.c = `printf("hello\n");`
- Other branch: foo.c = `char* ptr = malloc(12); free(ptr);`

You cannot heuristically (without understanding the context)
resolve this conflict.

In this case, the programmer would have to decide which change (or
combine both) the final *merged* commit should contain.

### Why branches?

To understand why branches exist, consider a simple thought
experiment.

We have 3 people working a single codebase, each are tasked with
implementing a certain feature, no features require overlapping
changes.

The initial commit is commit is given id `I`.

Person 1 finishes first, they add their commit to the version
control history, their commit is given id `P1`.

Person 2 finishes later, they haven't realized person 1 has added
their commit to the version history and add their commit `P2`
right after `I`.

They attempt to push and are rejected, there are two divergent
version control histories.

The solution now is to rebase, to rewrite their own history so
that their commit comes after person 1's.

This is something that does happen and is acceptable under certain
circumstances (usually the rule is to only rebase locally), but
comes with limitations, consider the following:

Person 1 finishes first, their commit history is:

- `I` at time `t0`
- `P1.1` at time `t1`
- `P1.2` at time `t3`
- `P1.3` at time `t5`

Person 2 finishes second, their commit history is:

- `I` at time `t0`
- `P2.1` at time `t2`
- `P2.2` at time `t4`
- `P2.3` at time `t6`

Person 1 commits first to the git server.

Person 2 attempts to commit but is rejected, they have a divergent
history (divergent since `t0`).

Person 2 can rebase such that the history looks like this:

- `I` at time `t0`
- `P1.1` at time `t1`
- `P2.1` at time `t2`
- `P1.2` at time `t3`
- `P2.2` at time `t4`
- `P1.3` at time `t5`
- `P2.3` at time `t6`

Though they would need to `git push --force` to rewrite history
for everyone else to a form that includes their changes.

Now consider how the history would fluctuate when we add person 3
into the mix or bring in another 10.

For every developer making changes in parallel on the project, a
new divergent history is created. This would require everyone to
rebase every time they push and pull.

Now suppose person 1 wants to help out person 2 on their changes
before they push.

Person 1 now needs to figure out *some method* of getting person
2's changes without they having to `git push --force` and rewrite
history for everyone while having broken changes.

If the other developers are not notified of the newly added
incomplete changes, they may pull and find a nasty surprise.

The moral of the story is that there needs to be a clear way to
separate people's changes to the codebase, not everyone needs to
know about everyone else's changes.

This is why branches exist.

Though branches can still be quite flexible and chaotic if their
usage is not standardized, so many organizations adopt a
"branching model" like [git-flow](https://nvie.com/posts/a-successful-git-branching-model/).

