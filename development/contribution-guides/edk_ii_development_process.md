# EDK II Development Process

First check out [Getting Started with EDK II](../tutorials-howto/getting_started_with_edk_ii.md) for downloading the
latest EDK II development project with your build environment.

Are you new to using git? If so, then the [New to git](../tutorials-howto/new_to_git.md) page may be helpful.

```admonish note
Commands that you should directly type into a terminal are preceded with `>$`. Unless otherwise noted,
these commands are OS agnostic.
```

If you are new to GitHub or looking for ways to make you GitHub workflow more efficient, please read
[GitHub & PR Tips](github_pr_tips.md).

## EDK II Developer Onboarding

At a high-level, getting started with code development in the EDK II repo consists of the following activities:

1. Tool Setup - Performed once per development machine.
2. Workspace Setup - Performed once per development workspace.
3. Development and Test - Performed on every code contribution.
4. Code Review and CI - Performed on every code contribution.

By default, you may not be able to leave comments on a PR or your GitHub account might not be found
to be added as a reviewer to a PR. If you would like to have those capabilities, send an email to
the [edk2 developer mailing list](mailto:devel@edk2.groups.io?subject=Add%20to%20edk-ii-collaborators)
that includes your GitHub username to request access to `EDK II Collaborators`.

> **Note:** For significant design changes or architectural proposals, consider using the
> [RFC Process](../../rfc/text/0000-rfc-process.md) to gather community feedback before implementation.
> The standard development process described here is for code contributions, while RFCs provide structured design
> review for major changes.

## Tool Setup, Workspace Setup, and Development and Test

Refer to the [Build Instructions](../../build-tooling/build-workflows/build_instructions.md) documentation. After
following those instructions, you should have a workspace setup and understand how to build and test the code.

The remainder of this page focuses on source management details and how to prepare for code review.

## Contributor Process

1. Create and checkout a topic branch for your change.

   `>$ git checkout -b <new-dev-branch> origin/master`

2. Make changes in the working tree.

3. Break up working tree changes into independent commits that do not break *git bisect*.
  - [Commit Partitioning](commit_partitioning.md)

   - To stage all modifications: `>$ git add -u`
   - To add new files: `>$ git add <path-to-new-file>`
   - To have git prompt you to selectively stage changes: `>$ git add -p`

4. Follow the commit message template given below when writing commit messages.

    - [Commit Message Format](commit_message_format.md)

    - To commit staged changes: `>$ git commit`

      - Tip: Add the `-s` parameter to automatically append your `Signed-off-by` tag to the commit message.

5. Use the `PatchCheck.py` script under `edk2/BaseTools/Scripts` directory to verify the commits are correctly
   formatted.

    - To check the latest `<N>` changes: `>$ python BaseTools/Scripts/PatchCheck.py -<N>`

      - For example, 2 changes would be: `>$ python BaseTools/Scripts/PatchCheck.py -2`

    - It is strongly recommended that you run `PatchCheck.py` after each commit. You can then easily amend the commit
      to correct any issues.

6. Get the latest changes from the remote (`origin`).

    `>$ git fetch origin`

    > Note: This updates `origin/master`, but not your local `master` branch. (`origin/master` may have newer commits
    > than `master`).

7. Rebase the topic branch onto `master` branch.

    `>$ git rebase origin/master`

8. Run the automated code formatting tool (Uncrustify) against your changes.

  - [EDK II Code Formatting](../coding-standards/edk_ii_code_formatting.md)

   - The changes must pass local CI which includes a code formatting check in order to be merged into the code base.

   - It is strongly recommended that you format the code after each commit. The code can then be easily amended with
     the formatted output. Some developers might also prefer to format frequently while writing the code using the
     plugin instructions described in the code formatting wiki page.

9. Compile and run local CI checks.

  - [Build Instructions](../../build-tooling/build-workflows/build_instructions.md)

   - If you encounter a CI failure, you can use Stuart to run CI checks locally.

    - [How to Build with Stuart](../../build-tooling/build-workflows/how_to_build_with_stuart.md)

     - Stuart can both compile your code and run CI plugins that check other aspects of the code outside compilation.

     - These are the same CI checks that will run against the code when you create a pull request. By running the tests
       locally you will be able to get results much more quickly and reduce overhead on CI resources.

     - However, some aspects of what is used in CI versus your local setup might be different depending on what
       parameters you pass to Stuart. For example, if CI is using VS2019 and you are using VS2017, you might get
       different compilation results.

     - If you are new to the Stuart build system, first learn about the basics of Stuart in the link above and then
       read the "I just want to check if my changes will pass all the non-compiler checks in CI" section to learn how
       to get CI results without having to wait through compilation.

10. Push changes to the developer's fork of the EDK II project repository.

    - How to create a [GitHub fork](https://help.github.com/github/getting-started-with-github/fork-a-repo)
      - **NOTE:** A GitHub fork can also be created using the command line utility called
        [`gh`](https://cli.github.com/).
        - See [`gh repo fork`](https://cli.github.com/manual/gh_repo_fork).

    - Add remote to the developer's fork of the EDK II project.

      - `>$ git remote add <developer-id> https://github.com/<developer-id>/edk2.git`

    - Push the integration branch.

      - `>$ git push <developer-id> <new-integration-branch>`

11. Create a GitHub pull request from the developer's `<new-integration-branch>` to `edk2/master`.

- How to create a [GitHub pull
request](https://help.github.com/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request).

- **NOTE:** A GitHub pull request can also be created using the command line utility called
[`gh`](https://cli.github.com/).
        - See [`gh pr`](https://cli.github.com/manual/gh_pr).

    - If a pull request is only being created to run CI checks, create a [draft pull request](github_pr_tips.md).

- Pull request description needs to include Fixes issue url link, for example: Fixes
[https://github.com/tianocore/edk2/issues/10626](https://github.com/tianocore/edk2/issues/10626), then PR will
automatically be related to github issue.

    - The relevant reviewers and maintainers need to be added to the pull request.
      - If you are a maintainer, you should add the appropriate PR reviewers yourself.
        - See the step in [Maintainer Process](#maintainer-process)
      - Otherwise, a maintainer will do this for you.

    - Resolve GitHub pull request issues if failures are reported. Common failures are shown below.

      - A merge conflict is detected. You may attempt to resolve the merge conflicts outside of GitHub by rebasing
        `<new-integration-branch>` with `edk2/master` and resolving any conflicts that need manual resolution. After
        amending the relevant commits with the changes needed, a force push to `<new-integration-branch>` automatically
        restarts the checks.

      - The pull request fails the `PatchCheck.py` check. Resolve the issues reported by `PatchCheck.py`. A force push
        to `<new-integration-branch>` can be used to automatically restart the checks.

      - The pull request fails Windows or Ubuntu checks. The GitHub pull request provides links to the Azure Pipelines
        results that are used to view test results for checks that failed.

    - If you are unsure how to resolve a PR failure, leave a comment in the PR to ask for help.

12. Modify local commits based on the review feedbacks and repeat steps 2 to 10.

    - For the latest commit, you can use `>$ git commit --amend`

    - For multiple commits use `>$ git rebase -i origin/master`

    - Create a [GitHub discussion](https://github.com/tianocore/edk2/discussions) or consult your git gurus on
      edk2-devel or irc channel if you have git questions.

    - Follow the [PR Conversation Resolution Process](#pr-conversation-resolution-process) to resolve feedback
      conversations.

## Maintainer Process

1. It is recommended to register for email notifications for pull requests, pushes, and check status results by
   setting up notifications for the EDK II repository ([tianocore/edk2](https://github.com/tianocore/edk2)).

   - [Configuring
     notifications](https://docs.github.com/account-and-profile/managing-subscriptions-and-notifications-on-github/setting-up-notifications/configuring-notifications)

2. Add the relevant reviewers and maintainers as reviewers to the pull request.

   - You can find the reiewers and maintainers for the code areas touched by the changes in
     [Maintainers.txt](https://github.com/tianocore/edk2/blob/master/Maintainers.txt).

- You can also use the
[GetMaintainer.py](https://github.com/tianocore/edk2/blob/master/BaseTools/Scripts/GetMaintainer.py)
     script to find the list of reviewers to add.

3. Verify that the Pull Request title and description succinctly describe the changes.

   - PR titles and descriptions are used in repo searches and show up in release notes. More precise titles make
     finding changes easier in the future. High quality descriptions make it much easier to understand a set of changes
     in the PR.

     > If you are familiar with the previous mailing list based process, you can think of the PR descritpion like
     > the cover letter.

4. Verify that the commit(s) can individually be submitted to `edk2/master`. Squashing commits is not allowed.

   - [Commit-Message-Format](github_pr_tips.md)

5. Review the files relevant to you. Once all files are reviewed and any feedback you left is addressed, mark the PR
   as approved.

   - If you are leaving general feedback (not on a line of code) and you would like to ensure that your feedback is
     acknowledged and resolved, use a file comment by clicking the button shown below.

     ![Create GitHub Pull Request](../../images/github-and-prs/file-comment.png)

     - Unlike a generic comment left in the PR, a comment in a code file must be resolved before the PR can be
       completed.

   - If you would like to leave a comment on a specific commit, it is recommended to leave a comment on the file
     and simply reference the commit in the comment. For example, "this change should be in commit `<commit A title>`
     instead of commit `<commit B title>`".

   - Ensure conversations in the PR follow the [PR Conversation Resolution
     Process](#pr-conversation-resolution-process).

   - Note: An approval means you approve of all the changes applicable to you in the PR.

   - Apart from generic comments left in the PR and the comments left on specific files mentioned above, you can also
     leave a comment at the time you "Approve" or "Request changes". In the GitHub Web UI, click the "Files changed"
     tab and then "Review changes", select the appropriate radio box, leave your comment, and then click
     "Submit review".

     ![Leaving Feedback on Review](../../images/github-and-prs/leaving-feedback-in-review.png)

6. Each code area modified in the PR will have a list of reviewers and maintainers assigned to the path in
   [Maintainers.txt](https://github.com/tianocore/edk2/blob/master/Maintainers.txt). If at least one reviewer assigned
   to each code area has approved the PR, the `"push"` label may be added to complete the PR.

   - Note: All PR status checks must succeed and there must be no merge conflicts for the PR to complete.

### PR Conversation Resolution Process

All conversations must be resolved for a PR to be completed. This is the process used to resolve conversations.

1. A PR author is allowed to resolve conversations after they have addressed feedback.
2. After addressing feedback, a PR author is expected to leave a comment describing their resolution in the
   conversations.
3. A conversation cannot be resolved until the PR author and commenter have reached an agreed upon resolution.

## Maintainer Process for the edk2-platforms Repository

The generic rules from the main process applies, with the following additions:

1. Maintainers are responsible for keeping their platform/driver ports buildable against an
   unmodified current version of edk2 and (if so required) edk2-non-osi. Unless given explicit permission
   by the repository owners (as was done for opensbi), platforms/drivers are not permitted additional
   external requirements beyond what is already present in those repositories.
2. Platforms/drivers must document any build steps/options beyond the basic steps described in the top-level
   Readme.md. They must also document the toolchains that are known working.
   - Helper scripts to streamline building are fine, but since those tend to come with their own stack of
     dependencies, they must not be *required* for a basic build test.
   - Pointing to a docker image is fine, as long as the toolchain versions in that docker image are
     also explicitly called out.
3. Platforms/drivers must be buildable with current toolchain versions (i.e. no "build is only supported on
   Ubuntu 14.04 LTS").

## Maintainer Process for the EDK II BaseTools Project

[EDK II BaseTools project](https://github.com/tianocore/edk2-basetools) is a Tianocore-maintained project consisting
of the python source files that make up EDK2 basetools. It provides an easy way to organize and share python code to
facilitate reuse across environments, tools, and scripts. In the future, this project will be the only location of the
EDK II BaseTools python source code, and the EDK II project will remove all BaseTools python source code.

Now, we are in the phase where the BaseTools python code is in both the [edk2](https://github.com/tianocore/edk2)
repository and the [edk2-basetools](https://github.com/tianocore/edk2-basetools) repository. The BaseTools maintainer
should follow the following steps to keep the code in sync.

1. After the patch gets reviewed, the maintainer creates a PR to the `edk-basetools` repo, and merges it into the
   `edk2-basetools` repo if the CI checks pass.
2. Wait for the new version pip module generated in pypi.org.
3. Update the `edk2-basetools` value to the latest basetools pip module version from the `edk2/pip-requirements.txt`
   file. Create a PR to the edk2 repo to trigger edk2 CI to do the packages build tests.
4. Create a pip-requirement patch and send it to community review.
5. Get the Reviewed-by from the reviewers.
6. Create a PR and merge the pip-requirement change to edk2 repository.
7. Create a PR and merge the basetools patch to edk2 repository.

## See Also

- [Commit Message Format](commit_message_format.md)
- [Code Style](../coding-standards/code_style.md)
- [Inclusive Language Guidelines](../../governance/charter-policies/inclusive_language_guidelines.md)
