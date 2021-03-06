# Merging changes in Coq

This document describes how patches (submitted as Pull Requests) should be
merged into the main repository (https://github.com/coq/coq).

## Code owners

The [CODEOWNERS](/.github/CODEOWNERS) file describes, for each part of the
system, two owners. One is the principal maintainer of the component, the other
is the secondary maintainer.

When a pull request is submitted, GitHub will automatically ask the principal
maintainer for a review. If the pull request touches several parts, all the
corresponding principal maintainers will be asked for a review.

Maintainers are never assigned as reviewer on their own PRs.

If a principal maintainer submits a PR that changes the component they own, they
must assign the secondary maintainer as reviewer. They should also do it if they
know they are not available to do the review.

## Reviewing

When maintainers receive a review request, they are expected to:

* Put their name in the assignee field, if they are in charge of the component
  that is the main target of the patch (or if they are the only maintainer asked
  to review the PR).
* Review the PR, approve it or request changes.
* If they are the assignee, check if all reviewers approved the PR. If not,
  regularly ping the author (if changes should be implemented) or the reviewers
  (if reviews are missing). The assignee ensures that any requests for more
  discussion have been granted. When the discussion has converged and ALL
  REVIEWERS(*) have approved the PR, the assignee is expected to follow the merging
  process described below.

In all cases, maintainers can delegate reviews to the other maintainer of the
same component, except if it would lead to a maintainer reviewing their own
patch.

A maintainer is expected to be reasonably reactive, but no specific timeframe is
given for reviewing.

(*) In case a component is touched in a trivial way (adding/removing one file in
a `Makefile`, etc), or by applying a systematic process (global renaming,
deprecationg propagation, etc) that has been reviewed globally, the assignee can
say in a comment they think a review is not required and proceed with the merge.

## Merging

Once all reviewers approved the PR, the assignee is expected to check that CI
completed without relevant failures, and that the PR comes with appropriate
documentation and test cases. If not, they should leave a comment on the PR and
put the approriate label. Otherwise, they are expected to merge the PR using the
[merge script](/dev/tools/merge-pr.sh).

When the PR has conflicts, the assignee can either:
- ask the author to rebase the branch, fixing the conflicts
- warn the author that they are going to rebase the branch, and push to the
  branch directly

In both cases, CI should be run again.

In some rare cases (e.g. the conflicts are in the CHANGES file), it is ok to fix
the conflicts in the merge commit (following the same steps as below), and push
to `master` directly. Don't use the GitHub interface to fix these conflicts.

The command to be used is:
```
$ dev/tools/merge-pr XXXX
```
where `XXXX` is the number of the PR to be merged. This operation should be followed by a push.

Maintainers MUST NOT merge their own patches.

DON'T USE the GitHub interface for merging, since it will prevent the automated
backport script from operating properly, generates bad commit messages, and a
messy history when there are conflicts.
