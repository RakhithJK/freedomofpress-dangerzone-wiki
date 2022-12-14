# Wednesday - 2022-12-14

Deeplow:
* Make fix for instability issue dangerzone#217 (with PR  dangerzone#288) and
  make a script to identify podman version where the root cause issue was fixed
  (it wasn't in the release notes)
* Quick investigation on the state of Pyside6 (Qt6) availability in distros (it
  doesn't look promising) - dangerzone#211
* TODO: review https://github.com/freedomofpress/dangerzone/pull/289

Alex:
* Created an issue for Qt testing.
* Created an issue for using packages.freedom.press for APT packages.
* Created an issue for failing CI tests
* Created a milestone for 0.4.1. We should discuss priorities for them and
  divide tasks.
  - We did that and assigned a custom order to the issues:
    https://github.com/freedomofpress/dangerzone/milestone/10

Discussion:
* It seems that the python3-pyside6 package is available only on PyPI, and not
  any distro repos (see https://github.com/freedomofpress/dangerzone/issues/211)
  - We need to have some next steps on this:
    * Find an alternative?
    * Contact the Qt devs?
    * Ask Debian devs?

# Monday - 2022-12-12

Alex:
* Trying to wrap up the QA PR, currently ~1K LOC. Will contain scripts for
  building Linux environments where Dangerzone can run, a script that follows
  our QA steps, and run CI tests for Fedora and other flavors on CircleCI.
* TODO: Meeting notes that I need to update.
* TODO: Open some GitHub issues for the next release.
* TODO: Create a PR for linting unused imports.

Deeplow:
* Debugging https://github.com/freedomofpress/dangerzone/issues/217# with Alex
* TODO: deeplow fix dangerzone#217 with discussed approach
* TODO: investigate package availability on all supported OSes #211

Discussion:
* Fixing dangerzone#217 - if podman version <4.0.0  then we run tests
  sequentially

# Wednesday - 2022-12-07

Alex:
* I'll take a look at dangerzone#284. Looks good, and I might add some things as
  well.
* Working on the QA PR, I'll try to also add it in the CircleCI configuration.

Deeplow:
* Resumed on research about thumbnails and background reads of the file on
  MacOS.
  - Even if you disable thumbnail previews, downloading the file from Safari
    passes through the Indexer / thumbnail preview pipeline.
  - Will report the full findings
  - Maybe it makes sense to include our findings in the section on how one can
    still get hacked, even with Dangerzone.

# Monday - 2022-12-05

We have a release!

Alex:
* Reviewed the README PR
* Retrospective on release and consideration on which issues are release
  papercuts that can be improved

Deeplow:
* Sent a PR for updating Homebrew
  ([homebrew-cask#136918](https://github.com/Homebrew/homebrew-cask/pull/136918))
* Sent a PR for updating the screenshots in the README dangerzone#282

Action points:
* Alex: Sent a PR for dangerzone.rocks with the new screenshots.
* Alex: Send PR for Spin Linux environments...
* Alex: Check CI tests on MacOS / Windows
* Deeplow: Work on Bug: cannot install with Onionshare #153
* Deeplow: Evaluate if Disable previews/thumbnails is effective #65 and after
  that tests fail non-deterministically.

Discussion:

## Candidate Issues for Releases

Small issues that can help subsequent releases:

* Documentation: Disable previews/thumbnails #65
  - May take more time, since it's a complex issue.
* Bug: cannot install with Onionshare #153
* Support building on M1 macs #177
* Migrate to Qt6 before Qt5 end-of-life #211
* Test packages.freedom.press workflow #220
* tests fail non-deterministically (Error: error retrieving size of image) #217
* Get the Dangerzone version from the GUI and CLI #219
* Automated Testing in Windows & Mac #229
* "Open with" on Windows shows Dangerzone Description instead of "Dangerzone" #283
* Spin Linux environments for various distros/versions
  - Document how to setup X11/Wayland forwarding within a container.

Issues for a 0.5.0 release:

* Defense in Depth: ...
* User research: ...

# Monday - 2022-11-28

Deeplow:
* started QA on Fedora, found out that it has Python 3.11 version.
* worked on MacOS QA. Posted update [here](https://github.com/freedomofpress/dangerzone/issues/263#issuecomment-1327482860)

Alex:
* Found out that Ubuntu 22.10 "Kinetic Kudu" has been released 1 month ago -
should be supported. Sent a PR for that
* tested on debian buster (will still be supported for a year from now) -
doesn't have podman, but the instructions are the same as ubuntu 20.04 - some
more libs are required (buster backport, libseccomp)

Discussion
* dangerzone#269 - change the default app window on MacOS
* Python3.10 isn't installed on Fedora 37. We need to check if Dangerzone works
with Python 3.11, and bump the dependency on Poetry.

Action points:
* deeplow: address dangerzone#269 and other MacOS related issues
* deeplow: Check Python 3.11 in his Fedora environment
* Alex: Complete QA process on Windows and ubuntu 22.04 / 22.10
* Alex: send PR for QA script
* Alex: Review deeplow's PRs
* Alex: Check out Dangerzone on a real Kinetic Kudu

# Wednesday - 2022-11-23

Alex:
* Proposed an implementation for skipping slow steps in GUI.
* Worked on publishing a Debian package on apt-tools-prod.
* Reviewed dangerzone#247.

Deeplow:
- finished work on dangerzone#255 (opt to move untrusted files)
- give more feedback on QA process for Dangerzone

Discussion:
- timeouts: for this release
  - double timeout to 2m
  - for this release "disable parallel conversions" by setting the max threads to 1
- Version migration in QA tests:
  - "Install the previous version of Dagnerzone, and tick some non-default settings"
  - "Install the new version of Dangerzone system-wide, and ensure those settings exist"
  - Do `grep -f share/image-id.txt <(podman images)`

Action Points:
- Deeplow: open PR for showing number of selected docs while in settings
- Deeplow: open PR for disabling parallel conversions
- Alex: Review dangerzone#255
- Alex: open PR for doubling timeout
- Alex: Address comments for QA PR

# Monday - 2022-11-21

Deeplow:
* reviewed QA proposal PR
* Opened issue for missing documentation for the whole project - and created
  branch with code implementation (will open PR soon)

Alex:
* Sent a QA proposal for release testing.
* found way to open documents
* Finished the review of dangerzone#247
* Started looking at linting unusued imports.

Discussion:
* The infra may not be there before the feature freeze. We need to address this
  somehow, while still contributing to `main`.
* Timeout increase - why are there timeouts? (investigate)
  - Per our discussion, it seems that it would be best if we have a soft
    timeout; warn the user that the conversion takes too long, and ask them if
    they want to skip OCR.
* Things we want to have ready by the feature freeze:
  - Better timeouts (discussed above)
  - Option to move untrusted files into subdirectory after conversion
    (dangerzone#251)
  - QA tests (dangerzone#246)

Action Points:
* Alex: check out the final changes for dangerzone#247.
* Alex: investigate timeout issues and make a PR for not limiting timeout (see
  discussion)
* Alex: post thread about our "soft timeout" ideas and ask Micah for ack
* deeplow: merge dangerzone#247
* deeplow: open PR for dangerzone#251
* deeplow take a look at the linux-namespace support dangerzone#248

# Wednesday - 2022-11-16

Alex:
* Dived into seccomp and gVisor.

Deeplow:
* Changing the box for the output filename to allow arbitrarily naming for single file conversions turned out to be more difficult.
* Almost done with the fixes in dangerzone#247.
* Started to work on moving the unsafe files on a separate directory, kind of like Qubes TrustedPDF does.

Action points:
* Alex: Send a PR for bumping the timeout.
* Deeplow: Open issue for missing documentation for the whole project.

# Monday - 2022-11-14

Alex:
* Final review of dangerzone#209
* First round of comments for dangerzone#247
* Created a draft PR for Linux User Namespaces.

Deeplow:
* final review of dangerzone#241 (ubuntu focal support)
* address feedback in dangerzone#209 (multi-doc cli support)
* investigate way to reliably detect seccomp policy violation (dangerzone#225)

Discussion:
* Seccomp policy violation detection: have a way to say to the user that opening
  the document failed in the sandbox -- discourage the document opening. -> A
  way to contact the dangerzone team to solve the situation
*  maybe a static analysis tool to check if what syscalls certain documents
   types lead to. This could make us more sure about
* for 0.4.0 we may not be able to give users a foolproof way to detect a seccomp
  violation -> open issue for this
* UX feedback on multi doc conversion:
  * Selecting a directory would be great, but let's not have it in this PR.
  * Selecting files from different directories requires two steps (one to add
    files, and one to add more). Let's add this in a next release.
  * Seeing the files takes real estate from the settings. Having two tabs for
    settings will require quite a bit of work. This is better suited for a next
    release.
  * Let's use the full path of the directory where we will save the converted
    files
  * In general, UX-wise, having safe files with `-safe.pdf` may not help users,
    because they may erroneously choose the unsafe (and shorter) file name. In
    Qubes TrustedPDF, the original files are moved to an "untrusted files"
    directory. We could have a checkbox in the setings for that.
  * We could have a mode where we allow the user to change the output filename,
    if they convert a single file.
  * Once the conversion ends, let's show the original filename.
  * We have a bug where OCR gets applied the next time we run Dangerzone.
  * Showing full context for a single conversion (logs, tracebacks) is something
    we should consider when we discuss the UX side of things, but not right now.

Action points:
* Alex: Jump on board the seccomp PR.
* Alex: Document how we should do QA on Dangerzone.
* Alex: Write down UX comments that we want to consider in the future on dangerzone#117.
* Alex: Review dangerzone#210.
* Alex: Create PR for linting unused imports.
* Deeplow: Do some fixes on dangerzone#247 based on Alex's review comments.
* Deeplow: Ask for feedback on the idea of moving untrusted files to subdirectory

# Wednesday - 2022-11-02

Alex:
* Sent PR for Ubuntu Focal support.
* Minor PRs Changelog, Poetry.lock files, Poetry instructions.
* Done with the review of dangerzone#216
  - Tried out asyncio support successfully.

Deeplow:
* address review comments for dangerzone#216
  * figure out alternative strategy for avoiding wildcard injection vulns by the
	user mistakenly running `$ dangerzone *` on a maliciously crafted document
    set.
  * rebase branches based on it

Discussion:
* Regarding the asyncio support, let's merge this PR first, and then see if we can add it in the GUI PR.

Action points:
* Alex: Start looking at AppArmor

# Monday - 2022-10-31

Deeplow:
* Fixed and merged dangerzone#208
* Merged Debian 12 and Fedora 37 support (dangerzone#230 / dangerzone#233)
* Rebased PRs 2/3 and 3/3 regarding multi doc support
* Tested out Ubuntu 22.04 on our CI runners, but encountered issues.
* Checked out the Alpine status wrt reproducible builds.
* Started working on the seccomp issue.

Alex:
* merged the windows unittest PR
* acked the PRs for Fedora, Debian & the refactor container
* reviewed the part 2 PR for the multi-doc support (#201)
* opened a few small fixes PRs
* added a couple of stuff to the internal wiki

Action points:
* deeplow: Create an issue about handling error messages from the dangerzone
  container, and the security/UX impliciations that this has.
* Alex: 2nd pass of the dangerzone#201
* Alex: update ubuntu focal install (& test on focal) instructions

# Wednesday - 2022-10-26

* Estimate time for each issue in prep for mgmt meeting

Deeplow:
* deeplow: merge commit for dangerzone#161
* deeplow: (re-review) dangerzone#235
* deeplow: (re-review) dangerzone#208

Alex:
* Merged the dangerzone#235 PR

## Estimations

Sizes: XS, S, M, L
Fibonacci-like increments: XS = 1 hour, S = 4 hours (1 day), M = 8 hours (2
days), L = 16 hours (4 days),

* #205: (GUI part of #77)
* #206: Dev: M, Review: XS -> ~2 days
* #188: Scoping: M -> ~2 days
* #77:  Dev: L, Review: M -> 6 days (d: 2 weeks more realistically)
* #204: (GUI part of #77)
* #207: Dev: S, Review: S -> ~2 days (d: 1 hour)
* #209: Dev: S, Review: M -> ~3 days (d: 1 week more realistically) <- + #205 = multi-doc support (CLI)
* #220: (part of untracked)
* #233: Dev: -, Review: XS -> 1 hour
* #230: Dev: -, Review: XS -> 1 hour
* #225: Dev: L, Review: M -> 6 days (d: 4 days)
* #227: Dev: M, Review: S -> 3 days
* #228: Dev: M, Review: S -> ~3 days
* #224: Dev (only removing sudo & ro fs): S, Review: XS -> ~1 day
* #232: Dev (only replace pdftk deps): S, Review: XS: -> 1 day
* #157: (part of #228)

Total time: 35 days * 4 hours / 45 hours per week = 3.1 weeks = ~17th of
November

If we remove:
* #188 (Reproducible builds): -2 days
* #77 (GUI): -9 days (-10 days +1 day for backporting #205 & #204) <- keep that
* #207 (Prevent running DZ): -2 days
* #224 (Prevent root): -1 day
* #232 (replace pdftk): -1 day -> maybe bump to 0.5.0
* #224 (remove sudo & ro fs): -1 day

Remaining: 20 days * 4 hours / 45 hours per week = 1.8 weeks = ~8th of November

Plus GUI: 29  * 4 hours / 45 hours per week = 2.6 weeks = ~14th of November

Untracked (not exhaustive list):
* Provisioning/accessing Mac Minis,
* Signing on Windows and MacOS
* Changes to the website and GitHub
* Build and upload Linux packages, MacOS and Windows binaries
* Perform QA on all platforms.
* Homebrew bump release hash & number.

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

# Wednesday - 2022-09-28

Alex:
* Setup a Windows VM with nested virtualization
* Setup of MacOS environment in KVM (Docker Desktop pending)

deeplow:
* Finished for the cli bulk document conversion, for the most part

Action points:
* deeplow: continue implementing GUI bulk document conversion
* Alex: Today focus on MacOS on QEMU/KVM and make *that* work. Worst case scenario, help with the PRs of deeplow.

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
