---
layout: post
title: Integrated and distributed issue tracking
---
People are tending to realize now that version control and issue
tracking are more closely related.  Distributed version control seems to
win over a centralized one, but issue tracking still works in a
centralized model.  There are two typical questions we want to answer
 - which issue was fixed by commit X
 - which commit(s) fixed issue Y

You can dismiss them saying, just include the issue id in each commit
message.  This is what some integrated applications say, too, [Trac][trac],
for example.  But it becomes more difficult if we have different
development branches.  Suppose, we just released version 1.0 without
fixing the Big Annoying Bug.  Now, having more time, we fixed it in the
1.1 branch and change its status to resolved.  A new developer on the
old branch checks the central bug tracker, and expects that Big Bug to
be gone, only to find out it's gone only in the new branch.

You can say, no problem, have a field in the bug tracker to record in
which version it was fixed.  This approach forces you to break the DRY
principle, and enter the version information already stored in your VCS.
Furthermore, it doesn't eliminate discrepancies in minor ad-hoc
branches.

The stance [Fossil][fossil] takes is to store both commits, and tickets in
the same repository (actually, an sqlite database), but handle them
separately.  If you change a ticket, no commit is required, the source
remains unchanged.  The other approach used [Ditz][ditz], [B][b],
[BugsEverywhere][be], or [Artemis][artemis] is to store them as simple
files in the source tree and let the VCS handle them.  They use
different text formats (YAML, plain text, mailbox) but the idea is the
same, if you add a ticket, a new text file is generated somewhere in a
`.bugs` directory.  It gets added to version control, but you have to
commit it.  When you update a branch, the issues in it will show
their real state, Big Bug is open 1.0, resolved in 1.1.

I think it's one step closer, but it's not really integrated yet.  Here
is my list of assumptions about and expectations from an integrated
issue tracker,
 - Each commit belongs to an issue, there is no commit just by itself.
 - An issue may have more commits that contributed to its resolution.
 - There may special, "maintenance" issues that never get closed, like
   "Clean up indentation" or "Re-organize modules to simplify things".
 - Adding a new ticket should be commited by itself, not mixed with code
   changes.
 - If more tickets are changed (e.g. by a tester), they should be
   commited separately, not mixed with code changes.
 - It should be easy to just fix a ticket, and an automatic commit
   message gets generated. 
 - Whenever it can be inferred from a commit which issue it contributed
   to (because the issue was changed, too), the issue id should be
   automatically appended to the commit message.
 - It should be easy to answer the two basic questions mentioned above.
 - Added bonus if it can communicate with widely used, traditional bug
   tracking systems.

There are more promissing candidates, but we not there yet.

[trac]: http://trac.edgewall.org
[fossil]: http://www.fossil-scm.org
[ditz]: http://ditz.rubyforge.org/
[b]: http://www.digitalgemstones.com/projects/b/
[be]: http://bugseverywhere.org
[artemis]: http://www.mrzv.org/software/artemis/
