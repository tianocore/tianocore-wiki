# Who We Are

![TianoCore Logo](../../images/TianoCoreLogo.png)

## TianoCore Open Source Community

Rev 1.0  **Draft for Review**

## Overview

TianoCore is a community supporting open
source implementation of the Unified Extensible Firmware Interface
(UEFI). This community includes a mixture of professionals and
volunteers from all over the world, working on every aspect of the
mission - including mentorship, teaching, and connecting people. Our
community enables UEFI firmware and tools through various open source
projects. Most of our efforts are currently related to the [EDK
II project](../../reference/external-resources/edk_ii.md).

The community consists of Contributors, Maintainers, Reviewers,
Stewards, Community Manager, Release Manager, and TianoCore Admin. All members adhere to the Tianocore.org
[Code of Conduct](https://www.tianocore.org/coc.html).

### Structure of the Technical Development Community

Technical development community includes Stewards and Maintainers to
steer the technical direction of EDK II project hosted on Tianocore.org.

The technical development community will be assisted by the Stewards.
Current Stewards body includes technologists from Apple Inc., Intel Corporation, NUVIA
Inc, and Red Hat, Inc., representing diverse areas to maintain a healthy
ecosystem.

## Members of the TianoCore Community

## Contributor

------------

### Definition

A Contributor is anyone with interest in contributing to Tianocore.org

### Role of a Contributor/developer

- Follow the [process
    rules](../../development/contribution-guides/edk_ii_development_process.md)
    for code contributions to any of the EDK II Projects on
    Tianocore.org such as:

- Post patches to edk2-devel

- Ask questions (edk2-discuss, edk2-rfc, edk2-devel)

- File bugs (TianoCore Bugzilla)

- Test and review patches for others as defined in the [process
    rule](../../development/contribution-guides/edk_ii_development_process.md)s

- Author or correct wiki articles

### Steps to contribute code

1. Create a Github account: [https://github.com/](https://github.com/)

2. Join the EDK II Project Mailing list: [Mailing
List](../communications/mailing_lists.md)

3. Follow the guidelines for submitting a code contribution by the EDK
    II Project Code Contributions process:
    [../../development/contribution-guides/code_contributions.md](../../development/contribution-guides/code_contributions.md)

## Maintainer

-----------

### Definition

A Maintainer manages one or more packages from the EDK II project of
Tianocore.org.

### Role

Every EDK II package (top-level directory) has a list of maintainers.
The role of a maintainer is to:

1. Maintainer assignments to packages and source file name patterns are
    provided in the
    "[Maintainers.txt"](https://github.com/tianocore/edk2/blob/master/Maintainers.txt)
    file.

2. Subscribe to the \"edk2-bugs\" mailing list
    [https://edk2.groups.io/g/bugs](https://edk2.groups.io/g/bugs),
    which propagates TianoCore Bugzilla
    [https://bugzilla.tianocore.org/](https://bugzilla.tianocore.org/)
    actions via email. Keep a close eye on new issues reported for their
    assigned packages. Participate in triaging and analyzing bugs filed
    for their assigned packages.

3. Responsible for answering questions from
    contributors, on the edk2-devel mailing list
    [https://edk2.groups.io/g/devel/](https://edk2.groups.io/g/devel/)
    and reviewing pull requests for their assigned packages.

4. Responsible for coordinating pull request review with co-maintainers and
    reviewers of the same package.

5. Has push / merge access to the merge branch.

6. Responsible for merging approved pull requests into the master branch.

7. Follow the EDK II development
    [process](../../development/contribution-guides/edk_ii_development_process.md).

**Guidelines for a Maintainer**

1. Process review requests in FIFO order.

2. Maintainer is responsible for timely responses on emails addressed to them (preferably less than calendar week).

3. If you are out-of-office for an extended period of time, inform the
    Stewards and/or your co-maintainers about pending requests. Use
    automated out-of-office replies to indicate your return date, if you
    feel comfortable sharing that info. You can also update your GitHub
    status to indicate you are out-of-office.

4. Try to let each patch sit on the list for at least 24h before
    merging it, so that other community members can comment on the
    patch.

**Adding a Maintainer**

1. A Contributor proposes a patch for Maintainers.txt, extending the
    section for the subsystem / package in question, with a new \"M:\"
    line.

2. An existing maintainer of the package approves the new Maintainer.

3. One of the stewards or one of the existing maintainers of the
    package merges the pull request containing the change.

4. TianoCore admin will update the permissions for the new maintainer
    with access rights to merge and push updates to the appropriate repo
    on TianoCore.

**Removing a Maintainer**

1. The maintainer who wishes to leave will propose their own removal in
    a patch for Maintainers.txt.

2. Optimally, this pull request should be posted from the GitHub account
    which the leaving maintainer is currently represented in
    Maintainers.txt. The email address used for the git author should ideally
    be from the currently represented email in Maintainers.txt \-- for example,
    preferably from the \"old company address\", and not the \"new company address\".
    Giving up a maintainership, if it was associated with someone\'s job, should
    preferably be part of the exit procedure from that particular job.

3. The remaining co-maintainer(s) for the same package acknowledge and
    merge the patch. If no maintainer is left for the subsystem,
    stewards may help with the merging.

4. TianoCore admin will update permissions and remove access rights.

## Reviewer

--------

### Definition

A Reviewer is anyone with interest in contributing to Tianocore.org

### Role of a Reviewer

- Reviewers help maintainers review code, but don\'t have push access.

- A designated Package Reviewer:

  - shall be reasonably familiar with the Package (or some modules
        thereof)

  - will be copied on the pull request and mailing list discussions,

  - and/or provides testing or regression testing for the Package
        (or some modules thereof), in certain platforms and
        environments.

  - Reviewer is responsible for timely responses on emails addressed to them (preferably less than calendar week).

  - Same rules apply from the Contributors.

## Steward

-------

### Definition

Steward is an active community member that helps steer the TianoCore
project and helps drive the community to reach consensus.

### Role

1. The role of a steward is to influence and enact policies and define
    direction of TianoCore projects.

2. The Stewards meet regularly to discuss the EDK II project.

3. Identify and analyze issues in collaboration, communication, and
    workflow; help the community reach consensus to drive a decision.

4. Stewards are responsible for long term planning & infrastructure of
    the EDK II project.

5. Stewards guide the relationship of the TianoCore project with the
    projects that TianoCore depends upon.

6. Stewards guide the relationship of TianoCore project with downstream
    consumers and encourage them to contribute back to the EDK II
    project.

7. Promote open source development practices on the edk2-devel mailing
    list

8. Uphold the [code of
    conduct](https://www.tianocore.org/coc.html); speak up
    if someone violates it on the list or other collaboration areas.

9. Evaluate exceptions to established processes. Evaluate conscious
    divergences from, or evolution of, current processes. For example,
    this covers the eligibility of patches for merging during the
    feature freezes, or more sweeping model changes such as \"git
    forge\" evaluation / adoption.

10. Stewards are responsible for maintaining \"Maintainers.txt\", and
    reviewing patches submitted for \"Maintainers.txt\".

Stewards loosely distribute the above responsibilities between each
other; not every steward is active in every role. Activities and
interests shift with time too.

### Adding a Steward

New stewards are elected by existing stewards in recognition of work
already being done.

### Replacing a Steward

If one of the Stewards decides to retire, they can resign simply by
sending a patch to modify Maintainers.txt. The remaining Stewards may
elect a new Steward to cover the gaps to maintain a healthy ecosystem.

## TianoCore Admin

--------------------

### Definition

A TianoCore Admin manages the access rights to TianoCore Community.

### Role

- The TianoCore admin is responsible for monitoring role changes in the community such as managing access rights while
  adding/removing maintainers.
- Responsible for Approval and removal of access to TianoCore resources such as GitHub, Bugzilla, groups.io etc..

## Release Manager

--------------------

### Definition

A Release Manager manages the TianoCore releases to the Community.

### Role

The Release Manager is responsible for driving the quarterly Stable Tags. The Release Manager will plan the features,
schedule the release date, create the Stable Tag with the release notes and announce to the EDK II community on the
release milestones: Soft feature freeze, hard feature freeze and the final release of the Stable Tag.

## Community Manager

------------------

### Definition

A Community Manager manages the TianoCore Community.

### Role

1. Responsible for chairing TianoCore community meetings.

2. Management of community issues.

3. Coordinate community outreach activities.

4. Run the TianoCore community meetings on a regular basis.

### Guidelines for TianoCore Community

#### Mailing List

1. Use a mail user agent that supports a \"threaded view\" and supports
    tagging messages for later.

2. Process messages / backlog in rough FIFO order.

3. Keep an eye on your mailbox, do revisit tagged messages.

#### GitHub

1. Use the GitHub interface to create and review pull requests.

#### Bugzilla

1. Writing bug reports, issues and/or feature requests:

    - Capture all relevant details such as log files, screenshots,
        command lines, product names, specific product / use case
        details etc.

    - Provide minimal reproducers.

    - Always include where the bug is reported (e.g. EDK II commit,
        tree and/or branch).

    - Frame your bug report / feature request in terms of \"actual
        results\" vs \"expected results\".

2. Responding to new or existing bug reports:

    - Coordinate work with others

    - If the issue is an embargoed security issue, share & discuss
        with your own (downstream) un-embargo deadlines or other
        constraints.

    - Capture posted patches / patch versions in Bugzilla comments and
        link to the mailing list archive.

    - Manage Bugzilla metadata for the fields: assignee, package,
        status etc. and manage the dependencies diligently.

    - Identify duplicate bugs, use the matching Bugzilla resolution.

    - Close Bugzilla ticket once fixed, or feature is complete. Record
        subject commit range in Git history in a new Bugzilla comment.

    - List new feature Bugzillas and high-profile bug fixes
        [here](../../releases-history/planning-roadmaps/edk_ii_release_planning.md).

---

**Resources**

[https://wiki.openstack.org/wiki/Open](https://wiki.openstack.org/wiki/Open)

[https://en.wikipedia.org/wiki/Open\_source\#The\_open-source\_model\_and\_open\_collaboration](https://en.wikipedia.org/wiki/Open_source#The_open-source_model_and_open_collaboration)

Links: [About TianoCore](https://www.tianocore.org/about.html)\|
[Licensing](https://www.tianocore.org/legalese.html)\|
[FAQ](https://www.tianocore.org/faq.html) \| [How to
Contribute](https://www.tianocore.org/contrib) \| [Code of
Conduct](https://www.tianocore.org/coc.html) \|
[Wiki](../../index.md)
