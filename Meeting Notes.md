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

# Monday - 2022-10-10

deeplow:
* addressed feedback in dangerzone#208
* css & logic to settings in multi-document support (safe extension -safe.pdf and its customization)

Alex:
* Several meetings with Sec / UX people.
* Finished the first pass of the review of [#208](https://github.com/freedomofpress/dangerzone/pull/208)
* Took a look at [#157](https://github.com/freedomofpress/dangerzone/issues/157)

Action points:
* Alex: List the security vectors we need to treat first.
* Alex: Share info for the Mac situation.
* Alex: Edit, test, and merge [#208](https://github.com/freedomofpress/dangerzone/pull/208)
* Alex: Begin the scoping discussion for reproducible builds.
* Deeplow: finish multi-document support in GUI (limit threads & finish styling)

Discussions:
* reproducible builds:
  * dependencies: poetry.lock
  * for system packages: have a debian snapshot
  * we might want to tackle debian first as this is where we / the SecureDrop team has the most experience

# Wednesday - 2022-10-12

Alex:
* Shared an update regarding Mac.
* Very close to merging [#208](https://github.com/freedomofpress/dangerzone/pull/208).
* Didn't manage to work on the security and reproducible builds issue.

Deeplow:
  * Deeplow: multi-document support in GUI: limit threads & add progress icons
    * QThreads implementation is leading to a race condition; Alex suggests we might need to use GDB. Probably inspect the core dump

Action points:
  * Test #208 on Windows as well.
  * (leftover) Alex: Begin the scoping discussion for reproducible builds.
  * (leftover) Alex: List the security vectors we need to treat first.
  * deeplow: review Alex's work on #208 and force-push
  * deeplow: fix Concurrency issues with conversion thread limit
  * deeplow: finish multi-document styling (aligned progress bars)

# Monday - 2022-10-17

Alex:
* Started the umbrella issue for defense in depth.
* Found some failing tests on the main branch on Windows.
* Python 3.9 is not easy to install for developers on Linux (deadsnakes repo)
  and Windows (official installer harder to found).

Deeplow:
* Fixed concurrency issues with conversion thread limit.

Action points:
* Alex: Create subtasks for defense in depth, get ACK from rest of people.
* Alex: Fix the failing tests on Windows.
* Alex: Probably don't drop the commit that drops the Alert class because we'll
  need it
* deeplow: finish multi-document styling (align progress bars), add alert box
  for when exiting while conversion in progress, increase converstion timeout
  when multiple docs being converted.
* Alex: Update the instructions for developing Dangerzone on Linux (add poetry
  reference)

# Wednesday - 2022-10-19

Alex:
* Elaborate on defense in depth subtasks
* went through the Windows unittests and will add that a PR

Deeplow:
* Finished a big portion of the multi-document support.
* Timeout part remaining, will require merging a PR by a contributor.
* Improve thread handling: stopping threads is not straight-forward.

Discussion:
* updating dangerzone - how to detect new updates (separate issue)
* separating the container from the download is important
* supporting ubuntu focal:
  - podman is not available in the 20.04 repos and would require documenting
    how to add external dependencies.
  - Add note for ubuntu focal in the installation wiki and add instructions.

Action points:
* deeplow: create issue for minimizing software in container
* deeplow: Look into stopping threads in multi-document
* deeplow: comment on security in depth issues
* deeplow: final review of python 3.10 bump
* Alex: do PR for window tests
* Alex: give a look at [#167](https://github.com/freedomofpress/dangerzone/pull/167/commits) and ACK it.
* Alex: update ubuntu focal install (& test on focal) instructions

# Monday - 2022-10-24

deeplow:
* Created a template for Dangerzone presentations.
* Tested submitting a package to packages.freedom.press.
* Checked out the Windows unit tests and wrapped up the review.

Alex:
* Sent PR for Windows unit tests dangerzone#235
* Able to GPG-sign as alex.p@freedom.press.
* CVE assessments for Dangerzone.

Discussion:
* the timeout is failing sometimes. How do we solve this?
  - idea: make timeouts proportional to a benchmark ran on first run (might not be good because CPU bursts exist)
  - idea: more leninent timeout (disadvantage: the user can't have an estimate)
  - better approach: add watchers on subprocess commands
* Probably the extra dependency from GitLab can be removed, if we side-step the "spliting the pdf into pages" step.
  - We could also split the PDF using tools from the official repos, such as Poppler.

Action points:
* Alex: Add/Suggest content for Dangerzone presentation.
* deeplow: merge commit for dangerzone#161
* deeplow: (re-review) dangerzone#235
* deeplow: Start with Seccomp issue.
* Alex: Merge the Debian/Fedora PRs.
