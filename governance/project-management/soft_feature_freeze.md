# What is the soft feature freeze?

The soft feature freeze is the beginning of the stabilization phase of edk2's
development process. By the date of the soft feature freeze, developers must
have sent their pull requests to the edk2 repository **and** received maintainer
review approval. This means that features, and in particular non-trivial ones,
must have been accepted by maintainers before the soft freeze date.

Between the soft feature freeze and the [hard feature
freeze](hard_feature_freeze.md), previously reviewed and unit-tested features may be
applied (or merged) to the master branch, for integration testing. Feature
addition or enablement patches posted **or** reviewed after the soft feature
freeze will be delayed until after the upcoming [stable
tag](../../releases-history/current-releases/edk_ii_release_notes.md).

## What should I do by the soft feature freeze?

As a maintainer, for any feature that you're targeting to the next [stable
tag](../../releases-history/current-releases/edk_ii_release_notes.md), you should make sure that you've reviewed and
accepted the patches related to the feature.

As a developer, you probably should target a date that is at least 1-2 weeks
earlier than the soft freeze date. This will give the maintainer enough time to
review and test your patches. For major features you should probably
communicate with the maintainer about their intentions. It also helps if you:

- Report a feature request in github project.

- Optionally, write a Feature page in the [edk2 wiki](../../index.md) describing the
  feature and the motivation.

- On the [release planning wiki page](../../releases-history/planning-roadmaps/edk_ii_release_planning.md), link to your
  issue entry and/or the feature wiki page.

- Do all this early enough that you can work with the maintainer to get the
  review process underway.

(This definition is modeled after QEMU's [soft feature
freeze](https://wiki.qemu.org/Planning/SoftFeatureFreeze).)

## Announcing the soft feature freeze

The proposed schedule for the active development cycle is shown in the [EDK II
Release Planning](../../releases-history/planning-roadmaps/edk_ii_release_planning.md) article. Shortly before the soft
feature freeze, an announcement email is sent to the
[edk2-devel](https://edk2.groups.io/g/devel) mailing list.
The announcement is posted by one of the maintainers that are listed in
[Maintainers.txt](https://github.com/tianocore/edk2/blob/master/Maintainers.txt),
in section `EDK II Releases`. And, tag edk2-stablexxxxxx-rc0 will be created.
