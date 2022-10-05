# Monday - 2022-09-26

Alex:
* Wrap up Windows / MacOS platforms by middle of this week.
* Goal: Manage to merge a PR by end of this week.
* Goal: Start reading on the literature for securing containers in-depth (tied to the milestone item).

deeplow:
  * continued work on bulk document conversion. Cli conversion implemented via threading.
  * approved contributor's PR https://github.com/freedomofpress/dangerzone/pull/167

Discussion:
  - for the seccomp/apparmor/selinux scoping discussion (or more general container security) we should schedule that when Alex is done with the dev env. setup on all target platforms
  - brief discussion on build reproducibility
  - parallel discussion should probably use asyncio instead of treading since it is most likely IO-bound instead. But that is a larger refactor which we can do in the future.

Action points:
  - on next monday schedule container security overview (scoping #182)
  - discuss which code parts each of us should be responsible for

# Wednesday - 2022-09-28

Alex:
* Setup a Windows VM with nested virtualization
* Setup of MacOS environment in KVM (Docker Desktop pending)

deeplow:
* Finished for the cli bulk document conversion, for the most part

Action points:
* deeplow: continue implementing GUI bulk document conversion
* Alex: Today focus on MacOS on QEMU/KVM and make *that* work. Worst case scenario, help with the PRs of deeplow.

# Monday - 2022-10-03

deeplow:
 * Continue multi-document GUI support

Alex:
* Finalize PR review: https://github.com/freedomofpress/dangerzone/pull/208
* Took a look at the topic of container security.

Action points:
* Meet with UX person to go over the Dangerzone UX journey.
  - Share the UX issue before discussions start: https://github.com/freedomofpress/dangerzone/issues/117#issuecomment-861691174
  - Give a heads up before merging the bulk conversion feature, suggest optional UX review.
* Deprecate the multi-window functionality, and convert new documents in the existing window. This should affect only MacOS environments, but we expect users to be able to open a second Dangerzone instance, if they want conversion with different parameters.
* Alex: Share the research on the container security subject, schedule a meeting with Alex M.

# Wednesday - 2022-10-05

deeplow:
* implementing ModelView approach for document handling. Ideally we'd have frontends (branch: 77-muti-doc-gui)
* blocker: difficulty in Qt in regards to using dangerzone (see discussion)

Alex:
* Tested PR [#208](https://github.com/freedomofpress/dangerzone/pull/208) on Linux
* I need to create a build environment on Windows as well.
* Send rest of the comments on deeplow's [#208](https://github.com/freedomofpress/dangerzone/pull/208) PR.
* Document my forays around security / nested virtualization on a wiki.
* See how Qubes treats the GitHub commit signatures.

Discussion:
- ModelView blockers
  - performing the same sequence of calls from the GUI and the CLI is not necessary, since presentation differs. What should be the same is the underlying logic for a conversion of a document.
  - in the GUIs we get the dynamic list of documents and convert them individually. A QThread would call
  - if the Model/View separation is complicated at the moment, we can do it in the future if we hit scalability issues (1000+).

Action points:
* deeplow: multi-document UI polish & limit number of parallel conversion
* PR review strategy: avoid rewriting PR history when when the review has started. Preferably append commits but if some history needs to be rewritten or commit messages changed, then do it in new branch called [feature-branch]-N, where N is the iteration number. After OK from the original in-house contributor, force push into [feature-branch].
