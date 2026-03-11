# Commit Partitioning

Since we have many people involved in our projects, communication is
very important. One often overlooked form of communication is the source
control commits.

To maintain an easily reviewable source control history, it is important
to put the appropriate amount of effort into partitioning your changes
properly. This is just as important as the time you spend developing the
code and writing a good [Commit Message](commit_message_format.md).

Part of your role as a
[Code
Reviewer](code_reviews.md) should be to give feedback about proper commit
partitioning.

In general, try to:

- Fix only one issue or add one feature per commit
- Break major changes down into several smaller logical chunks
  - Each chunk should be able to compile and function within the tree

## Examples of what to commit separately

- If checking in both a library class (or PPI, or Protocol) interface,
  and an implementation, then the interface can be checked in first. The
  interface is logically separate from the implementation.
- If checking in several fixes in the same file or module, consider if
  the changes are separate enough to be checked in as separate commits.

## Examples of what to not further sub-divide

- Don't go overboard on breaking down your change
  - If you fixed 5 spelling errors within comments in a file, then it
    does not require 5 commits!
- In most cases a new driver or library implementation should be a
  single commit

## Dealing with bisect issues

Each commit should build and function on its own. This allows for the
use of *git bisect* to identify changes that result in bugs. This is a
powerful tool so be sure not to break it. Below are some hints to deal
with potential *git bisect* issues.

- See if reordering commits will help to keep each commit building and
  functioning
- Adding temporary code that is removed at the end of the patch series
  is an acceptable practice
- Developers are only responsible for testing the build with their
  preferred tool chain
  - If possible test with a Microsoft and GCC tool chain

## See Also

- [Code-Style](../coding-standards/code_style.md)
