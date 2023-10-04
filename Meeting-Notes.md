# Wednesday - 2023-10-04

Alex:
- Sent and merged a fix to catch exceptions in the second stage of Qubes conversion (#568)
- Sent and merged a fix to properly support "dark mode" in our user dialogs (#569)
- Bumped our Poetry deps (#570)
- Bumped our version to 0.5.0 (#571)
- Reviewed the notarytool PR (#558)
- Started QA for 0.5.0
  * We have stumbled on some Qubes issues for which we will open GitHub issues.

# Monday - 2023-10-02

Deeplow:
- rebased and merged "Handle errors in Qubes" (#546)
- let 0.5.0 running large test in background over the weekend
- TODO: commit large test results

Alex:
- Helped with the merging of #550. Required a bit of Git plumbing.
- Addressed some comments in #561 and merged it.
- Re-opened #430 to highlight some issues that we haven't addressed.
- Re-opened the HWP support PR, since we will go with alpine:latest instead of edge.

Discussion:
- Release 0.5.0:
  * Revert the HWP support for Apple Silicon @apyrgio
  * Merge the altool PR once we have tested it during the release @apyrgio
  * Error handling:
    - Catch the OCR error on Qubes @apyrgio
    - Move some error handling scenarios to the stabilitization effort (stable qubes integration) @deeplow
    - Change the out of RAM message with a more generic one: @deeplow
      * "Could not start a disposable qube for the file conversion. More information should have shown up on the top-right corner of your screen."

# Wednesday - 2023-09-27

Alex:
- Merged the PRs for switching to tessdata-fast and for adding installation instructions for Qubes (#548 and #543)
- Sent a PR (#551) for detecting Qubes errors when we receive EOF, and merged it.
- Found a nasty bug (#560) that could potentially lead to leaving the last page of a document out.
- Found an issue with client-side timeouts in the current Qubes implementation (#557)
- Sent a PR (#561) on fixing #560 and #557
- Reviewed #554, #556, #546

Deeplow:
- reviewed "Detect if we received EOF due to a command that failed" #551
- follow up on PR Open Better "dark mode" support (#550)
- open PRs for minor Qubes conversion issues (#554 and #556)
- migrated macOS notarization process and open PR for it (#558)
- TODO: Handle errors in Qubes #546
- TODO: Merge the Dark Mode PR
- TODO: review "Stream page data in real time" #561

# Monday - 2023-09-25

Deeplow:
- wrapped up Qubes error handling PR (#546)
- look into contributor PR "Better Dark Mode Support" #550
- TODO: update notary tools
- TODO: finish reviewing #550 (dark mode PR)

Alex:
- Merged 3 PRs
- Worked on making every read function check the exit code when it receives EOF.
- TODO: Review the error handling PR

Discussion:
- Better Dark Mode PR has some challenges in App mocking. See CI failure https://app.circleci.com/pipelines/github/freedomofpress/dangerzone/1813/workflows/6755dd53-cbd2-49f4-89de-c65008d81995/jobs/20737
- Dangerzone 101 prep

# Wednesday - 2023-09-20

Alex:
- Sent a PR for slimming down our OCR models
- Reviewed the WIP error handling PR
- TODO: Send a fix for detecting exit code of process whenever we reach EOF in our read_* helpers.

Deeplow:
- reviewed pending PRs:
    - OCR parameters passing (#544)
    - RPM packages from an RPM SPEC (#538)
    - installation instructions for Qubes (#543)
    - qubes: Add client-side timeouts (#547)

Discussion:
  - We're running into an issue where if some error happens early in the conversion the conversion, then it won't detect that as the failure reason. Rather, it will detect the fact that it didn't receive the number of pages. So in that case we need to catch also the exit code to understand if the cause. @apyrgio will work on this.
  - We were thinking about having a server-side limit of 56K pages to detect early failures -- the max an unsigned 16bit int can have (2^(8 + 8)). However, this approach is a bit useless because it'll do all the work server-side just to have a 10K page limitation on the client. We concluded that it's best to have the same 10K limit in the serve and the client.

# Monday - 2023-09-18

Alex:
- Review dangerzone#537
- Fixed some issues deeplow commented on (#538, #543, #544, #547)
- Send a PR for client-side timeouts (dangerzone#547)
- Wrote an issue about slimming down our language models (dangerzone#545)

# Monday - 2023-09-11

Deeplow:
- took a look at: Add installation instructions for Qubes" (#543)
  - question: language #431 to update RPM packaging #543 doubts about the "alpine:edge" (#541 #542) https://wiki.alpinelinux.org/wiki/Edge. We run the risk of not having the package in time or forgetting that we even have ":latest"

Alex:
- Updated the items for the 0.5.0 roadmap.
  * Removed the stretch goals that wouldn't cut it for this release, did my best to set due dates for each task.
- Reviewed and merged dangerzone#451, which adds HWP support on MacOS.
- Reviewed a PR for Qubes error handling (dangerzone#537)
- Sent a PR with formal installations instructions for Qubes, as well as some updates on our Qubes RPM packaging PR.
- Sent a PR that improves the way we pass OCR parameters during sanitization (dangerzone#544)
- TODO: Update the PR with the installation instructions to make the Qubes RPM to include every tesseract-langpack-* package.
- TODO: Recap internally the main points of the RPM packaging story
- TODO: Weigh in on the 1.0.0 vision
- TODO: Check the discrepancy on the size of RPM language models vs the downloaded language models.

Discussion:
  - alpine:latest (#540)
     - deeplow agrees with the assessment for ":latest"
  - 1.0.0 vision
  - OCR languages
  - Let's take a look at the proposed roadmap, see the issues that each of us can work on parallel.
  - mention GL discussion on slack?


# Wednesday - 2023-08-30

Alex:
- Continue working on RPM packaging: #298, #431, #514
  * I have managed to follow pretty much the latest conventions regarding SPEC files, and I have created a fully working one, albeit hacky.
  * I have a more polished version that lacks only the following:
    - Includes our assets (`/usr/share`) in the final RPM
    - Run a post install script that fixes the stale `.egg-info`  directories
    - Allow it to produce a `-qubes.rpm`
- TODO: Verify that .dist-info is the latest recommendation by Python, and not .egg-info
- TODO: Consider adding temporary directory for RPM builds in ~/.local/dangerzone-dev/rmp-build/<tmpdir>/...

Deeplow:
- post on forum
- continue continue work in Qubes error handling

Discussion:
- implement client-server shared error codes with an increment of 128

# Monday - 2023-08-28

Alex:
- Taking a look at our bdist_rpm alternatives
- Prioritized items for the 0.5.0 milestone
- TODO: Clear up the 0.5.0 milestone from the stretch goals
- TODO: Check how SecureDrop Workstation creates their RPM files and incorporate some of the logic in Dangerzone.
- TODO: Review the Qubes alpha instructions PR

deeplow:
 - continue RPM packaging issue (#298)
 - sync 0.5.0 release scoping in prep for planning meeting
 - follow up on dangerzone.rocks not updating
 - discuss with user issue where Dangerzone wouldn't start (#514)
 - create issue about Dangerzone not showing on Gnome Software (#531)
 - drafting up post for Qubes Forum about DZ alpha (Marek's suggestion)
 - upgrading dev environment to Fedora 38
 - test and review Qubes alpha setup instructions (related to the forum post)
 - TODO: Publish Qubes forum post
 - TODO: error handling on Qubes and timeouts

# Wednesday - 2023-08-23

deeplow:
- investigate /tmp space shortage (see #518)
- found upstream issue with pdftoppm https://github.com/freedomofpress/dangerzone/issues/524
- merge simple and approved PRs (#510, #509, #508)
- implement suggestions in PR 'Propagate "update check" prompt to UI checkbox' (#515)
- final review and approval of: "Post-release fixes for MacOS issues" (#523)
- Address feedback in large tests PR and merge it
- Continue RPM packaging work (#298)

Alex:
- Reviewed PR #386
- Test uninstall situation in Debian
- Created issues for page size problems and test_update_error
- Milestone 0.5.0 discussion

Discussion:
- SELinux violation (#517): We haven't yet managed to trigger an SELinux violation yet. Will try removing the :Z flag.
- RPM packaging (#514): We've made progress there, but we don't have a package out yet. We need to pair on this problem fix some remaining issues.
- Announce Qubes Alpha integration in the Qubes forum:
  * Also start a GitHub discussion for this feature.
  * Give a list of issues that we will work on in the next few months.

# Monday - 2023-08-21

Alex:
- Reviewed PRs #515, #510, #509, #508
- Added fixes to my PR (#523)
- TODO: Review #386
- Tested uninstalling Dangerzone on Debian and it does not leave stale folders behind.
  * Actually, I never tested with __pycache__ folders. I guess I need to retest...
- TODO: Check if Dangerzone originally used the tmp directory, and then moved to the config one.
- TODO: Create an issue for fixing the page size problem.
- TODO: Open an issue for test_update_error that fails randomly

Deeplow:
- looking at packaging RPM
- TODO create issue for dark mode on macOS (black)
- TODO try and try to get an SELinux violation :z argument
- TODO continue RPM packaging work

Discussion:
- What is the size of a single page?

  * 1 A4 page - 72 DPI = 595 x 842 pixels
  * 1 A4 page - 150 DPI = 1240 x 1754 pixels

  We need to account for 3 color channels though (RGB), meaning that the final size is:
  * 1 A4 page - 72 DPI = 3 x 595 x 842 pixels = 1.43 MiB
  * 1 A4 page - 150 DPI = 3 x 1240 x 1754 = 6.22 MiB

  If we want to fill 1 GiB of RAM, we need 716 pages (72 DPI) or 165 pages (150 DPI). Note that at some point, we compress the end product, so there will be two files in the same tmpdir (the united ones, and the compressed ones).
  * What can we do here?
    1. Can we stream the pages from container 1 to container 2, and call the programs that "unite" them on the stdin or sth?
       * This would be optimal, but it requires an architecture that we don't have right now (2 containers speaking to each other).
    2. Can we compress each page that we receive from container 1 (e.g., RGB to PNG)?
       * The streaming pages feature is a hard requirement for this. Else, we'd need to use an inotify-like mechanism, which is typically not cross-platform.
       * PNG-compression improvements: 30x - 40x for document types, 2x for photos.
    3. Can we store the pages in a data dir? (reverting to the way it was before)
       * Not the best option, as this will leave traces of the file in the computer, especially if the original file existed in a tmp dir or an external device.
- bdist_rpm deprecation:
  * RPM packaging commands can take a .toml as an argument.
    - RPM can now fetch dependencies from pyproject.toml. But our toml file has sections that are poetry-specific [tools.poetry.dependencies] so rpm packaging commands cannot recogize this, but maybe there is a PEP.
    -  We need to handle removing files from previous installations, even if the RPM that we produce is correct. We need a pre-installation script that will handle removing the stale egg.info dirs, and it needs to work even if no such dirs are present. Also, it needs to exist for quite a lot of time in our codebase, because there may be users out there who forget to update for a while, and we need to cater to them as well.
- I saw a screenshot in this issue (https://github.com/freedomofpress/dangerzone/pull/508) and I wondered, do we handle dark mode correctly? Reminder that we forced the font color to be black in https://github.com/freedomofpress/dangerzone/pull/487
- I think the test `test_update_error` is flaky. I've seen it fail a couple of times with "signal not emitted withing 5000ms". We should open an issue.
- We need to trigger the SELinux bug without Dangerzone. Can we use `chcon` / `restorecon` for that?
  * The SELinux alert brower is probably not stock Fedora. So the user in #517 _could_ have edited something else as well.

# Wednesday - 2023-08-16

Deeplow:
  - Replied to users comments over the weekend
  - make a fix to show immediately the "check for updates" checkbox as soon as the users accepts updates in a prompt.
  - analysis of language-stripped down versions of DZ to evaluate the impact on app size
  - helping user troubleshoot Dangerzone failing to start (#514)
  - TODO: look into https://github.com/freedomofpress/dangerzone/issues/517
  - TODO: reply https://github.com/freedomofpress/dangerzone/issues/514
  - TOOD: tacklet error handling but also look into packaging qubes components

Alex:
- Started looking on some Fedora-related issues (#514, #517, #518)
- Opened some issues/PR for problems that we found during release.

# Monday - 2023-08-07

Deeplow:
  - reviewed and merged all pending PRs for 0.4.2 release
  - continued work on large-test-docs PR to see if we could leave it running over the weekend (wasn't be possible too much work left rebasing)|
  - TODO: Create release artifacts for Windows
  - TODO: Create release artifacts for Intel Mac

Alex:
- TODO: Create release artifacts for M1 Mac
  * Ping deeplow once it's free, so that he can run the large test
- TODO: Create release artifacts for Ubuntu/Debian/Fedora

Discussion:
- We missed CI testing on MacOS M1 platforms
- We missed GUI testing on installed Debian packages (e.g., PySide2)
- We should ensure that Poetry runs from the latest Python version.
- We should ensure that the container.tar.gz is fresh and exists, when building the final Windows / MacOS artifacts.

# Wednesday - 2023-08-02

Deeplow:
- TODO QA on windows but first check if hwp works there
- TODO QA on Ubuntu
- TODO QA on Debian

Alex:
- TODO QA on MacOS x86 / Apple Silicon
- TODO QA on Fedora

# Monday - 2023-07-31

Deeplow:
- Look into flatpak situation
- Merge 2 approved PRs
- HWP: extra docs folder and base64 to avoid accidental opening
- TODO: review "Improve the UX of the update check flow" #490
- TODO debug issue making tests where CI ran out of space

Alex:
- Sent a PR for various UX improvements (dangerzone#490)
- Sent some small PRs for minor improvements (dangerzone#486,487)
- Looked our Flatpak situation a bit (dangerzone#45)
- Ready to send a PR for sanitization logic
- TODO: FIXUP https://github.com/freedomofpress/dangerzone/issues/489

# Wednesday - 2023-07-26

Alex:
- Merged the update notifications PR (dangerzone#466)
- Merged the PR that bumps our Python deps and allows us to run more than one Qt CI test (dangerzone#483)
- Merged a user contribution for a long-standing warning (dangerzone#481)
- Debugged lots of CI failures in the meantime, that are Qt-related (e.g., dangerzone#480)
- TODO: Sunset Ubuntu Kinetic
- TODO: Fix fonts in Fedora
- TODO: Research on the flatpak situation
- TODO: Add tests for sanitization logic (2023-07-santize-progress-update)
- TODO: Open a Qt issue for PySide6
- TODO: Send a PR to the extrepo devs
- TODO: Convert https://github.com/apyrgio/homebrew-reprepro to actual PR

deeplow:
- rebase and fix CI issues with PR" Add "change document selection" button #439
- Taking another look at the HWP PR to see what's missing
- Rebuild broken dev environment (something podman related got broken)
- review PR "Bump python dependencies" #483
- TODO Look into flatpak situation
- TODO Merge 2 approve PRs
- TODO HWP: extra docs folder and base64 to avoid accidental opening

# Monday - 2023-07-24

Alex:
- Wrap up update notifications PR
- Add extra GUI tests for full coverage
- Bump Python dependencies
- Remaining fixmes for updater notification PR:
  * Detect Homebrew installations: we cannot detect that an application has been installed via Homebrew. Open an issue for this
  * What if users click on X? TODO: I need to open a PR for this.
- TODO: Update the update notifications PR with proper fixes for the CI tests.
- TODO: Send the PR for the rest of the GUI tests
- TODO: Send the PR for the new Python dependencies

Deeplow:
- TODO: strip out certain code from bulk doc conversion
- TODO: https://github.com/freedomofpress/dangerzone/pull/464
- TODO: hancom PR
  - final look at the code
  -  remove "without-extensions" files (duplicates) they are mounted

Discussion:
* Open threads for releasing 0.4.2:
  - Container logging:
    * Defer to after 0.4.2
    * Qubes support required major revisions
    * Reworked the training part of the testing
    * TODO: Factor out the string sanitization logic from this PR. Sanitize the strings in `print_progress` in our current code. Pass it optionally through `repr()`.
  - Updater PR:
    * TODO: Add proper commit for fixing our CI tests (use `-g` instead of `offscreen`, remove pytest-wrapper)
  - Change document selection:
    * Rebase it on top of the main branch, once the updater PR gets merged. We expect that all tests will be green.
  - Use containers in Qubes:
    * Follow up on the review comments
  - HWP PR:
    * TODO: Remove the files without extensions

# Wednesday - 2023-07-19

Deeplow:
  - caught up with over-the-weekend contributors comments
  - continue work on large docs PR
  - TODO do minor QT-related TODOs on "DZ Update notification" PR (#466)
  - TODO feedback in other PRs
  - TODO update

# Monday - 2023-07-17

Deeplow:
    - ask: @apyrgio to comment on GPL situation https://github.com/freedomofpress/dangerzone/pull/460#issuecomment-1637200125
    - review of on paper on PDF redaction relevant for advising on the use of DZ for post-redaction mitigation. https://petsymposium.org/2023/files/papers/issue3/popets-2023-0069.pdf
  - address feedback in "DZ Update notification" PR (#466)
  - follow up on HWP office issue (#460)
  - open issue about adding CJK fonts
  - prep machines for QA (found some issues)
  - TODO: check HWP PR conflicts with the Qubes conversion (#460) and open issue for adding that
  - address feedback large docs test PR (#386)

Alex:
- Reviewed some PRs for 0.4.2 (#467, #464, #450, #439, #386)
- Still working on fixing some issues with the update notifications PR (#189), based on deeplow's review comments
- Researched a bit what's the case (licensing-wise) with regards to H20Restart (GPL) and Dangerzone (MIT)
- Took a look at our Homebrew situation, due to a user report (#470). Haven't found something breaking so far, but we're talking with the user to understand what's breaking in their side.
- Merged a PR for SIP instructions (#401)

Discussion:
- Homebrew issues:
- 0.4.2:
  * Updater notifications PR:
    - We need to store the error somewhere, instead of retrieving it from the settings. We can either store it in the QAction, or in the MainWindow.
    - The tests for the updater settings have lots of updater logic (updater fixture for instance) so maybe we can keep them within the `test_updater.py` module.
  * Enable container logging PR:
      - Maybe it makes sense, in the server side, to grab the output from the commands verbatim, and then do any processing in the client side
  * HWP support PR:
    - Summarize the licensing situation and mention it's not a blocker.
    - Check that in Qubes, missing HWP support is not a big issue (doc conversion will fail)

# Monday - 2023-07-10

Deeplow:
    - Found issue leading to recusion in QT tests https://github.com/freedomofpress/dangerzone/actions/runs/5457217733/jobs/9930987271?pr=466
    - reviewed https://github.com/freedomofpress/dangerzone/pull/466
    - continued work on Hancom Office PR
    - checking windows situation for QA (opened PR)
    - TODO finish setting up manual QA system for windows

Discussion:
    - TODO book UX meeting
    - QA and release: bump python and python deps for mac and windows (add to release procedure)

# Monday - 2023-07-03

Deeplow:
    - review and propose a patch to hancom PR
    - TODO: look at https://wiki.documentfoundation.org/Documentation/DevGuide/Extensions

Alex:
- Finalizing initial work on update notifications PR

# Wednesday - 2023-06-28

Deeplow
  - Review scope of UXD work
  - Open PRs for 2 new platforms (Debian Trixie and Ubuntu Lunar)
  - Start work on falling back to using containers on Qubes by default (we don't want any Qubes users expecting it to use containers to be surprised) #451
  - Catch up on contributor's PR for supporting Hancom Office files (see #460)
  - check if containers isolation works in Qubes

Alex:
* Reviewed PRs for Debian Trixie and Ubuntu Lunar
* Reviewed and merged PR for reducing container image size by a first-time contributor (thanks!) (see #459)
* Started reviewing a PR for Hancom file support (see #460)
* Started working on the update notifications issue (see #189)
* TODO: Check Hancom issue with Libreoffice

Discussion:
  - hancom office files support
    * Security consideration: Do not pre-load this extension for every filetype.
    * Size consideration: Fonts take ~90 MiBs of space, probably bitmap, might be worth checking ttf
    * (optional) Security consideration: Have an "experimental" flag for Hancom files.

# Monday - 2023-06-26

deeplow:
    - rebase progress reports PR (#450)
    - finishing up rebasing large docs test on top so it works on Qubes (plus many improvements)
    - TODO wrap up bulk doc test (#386)
    - TODO (?) improve GUI test coverage (make QA easier)

Alex:
- We merged alpha Qubes integration!
- Prepared an internal presentation for the above item.
- Added a plan for update notifications (dangerzone#189)
- Started working on a plan for the 0.4.2 milestone.

Discussion:
- Leftovers for Qubes integration presentation
- Set dates and work items for 0.4.2
- UX for notification updates:
  * No need to terminate the updater thread (if it's running in the background) when the user unclicks "Check for updates"
  * Threshold for update checks can be 12 hours
  * We can add a setting icon (hamburger/gear) in the top right corner, that can be always visible.
    - We will follow the Tor Browser notification model.
    - This icon will have a notification bubble in case of a new update or error.
    - Clicking on this icon will open a dropdown menu.
    - This menu will have the following items:
      * "Check for updates" with a slider (on/off)
      * "Update available" / "Update error", accompanied by a green/red notification bubble.
        -  Clicking on this menu entry will open a pop-up with info.

# Wednesday - 2023-06-21

deeplow:
    - TODO record dangerzone-qubes demo video
    - TODO give feedback on presentation
    - TODO continue bulk doc test (based on Qubes PoC PR)

Alex:
- Wrapped up the final branch for the Qubes PoC
  * Mainly made the Git history more sensible.
- TODO: Work on the Qubes presentation
- TODO: Propose plan for 0.4.2 or 0.5.0

Discussion:
    - capturing all output for debug purposes
      - send all _relevant_ data via stdout (pixels)
      - send all debug info via sterr. Currently captured only on development. In the future add in cli qubes-similar --pass-io that shows sanitized attacker-controlled sterror so we can understand why a certain document failed. We'll cap this at a reasonable amount.
      - in production for the moment, we'll be using exit codes to identify the step in which it failed

# Monday - 2023-06-19

Deeplow:
  - investigate the tails situation following private user question
  - Finish addressing Qubes PoC PR feedback
  - TODO: Work on progress reports (#429)

Alex:
- Continued working on Qubes PoC.
- TODO: Finalize the Qubes PoC branch
- TODO: Scope the update notification feature (#189)
- TODO: Create an issue for using containers by default on Qubes
- TODO: Add support for Ubuntu Lunar (23.04) and for Debian Trixie (13)

# Wednesday - 2023-06-14

Alex:
- Reviewed the Qubes PoC
- Reviewed the large tests PR

Deeplow:
- address review comments for Qubes PoC

# Monday - 2023-06-12

deeplow:
- opened PR for Qubes ([#437](https://github.com/freedomofpress/dangerzone/pull/437))
- working on "add change docs selection button" issue ([#428](https://github.com/freedomofpress/dangerzone/issues/428))
- migrated user stories to github. Hopefully, this can be made public soon.
- TODO: finish "change docs" PR
- TODO: investigate updating

Alex:
- Sent review note for Qubes PR
- TODO: Reply to deeplow's comments on Qubes OS
- TODO: Quick review comments for huge doc tests

# Wednesday - 2023-06-07

Alex:
- Updated the code for the server-side conversion of Qubes
- Fixed a CI race for Debian Bullseye
- Ignored a CVE in our security scanner that does not apply to us

deeplow:
- review "ci: Fix CI races in Debian Bullseye tests" #435
- organize some notes on admin API
- work on Add "Change selection" button near "x document(s) selected"dangerzone#428
- TODO: open PR for qubes-poc and review each other

# Monday - 2023-06-05

Alex:
- Presented Dangerzone at Dataharvest to ~20 people. Went pretty smooth and got interesting feedback.
- Finished the review of the Qubes integration PoC branch. Will share the review comments soon.
- TODO: Check out comments on asyncio in the server-side wrapper.
- TODO: Send comments for Qubes integration PoC

deeplow:
- reviewed Qubes Admin API
- Add "Change selection" button near "x document(s) selected"dangerzone#428
- TODO: Send PR for selection, once Alex comments on asyncio

# Wednesday - 2023-05-31

deeplow:
- Looking into Qubes Admin API for dangerzone distribution methods
- review https://github.com/freedomofpress/dangerzone.rocks/pull/16
- move Qubes design doc to the github wiki
- add user story for security slider
- Open Dangerzone-Qubes issues:
- Progress reporting
- Timeouts
- Exception handling + hardening
- packaging (OCR + libreoffice + ...)
- TODO: document / post summarizing Qubes API findings and limitations
- TODO: start taking a look at the UX issues
- TODO: talk about potential need to split container.convert() into 2 stages (for file preview)

Alex:
- Fixed a minor Qubes issue wrt Python's bindings for libmagic.
- TODO: Finalize the Dataharvest presentation
- TODO: Review the Qubes integration POC branch

# Monday - 2023-05-29

Deeplow:
* polish most of the dangerzone Qubes integration (still not done yet)
* work on user stories and creation of multiple issues following that work

Alex:
* Worked on the presentation for Dataharvest 2023
* Merged the PRs that fix our CI tests
* Weighed in on some GitHub issues for post-IJF user stories
* Debugged an issue with `py3-magic` in Fedora environments.
* TODO: Work on the server-side integration for Qubes.
  - Fix the Python magic issue
  - Merge dz.Convert with the dangerzone wrapper code.
  - Make sure that asyncio works on the server-side conversion.

Discussion:
- large doc tests on Qubes (https://github.com/freedomofpress/dangerzone/pull/386/)
  - we have to somehow change the logic to accommodate the fact that we stream pages and there's no space fo sending json data inbetween
  - we can probably just send the data at the end of the conversion as json. The client ignores this if not in debug mode

# Wednesday - 2023-05-24

Deeplow:
    - Review pending pull requests
    - continue work on Qubes PoC
    - Go through user research once again and map it to user stories
    - Idea: restricted web service - having a Dangerzone web service without file upload (only select doc from URL). This mitigates the somehow risk of having people upload sensitive documents https://github.com/freedomofpress/dangerzone/issues/110#issuecomment-1560534886

# Monday - 2023-05-22

deeplow:
- investigate OCR language issues that blocked CI and opedned PR #418
- merge contributor's typo patch #416
- reviewed open PRs from Alex
- finish first stage of Qubes PoC
- drafting installation instructions / packaging stuff for Qubes PoC

Alex:
- Proposed a fix for some Tesseract issues.
- Fixed a racy test for Debian Bullseye

Discussion:
- Qubes PoC next stages
- How do we homegenize the code
  - container/dangerzone.py will be split in two:
    - dangerzone/conversion/pdf_to_pixels.py:convert
    - dangerzone/conversion/pixels_to_pdf.py:convert
  - Move Dockerfile to the root directory.
    * Make our container image build scripts use the dangerzone/ dir as the build context.
- Move the design document to the wiki
- Tesseract:
  * Add a test for checking if the trained data match our language list
  * Consider adding a safeguard for making languages not selectable if the trained data do not exist.
- Debian Bullseye: Polish the PR.
- Create user stories as GitHub issues

# Wednesday - 2023-05-17

Alex:
- First draft of the Qubes integration design document
- Created some issues for the Qubes integration
- Merged Fedora 38 support
- Update security scanning PR
- Sent a PR for a CSS issue on dangerzone.rocks

Deeplow:
- development of Qubes integration

Discussion:
- Binary protocol: should we use asyncio? If we go full asyncio, we may need to greenify Qt as well. It may be worth making only the conversion async, and the rest of the code sync. Python has the ability to run async functions on a specific thread, and wait for it.
- qrexec: Can we use the Python API instead of the executable?
- we'll need the user to create a diposable template for dangerzone. Otherwise they'll use the fedora-37-dvm, which is networked. We can check the networking of the dispVM on the first start just as a santity check to make sure the user didn't shoot themselves in the foot
- ultimately we should streaming integration with Docker as it would make the user IDs easier to handle, which has caused us problems in the past. This won't be a concern for now.

# Monday - 2023-05-15

Deeplow:
- prune branches

Alex:
- Ran Dangerzone PoC on Qubes
- Followed up on some PRs

Discussion:
- Qubes OS:
    * Alpha: Provide a rudimentary integration with Qubes OS. E.g., may not have GUI, or other important (but not security-critical) features, but the core functionality is there.
      - assumption: main qube and disp qube are based on the same template
      - have a qubes isolation provider
      - set the proper isolation provider in Qubes (containers in most platforms, dispVMs in Qubes)
      - figure out if licensing affects the use of Qubes APIs
      - no code in dom0, install can include instructions for dom0
    * Beta: Improve the integration with QubesOS. E.g., add GUI support and other important features, make the installation easier.
    * Stable: Finalize the integration with QubesOS. There should be feature parity in all platforms, and installation on QubesOS must be as easy as possible.
    * Integration Testing
    * SecureDrop integration
      - assumption: main qube and disp qube are based on the same template

# Wednesday - 2023-05-10

Alex:
- Set up Qubes for experimentation
- TODO: Follow-up on IJF findings
- TODO: Run Dangerzone PoC on Qubes

Deeplow:
- review security scanning PR (https://github.com/freedomofpress/dangerzone/pull/405)
- Open discussion around OCR
- Rethink the need to have status messages (in fact we don't), which will make the Qubes integration easier
- reading up on UX user stories in prep for discussion
- prep Q meeting for DZ update
- present Qubes (minimal) PoC
- idea about potential signal service

# Monday - 2023-05-08

Alex:
- Sent the PR for the security scanning of our images (#405)
- Sent the PR for building our Debian package and installing it in every Debian-based flavor (#406)
- Sent a PR in yum-tools-prod to fix sig checking for RPM packages.
- Remaining:
  * Debian Bullseye CI
  * Write down notes from the 0.4.1 release.
  * Clean up stale branches.
  * Review open PRs
- Dataharvest: Pitch Dangerzone!

Discussion:
- Juggling priorities:
    - O.4.1 milestone: We should close it once we review the PR for the large tests. Actually this issue might be a collection of other issues (0.4.0 - 0.4.1 comparison, testing Dangerzone against large dataset, storing performance results per release). We'll make this clearer once we review the PR. For now, we will move this to the 0.5.0 milestone
    - IJF results analysis and User Stories development
      * We need to develop user stories based on user interviews.
      * Figure out lessons learned, convert those to User Stories
      * From User Stories, we will have actionable items.
    - Qubes integration Proof of Concept
      * alex: Install QubesOS and have a quick tour (end of week goal)
      * Current Qubes PoC: write down instructions, somehow make it mergeable.
      * Future steps: Create issues for missing functionality, add those in 0.5.0
    - Open PRs
    - TBD: 0.5.0 priorities
    - Dataharvest: Share presentation, pitch tool.

Deeplow:
- curate IJF data
- coordinating IJF insights analysis for this week

# Wednesday - 2023-05-03

Deeplow:
- GPG signing key in Mastodon account
- IJF interviews analysis
- sync meeting about IJF interviews
- write a list of branches to protect

Alex:
- Continued on the security scanning front.

Discussion:
- https://github.com/freedomofpress/dangerzone/issues/323
  - branches to protect
    - 397-f38 :: PR #399
    - 334-large-test :: PR #386
    - 221-user-ns :: PR #248
    - wip-gvisor ::
    - qubes-build-inst ::
    - release-0.4.1-grype ::
    - 334-large-test-gh-actions ::
    - qubes-integration-poc ::
  - for Alex to check:
    - qa-windows
    - ci-debs
    - ci-wheel-validation
    - 334-large-test-ssh
    - 352-0.4.0-large-tests

# Wednesday - 2023-04-26

Alex:
  - We released 0.4.1!
  - TODO: Send a PR for Grype code scanning and close #222 (release-0.4.1-grype)
  - TODO: Send a PR for CI of Debian-based debs (ci-debs)
  - TODO: Fix Debian Bullseye CI (https://github.com/freedomofpress/dangerzone/issues/388)
  - TODO: Clean up stale branches
  - TODO: Write down configuration notes for MacOS (homebrew fork of reprepro, multi-user Homebrew/Python, run Docker without sudo)
  - TODO: Prepare for release retro
  - TODO: Handle architecture options for Debian packages (https://github.com/freedomofpress/dangerzone/issues/394)
  - TODO: Nuke the PackageCloud repo after #323 is resolved
  - TODO: See improvements for yum-tools-prods CI checks (sig checking for unsigned packages, repo configuration for new distros)

deeplow:
  - add fingerprint to website (https://github.com/freedomofpress/dangerzone.rocks/pull/12)
  - make protocol for rotating keys, listing places where key need to be updated (mastodon, website)
  - investigate potential integration complexity of Dangerzone into OCCRP's Aleph
  - TODO: write a list of branches to protect
  - TODO: Add GPG signing key in Mastodon account
  - TODO: performance increase toot: You should expect this last Dangerzone release (0.4.1) to be faster overall on larger documents, especially on apple silicon chips (M1, M2), which now runs natively.
  - TODO: resolve https://github.com/freedomofpress/dangerzone/issues/323

# Monday - 2023-04-03

Deeplow:
  - release procedure preparation

Alex:
- QA and release preparation
- Check some Windows performance issues between GUI and CLI

# Wednesday - 2023-03-29

Deeplow:
  - follow up on libreoffice mime type issues
  - started QA on RC2
  - TODO: open issue about making libreoffice security level to max
  - TODO: open issue about about pipefail (see discussion)
  - TODO: update macOS signing key ID in code and same for Window

Alex:
- Fix Poetry CI issue
- Merged the Changelog PR for the Keep a Changelog format
- Fixed incomlete MIME type support for some files.
- Looked into running NoMachine securely on Linux
- Started the QA on Windows and Ubuntu 22.10
  * I should have started it in Fedora 37 :-|
- TODO: Update Debian PR with the new FPF keys
- TODO: Send a PR for Fedora instructions.
- TODO: Check out GitHub deployment keys for apt-tools-prod / yum-tools-prod

Discussion:
  - QA issues:
    * set -o pipefail for bash scripts (see podman save | grep situation).
    * Dangerzone shows an empty image when image load fails -> creeate an issue for that.
    * Bump version to 0.4.1-rc2 in pyproject.toml and share/version.txt
  - In the future, setup LibreOffice security level to Very High.

# Monday - 2023-03-27

Deeplow:
  - review 0.4.1 changelog update PR
  - brief look at mime type issue and propose solution (dangerzone#369)
  - internal demo of Qubes integration PoC (Dangerzone-Qubes-trusted-PDF-hybrid)
  - final work on large tests (local testing only)
  - short dive into how the Wikimedia software handles PDF security - create GH wiki page about "similar projects"
  - TODO: look into the mime types
  - TODO: Commence the QA (again!)

Alex:
- Got nerdsniped with Tailscale, debugged some issue for a little while
- TODO: Fix Poetry / CI issues
- TODO: Send a quick fix for MIME type support.
- TODO: Commence the QA (again!)

Discussion:
- QA 0.4.1:
    * Alex: Windows, MacOS M1, Fedora 37, Build .deb
    * Deeplow: MacOS Intel, Ubuntu 22.10, Build 2 .rpms
- Release candidates:
    * Remove PackageCloud logic for tags.
    * Create a `release-0.4.1` branch
    * Merge any PRs there that *must* go in 0.4.1
    * We can merge PRs on the main branch if we *don't* want them in 0.4.1
    * Tag the first commit of `release-0.4.1` as `v0.4.1-rc1`
    * Do the QA.
    * Once we tag the final commit as `v0.4.1`, then we merge `release-0.4.1` to the `main` branch.

# Thursday - 2023-03-23

Agenda:

- release check-in
    - Creds for infrastructure have been disseminated
    - poetry update has broken a package - Alex will open issue
    - OpenSUSE 502 errors
    - Onionshare/Dangerzone installation issue only affects users runing old binaries: https://github.com/freedomofpress/dangerzone/issues/153#issuecomment-1479354403
    - Discussion with Micah to come regarding packagecloud / potentially breaking migration to packages.freedom.press which would require manual user intervention
    - Reconvene on Tuesday next week for another release check-in (look out for meeting invite)
- early discussion about Qubes PoC and some post-0.4.1 planning on that
    - Action: Ro to share some experiences and findings with Alex, deeplow & Allie (optional)
- (if we have time) a bit of light brainstorming about future DZ capabilities.

# Wednesday - 2023-03-22

Alex:
- Configured a new Dangerzone environment on a Windows VM
- Worked a bit on [dangerzone#153](https://github.com/freedomofpress/dangerzone/issues/153).
    * Found out that it only affects 32-bit versions of OnionShare, so that's a different story.
- TODO: Resolve dangerzone#153
- TODO: Fix the MIME type issue

Deeplow:
- Create github action for SSH debugging github actions via onion service
- merge languages removal PR
- provide feedback on "Dangerzone; beyond tools" doc, which evaluates the feasibility of expanding Dangeronze
- debugging issue ssh connection to github actions
- TODO: research if there is a way to mark repo as deprecated
- TODO: do a PR for running the bulk tests locally (without the GH actions part)

Discussion:
- 0.4.1 announcement on PackageCloud:
    * Took a look at differential updates, Debian/Fedora don't seem to support it.
    * Maybe research if there's a way to deprecate an APT/YUM repo.
    * We will handle the removal of the PackageCloud in the installation instructions for packages.freedom.press, as a note for people coming from Dangerzone 0.4.0.
    * Alert box on Dangerzone start. no post install script because we want to promt the user about updating repos
    * Check if we can do "automated" button which will prompt users for their sudo pass and update the repos
    * Our 0.4.1 release will be both on PackageCloud (last time) and on packages.freedom.press.
- Large test update
    * Maybe reprioritize it after the 0.4.1 work, just for local development.
- Onionshare issue
  * it only affects 32-bit windows
  * Related issue: https://github.com/onionshare/onionshare/issues/1557

# Monday - 2023-03-20

Deeplow:
- created https://github.com/deeplow/action-ssh-onion-service to ssh into github actions runners
- found strategy to make have dangerzone run on github actions but feels hacky and creates other problems. Needs more research (probably Alex can tackle this one better).
- TODO: ping Erik about posting on mastodon about how Dangerzone could have protected against google pixel redaction failures
- TODO: warn packagecloud users
   - change repo is possible?
   - if not possible

Alex:
- Basically merged the vast majority of the open PRs.
- Did some research on how Dangerzone can sanitize other multimedia types
- Helped a bit with benchmarking and GitHub actions stuff.
- TODO: Send a fix for the MIME type issue
- TODO: Open an issue for running Dangerzone as a user other than 1000.

Discussions:
- Github actions runner. Creating this is not straightfoward due to some key differences:
  - CI user called runniner and with `uid=1001` instead of `uid=1000`. Our `env.py` (which creates a container to run Dangerzone on) isn't preparef for that.
  - Because of this, podman in the env fails to run due to insufficient permissions (running as user but ~/.local/share/containers is owned by docker)
  - They also have another user calledunneradmin
  - Let's deprioritize this task?

# Wednesday - 2023-03-15

Alex:
- Worked on finding the root cause for the faster OCR performance on 0.4.1, compared to 0.4.0.
  * Trigger: During our benchmarks, we could reproducibly demostrate that the OCR step was significantly faster on 0.4.1. The problem here is that we hadn't changed something in this particular step for 0.4.1.
  * Test subject: https://workstation.securedrop.org/en/stable/SecureDropWorkstation.pdf
  * Times for converting the above document:
    - 0.4.0 - No OCR : 52s
    - 0.4.0 - With OCR: 250s
    - 0.4.0 - Just OCR: **198s**
    - 0.4.1 - No OCR: 20s
    - 0.4.1 - With OCR: 177s
    - 0.4.1 - Just OCR: **157s**
  * Hypotheses:
    1. The image size (RGB file) is different between 0.4.0 and 0.4.1, due to `pdftoppm`.
       This hypothesis is **wrong**, since the image width and height are the same (1275x1651) in both versions. Given that RGB is a raw image format (i.e., with no compression), the image size should be the same.
    2. Another comamnd (probably `gm`) is the one to blame for this difference.
       This hypothesis is **wrong**. I verified by timing the runtime of each command in the container that `tesseract` is responsible for the 198s vs 157s difference.
    3. OCR does not work / do a good job on 0.4.1.
       This hypothesis is **wrong**. The image quality of the resulting PDF and the character detection seem to work the same in 0.4.0 and 0.4.1, at least on some random samples I chose.
    4. The RGB file produced by `pdftoppm` is more "readable".
       This hypothesis **may be** correct. While the RGB files have the same size, I saw that the PNG files, as produced by the RGB files, differ in size. In general, the 0.4.1 PNGs were slightly bigger in size, which **could mean** that the original files contain more info and are thus harder to compress.
    5. In 0.4.1 we also installed poppler-data, a package with additional fonts. It could be that tesseract-ocr was trained on these sets and thus it scans them faster.
  * Conclusion: Whatever the underlying reason for this performance speed-up, may understanding is that it seems to be benign and it shouldn't worry us.
- Discovered an issue with our MIME types. We don't handle `application/zip` and `application/octet-stream`, meaning that we ignore some perfectly valid files (dangerzone#369).
- TODO: Create a spreadsheet with our benchmark results.
- TODO: Send a fix for the MIME type issue

Deeplow:
- continue CI 200 doc short test
  - run into issues with git-lfs in CI "bad credentials" appears to be a bug in git-lfs
  - move on test on github CI
  - running into issues with "permission denied" when creating files in home folder of CI

Discussion:
- Insights from the benchmarks:
  * No regressions for now
  * The OCR speed up seems to be benign for now
  * 1.5x speed up for small files and 4x speed up for files with lots of pages (OCR enabled)
  * 4% less failures due to PDFtk
  * both versions fail on ~380 documents due to unsupported format ([dangerzone#369](https://github.com/freedomofpress/dangerzone/issues/369))
  * analysis limitation: we didn't do visual diffing on PDFs to see if there were rendering issues (appart from the occasional manual inspection)
  * analysis limitation: We ran the tests on bare metal, so timeouts will be less rare than in Windows/MacOS VMs.

# Monday - 2023-03-13

Deeplow:
- merge: Fix "Choose..." button not opening dir selection dialog on Qt6 (#361)
- final PR review and approval of poetry PEP 668 compliance (#353) (latest poetry CI breakage)
- review all other pending PRs
- revisit 0.4.0 large tests (and fix issue causing wrong result)
- propose changes to changelog (dangerzone#366)
- Investigate tesseract library not reporting issues on 0.4.1 but reporting them on 0.4.0: turns out 0.4.1 reporting was not being applied to second container whereas in 0.4.0 branch it was
- Adapt website to have two mac downloads (platform-specific) - dangerzone.rocks#10
- create PR for large_doc_set + CI for running daily on subset

Alex:
- Merged most of the pending PRs
- Sent a PR for our changelogs, and a PR for fixing a minor issue when building a container image.
- Started looking on performance comparison between 0.4.0 and 0.4.1.
- TODO: Create a spreadsheet with the results
- TODO: Find the root cause for the tesseract delay on 0.4.0.
- TODO: Understand why a specific document fails to convert.

# Thursday 2023-03-09

Agenda:
- Agree on must-do remaining tasks prior to release
- Identify any stretch goals if we have time due to release infrastructure readiness delays
    - Use the `stretch goal` label for these issues

Action items:
- (deeplow/Alex) Create a spreadsheet or doc with results from different team test runs and share with the group (we'll potentially share these results publicly in a post)
- (deeplow) Run same tests against 11,000 docs for Alex to include in the spreadsheet
- (Alex) Find out the root cause for the OCR delays on 0.4.0.
- (Alex) Try to understand why a certain document that can be rendered properly still fails on Dangerzone (stretch goal)
- (Maeve) Add logic to verify signature for RPM releases (see https://github.com/freedomofpress/yum-tools-prod/blob/main/scripts/publish.py#L29 and https://github.com/freedomofpress/securedrop-yum-prod/blob/main/.circleci/config.yml)
- (Erik) Check in with Riley on Windows infrastructure

Notes:
- update on speed difference from 0.4.0

    - seeing inconsistent results around performance between 0.4.0 and 0.4.1

    - how we are splitting pdfs into mutliple images might have something to do with it

    - without ocr, 0.4.1 is faster

    - with ocr, 0.4.1 is even faster

    0.4.0 on [X platform/hardware] with [Y features] on [Z dataset]: performance  / error rate

    0.4.1 on [X platform/hardware] with [Y features] on [Z dataset]: performance  / error rate

    - deeplow's run:

      * 0.4.0 on Fedora 37 with OCR disabled on 200 docs: 823s

      * 0.4.1 on Fedora 37 with OCR disabled on 200 docs: 595s (x1.4 speedup)

      * 0.4.0 on Fedora 37 with OCR enabled (eng) on 200 docs: 3098s

      * 0.4.1 on Fedora 37 with OCR enabled (eng) on 200 docs: 1281s (x2.4 speedup)

    - Alex's run:

      * 0.4.0 on Ubuntu Focal with OCR disabled on 200 docs: 759s

      * 0.4.1 on Ubuntu Focal with OCR disabled on 200 docs: 478s (x1.6 speedup)

      * 0.4.0 on Ubuntu Focal with OCR enabled (eng) on 200 docs: <took several hours, lots of timeouts, CPU thermal throttling is suspect)

      * 0.4.1 on Ubuntu Focal with OCR enabled (eng) on 200 docs: 1218s

    - All tests, except were noted, fail on ~50 docs, with "The document format is not supported" on *both* versions.

# Monday - 2023-03-06

Deeplow:
  - Fix the Choose folder bug on MacOS.
  - Wrap up the Fedora 37 QA
  - "investigate why dangerzone description on windows is still wrong" - in the end it was correct, but just not displayed by default (opened dangerzone#359)
  - run large tests on 0.4.0 (dangerzone#352) - tests were running over the weekend and still haven't finished
  - TODO: re-review dangerzone#353
  - TODO: The tesseract library
  - TODO: adapt website to have two mac downloads (platform-specific)
  - TODO: prepare some release notes to toot about substantial facts (when doing the changelog Alex will give some user-noticeable changes)

Alex:
- Finished QA testing on Ubuntu Kinetic
- Found an issue in our Debian packages, built from Ubuntu Focal.
  * Basically the entrypoint is different across OS, due to the setuptools version.
  * I've tested with a .deb produced by Debian Bookworm, and it seems to work cleanly across platforms
- I tried to create a GitHub actions job that tests the above:
  * Create a dev environment for Debian Bookworm, produce a .deb, create an end-user environment in all platforms, install the .deb there.
  * I tried to do so using caching, but it seems that the cache gets busted when building more than one Docker image. We don't exceed the 10GiB limit, so this is very weird.
  * TODO: Create a GitHub actions job regardless, even with no caching involved, to help cementing our hypothesis that .debs produced by Debian Bookworm can be used in all of our Debian-based distros.
- Sent an update for dangerzone#353, since the new Poetry installation methods failed on Ubuntu Focal and Debian Bullseye.
- Sent a PR for bumping our timeouts (dangerzone#363)
- TODO: (leftover): Send some fixes for the release instructions.
- TODO: Review dangerzone#{356,361}
- TODO: Send a PR for Fedora instructions, same as we did for the Debian one.
- TODO: Finalize the Debian instructions PR
- TODO: Create a lint for changelogs

Discussion:
- The tesseract library no longer outputs a message for calculating the size of the page.
  * Why does it do that though? Have we changed something? We need to investigate.

# Tuesday - 2023-02-28

Alex:
- Finished QA testing on MacOS M1
- Sent a PR for our recent CI breakage due to PEP 668 (dangerzone#351)
- Started working on Qa for Ubuntu Kinetic.
- TODO: Wrap up the Ubuntu Kinetic QA
- TODO: Send some fixes on the release instructions
- TODO: Produce a .deb that works on all Debian-based environments.
- TODO: Reproduce some MacOS findings during QA.
- TODO: Send a PR for the timeout issues

Deeplow:
- improve CI MSI building check (dangerzone#347)
- 11K docs test - summarize test results (it took 20h45m ~ 8s/document)
- run QA on windows and fedora 37
- TODO: Fix the Choose folder bug on MacOS. (deeplow)
- TODO: Wrap up the Fedora 37 QA (deeplow)
- TODO: investigate why dangerzone description on windows is still wrong
- TODO: dangerzone#352

Discusssion:
- Timeouts: We have seen that the .ppt file may timeout in our tests.
  * Let's have a minimum timeout of 1 minute, and multiply the proportional timeout by x3
- General todos:
  * Changelog / Version bump
  * Providing instructions for packages.freedom.press (Fedora)
  * Update the website to provide different links for MacOS architectures.

# Monday - 2023-02-20

Deeplow:
- feature-comparison with PDF Redact Tools (deprecated) add issue about documenting this
- fix issue with macOS container generation PR
- (final) review of "improve directory handling" PR (dangerzone#336)
- basically done with work on large doc test
  - make way of rebuilding the documents library and updating it
  - run test over several thousand docs and analyse results for timeouts
- merged arm64 docker image PR (dangerzone#337)
- looking a bit more into International Journalism Conference about possible place to do user research
- Write some initial notes about Dangerzone - SecureDrop integration in Qubes
- TODO: review pending PRs

Alex:
- Sent a PR for build issues for Ubuntu Focal
- Sent a PR for a PySide2 issue... in Ubuntu Focal
- Sent a PR for replacing references to First Look Media in the code.
- Merged some PRs that I sent (debian packaging PR for apt-tools-prod, temp dirs PR)
- TODO: Test packages.freedom.press locally and send a PR with instructions for user and dev
- TODO: By the might of Zeus, I **must** install Windows
- TODO: Send a PR for yum-tools-prod

# Wednesday - 2023-02-15

Alex:
- Wrapped up Debian packaging PR (dangerzone#322)
- Sent a PR with the resulting .debs to the apt-tools-prod repo
- Stumbled into some .deb build issues on Ubuntu Focal.
  - TODO: Send a PR for these issues.
- Replied to open questions on several open PRs
- TODO: Send fixes to my open PRs

Deeplow:
  - almost done with the testing

# Monday - 2023-02-13

Deeplow:
  - Finished reviewing of proportional timeouts PR (dangerzone#332)
  - large test set - analysis of results sorting throug common error types

Alex:
- Sent a PR for our temp files issue (dangerzone#336)
  * Also fixed a long standing issue wrt "permission denied" errors (dangerzone#335)
- TODO: Wrap up Debian packaging PR, based on our team meeting.
  * Do we need a .deb for each Debian distro?
- TODO: Open PR on the LFS repo, that will provide 0.4.0 .debs (currently for  testing)
- TODO: Reply to comments on my PRs.

Discussion:
- Qubes meeting preparation:
  * Introduce Dangerzone:
    - Based on TrustedPDF, which they can wrap their heads around. Where do we diverge though?
      * OCR
      * Batch conversion (with throttling)
      * 2-step process: second step also happens in container for practical reasons.
      * Multi-platform
      * Multiple filetype support.
      * Timeouts
      * Progress reports
      * Compresssion
      * (not ideal) we print the attacker-controlled message to the UI.
  * Question time: Given the above, if one were to piggy-back on the TrustedPDF architecture (dom1 <-> dom1 communication), would there be an issue? Would we be able to get progress reports? Would we be able to pass parameters for the conversion process?
- Large dataset:
  * We need to log the command output within the container (either failed or success cases). The reason is that some commands may complete successfully, but they may throw errors in their stderr, to indicate that the document is corrupted (think LibreOffice - Java issue).
  * To do so, the container will prepend the logs with a special prefix, and the host will be able to handle it.
  * To store these logs in files, the host must be in dangerzone deb mode, and the user needs to pass an environment variable to also enable command logging.
  * If we are in a dangerzone dev mode -> we print those logs to stderr.
  * If we also are instructed to write the logs in a file -> we also write these logs to a file.
  * If we are not in a dangerzone dev mode -> log an invalid json error

# Wednesday - 2023-02-08

Alex:
- Sent a PR for our CI issues.
  * Merged the PR as well, and now our CI is green again.
- Reviewed the PRs that deeplow sent.
- Pinged Maeve for the Debian PR.
  * Will reply to my comments soon.
- Drafted a status report about the 0.4.1 milestone.

Deeplow:
  - preview DZ sanitization proposal (engineering#11)
  - continue working on large test set

Discussion:
- large test test:
  - always run with --ocr
  - store expected output and result (pass/fail)
  - if the expected result diverges from the expected
  - consider increasing parallel number documents
  - large test set discovery: we'll have it as a submodule with a pytest

Action points:
- Work on the temp dirs issue, and send a PR.
- Nail down the last bits for the packages.freedom.press workflow.
- Deeplow: document above action plan in https://github.com/freedomofpress/dangerzone/issues/334

# Monday - 2023-02-06

Alex:
- Sent a PR for proportional timeouts.
  * Contains a commit that allows users to tweak the timeout value. Can be dropped if we don't like it.
- Looked into the building PySide2 from the Debian repos.
  * Not fun, we may have to create wheels and bundle them with our project.
  * At this point, one has to ask if it's better using PySide6 wheels for development, and PySide2 packages only for user environments.

Deeplow:
- look into why certain deps are failing
- look into adding PySide2 as dependency from source
- briefly taking a look at the PR for fixing timeouts (didn't test it yet)
- looking into testing a large document set a logging outputs to discover timeouts
- fix various things found while converting those documents
- made taiscale work in Qubes (installed only on appVM)

Discussion:
- Along with cleaning tmp dirs ([dangerzone#317](https://github.com/freedomofpress/dangerzone/issues/317)), should we also copy the input file to a temp dir (effectively what we do in https://github.com/freedomofpress/dangerzone/pull/248), so that we help users mount in the Dangerzone container any file that they can read?
- We'll be using PySide6 exclusively in the development environment but when shipping to linux distros, we'll use PySide2. It will be tested anyways in QA before any relase. Context: https://github.com/freedomofpress/dangerzone/issues/330

Action Points:
  - Deeplow: finish reviewing of proportional timeouts PR (dangerzone#332)
  - Deeplow: Continue work on large document tests
  - Deeplow: open PRs for fixing issues during document set conversion tests
  - Deeplow: add PR for running daily some document conversion tests
  - Deeplow: go through my PRs and merge if approved and they pass the CI (dangerzone#332)
  - Alex: Fix CI (PySide6)
    * Work on top of the `2023-01-lint` branch.
  - Alex: Review the PRs that deeplow has sent
  - Alex: Work on temp dirs issue, and also create a temp dir for input files (so that we fix the various permissions denied issues)
  - Alex: Ping Maeve for the Debian PR.

# Wednesday - 2023-02-01

Alex:
- Sent a PR that fixes pdfinfo and OpenJDK issues.
  * With this PR, the conversions should start working again.
- Working on introducing proportional timeouts.

Deeplow:
- Updated branch based on changes by @AlexP in adding pyside6 support for mac/windows
- Checking container code for non-doc dependent timeouts (they should depend on the doc's size)
- side-tracked with investigating how some inter-vm stuff works (could be interesting from SD-DZ communication)
- exit with code 1 when one doc failed to convert (dangerzone#329)
- review dependencies fix (dangerzone #328)
- investigate the multiple failures in the CI

Discussion:
  - Fixing our multiple CI versions
    - convert-tests: unpin Poetry 1.2.2, and use --no-ansi flag.
    - PySide2 on bookworm and fedora37 (where there is python 3.11) - get from debian sources to it actually supports - HACK use a git repo to get salsa.debian.org PySide2 version

Action Points:
- Alex: Press pause on timeout PR, review the infra PR
- Deeplow: Write a failing test for timeout and look for large pool of documents

# Monday - 2023-01-30

Alex:
- Fixed a regression in our Fedora 37 package ([dangerzone#156](https://github.com/freedomofpress/dangerzone/issues/156#issuecomment-1403237354)
- Continued the work on [dangerzone#296](https://github.com/freedomofpress/dangerzone/pull/296)
  * We can switch to types-PySide2 instead of PySide2-stubs, and sidestep the linting errors on MacOS M1.
- Opened [dangerzone#320](https://github.com/freedomofpress/dangerzone/issues/320) to track the PySide6/Mypy problem
- Started working on [dangerzone#315](https://github.com/freedomofpress/dangerzone/issues/315)
- Opened [dangerzone#321](https://github.com/freedomofpress/dangerzone/issues/321) to track our visual testing needs for our CI tests.
- Leftovers: Send PR for OpenJDK, LibreOffice conversion warnings.

Deeplow:
- Reviewed hotfix branch branch that fixed #307 (UI did not start in fedora 37)
- Merge #302 (add isolation providers)
- Create issues for unintentially introduced / other discovered bugs bugs from #305
  - #315 removal of java dependency lead to broken .xls files
  - #316 some normal container output is being printed back to the host.
  - tesseract (OCR scanning) is guessing image's DPIs even though we provide it via --dpi (will need investigation), but seems minor.
- Final test and closing PR for showing exceptions raised during a document conversion  (#313)
- open PR for supressing container output when it was not meant to be parsed (#326, which fixes #316)

Discussion:
- Enforce updating changelog when closing an issue.
  * Preferably add a lint that checks if a commit closes an issue without updating the Changelog (duh).

Action points:
- Alex: Create an issue for converting a 1000 pages PDF, and adding it to our 0.4.1 milestone.
- Alex: https://github.com/freedomofpress/dangerzone/issues/317
- Deeplow: (test-driven) make pdfunite step dependant on num pages
- Deeplow: https://github.com/freedomofpress/dangerzone/issues/318
- Deeplow:  Container: Fails to calculate number of pages #325
- Alex: Update #296 with the latest state of the PR, and what deeplow should do.
- Alex: Create signed per-distro packages in apt-tools-prod (w/ test key)
  * Current state is that we have signed packages, but not with a unique name per distro.
- (from meeting) go through docs and see if there was any other missing characters due to java or anything else

# Wednesday - 2023-01-25

Alex:
- Looked into [dangerzone#294](https://github.com/freedomofpress/dangerzone/issues/294)
  * Qt devs still haven't published a PySide2 version that supports Python 3.11
  * TODO: Open an issue for that on their issue tracker.
- Looked into our Fedora 37 installation issue:
  * Root cause: we deployed a Fedora 35 package to our Fedora 37 repo, which installs Dangerzone for Python 3.10
  * Suggestion: Create a new package (`0.4.0-2`) solely for Fedora 37, push it in a `hotfix-0.4` branch and tag it as `v0.4.0-2`

Deeplow:
- debugging CI issues on windows
- merge CI failure (re-fix) (dangerzone#312)
- add timeout proportional to # of pages in to PDF->Images code and merge (dangerzone#232)
- debugging multiple issues introduced by dangerzone#306 after merging (didn't find the root cause yet)

Discussion:
- How to find bugs in the conversion process. Currently our conversion process supresses error messages from the ran commands (e.g. libreoffice, tesseract).
  - We need to get the output of these commands in development mode
- Security Idea: move conversion error strings to host, container sends numeric ID and page number.
- Visual document diffing tests for catching changes (no, Dangerzone does not convert deterministically -- hashes are not possible)
  - https://pypi.org/project/diff-pdf-visually/#description
  - we only have to run this on linux tests
- Fedora 37 package issue:
  * There is a `hotfix-0.4` branch that is ready to tag as v0.4.0-2.
  * Only Fedora 37 packages will be deployed.
- General packaging improvements:
  * The following are suggestions only.
  * Ideally do not bundle Dangerzone with the .deb/.rpm package.
  * Let the builder build those packages, add them in the Git LFS repo, sign them and send the PR.
  * Let the maintainer verify these packages out-of-band (compare with source files), and resign the packages, and accept the PR.

Action items:
- deeplow: Check hotfix-4.0 for fedora branch https://github.com/freedomofpress/dangerzone/commits/hotfix-0.4
- deeplow: merge #302, then #303
- deeplow: address review comment in #313
- deeplow: After #313 is merged, open PR for exiting with non-zero if conversion failed (cli + gui)
- deeplow: report issue where tesseact guesses DPI even though it's passed as an argument.
- deeplow: (stretch) investigate error reporting in GUI: if unexpected conversion message it will stop showing progress information and just freezes. But in reality the document is still converting
- Alex: Fix the Fedora 37 packaging issue.
- Alex: Take over dangerzone#296.
- Alex: Add JDK again as dependency and test affected file conversion dangerzone#315
- Alex: Check why/if `__pycache__` files exist in RPM packages.
- Alex: open issue for discussed visual output diffing
- Alex: Check out if LibreOffice has a way to report conversion warnings.

# Monday - 2023-01-23

Deeplow:
- reorganize isolation provider PR to have isolation providers as separate files
- investigate a bit PyMuPDF as dependency alternative for processing PDF files. ((part of dangerzone#305)
- Make PDF conversion faster (dangerzone#305)
- Meeting w/ infra team for updates regarding the readyness for the next release

Alex:
- Fixed Poetry issue in our CI builds.
- Worked on packages.freedom.press
  * Tested it out on a container.
  * Wrote down the current workflows for devs/users.
  * We need to improve these workflows, especially as development requirements are ~2.5hours of build time and 45GiB of space.
- Made a review pass of all the open PRs
- Left out: Fedora 37 instructions, setup Thinkpad

Discussion:
- What to do about the lint on macOS? (dangerzone#296)
  - create an issue that we can't have mypy liting on ARM macs and pyside6
  - change make lint to say: "on m1 macs it can't run; see issue #???"
- packages.freedom.press workflow:
  - It's not practical to have the package building take so much time and space.
  - Might be worth having a runner and verifying the built packages locally.
  - Else, we will have to bite the bullet and do it ourselves.

Action items:
- deeplow: simple fix for (discussion on macOS linting)

# Wednesday - 2023-01-18

Alex:
- Reviewed all open PRs except for dangerzone#310
- I haven't tested the dangerzone#305 PR, because there are things that contend for disk space.
  * I think I will have disk space today.
- Stumbled on an issue regarding apt-tools-prod: there's a hash mismatch type of error that I'm not sure how to deal with.
  * I've started a discussion with Maeve on that subject.
  * I'll work on updating our installation instructions for packages.freedom.press on Debian, so that we can have them ready once the apt-tools-prod issue is resolved.
  * Fun fact: At least in our 0.4.0 release (haven't checked yet on `main`), the Debian debs have the exact same hash. Same applies to the Ubuntu debs, but they have a different hash from the Debian one. Not sure how we can use this fact, as it can change from release to release.

Deeplow:
- investigating why exceptions in conversion process where not raised (opened issue at dangerzone#309)
- retest pyside6 support in remote mac (dangerzone#296)
- address feedback in isolation provider abstraction (dangerzone#302)

Action items:
- Alex:
  * Review dangerzone#310
  * Test dangerzone#305
  * Second pass of the rest of the PRs.
  * Recheck the state of dangerzone#294 (Fedora 37 build environment)
  * Create PR for packages.freedom.press instructions
  * (stretch) Start seting up Thinkpad for Windows testing
- Deeplow:
  * TODO: continue addressing PR feedback
  * TODO: What's the state with RPM updates.

# Monday - 2023-01-16

Alex:
- Reviewed dangerzone#{295,296,297,301,302}
- Will merge today dangerzone#289
- TODO: Review dangerzone#30{3,4,5}
- TODO: Push the latest 0.4.0

Deeplow:
- TODO: Fix the mypy lint errors on the PR for PySide6
- TODO: Finish addressing open PR comments
- TODO: Tuesday - check how the RPM package publishing status is
- TODO: (strech) try to reproduce again dangerzone#153 (installing w/ onionshare)

# Wednesday - 2023-01-11

Deeplow:
- Submited PR to dangerzone.rocks to add security note in about.html (dangerzone.rocks#8)
- Merged Qubes build instructions (dangerzone#284)
- Finished removal of unused dependencies in container image dangerzone#305 (PDFtk in particular)
- Continue work on seccomp stuff. Reading up the paper behing the "confine" tool and playing around with it

Alex:
- Addressed all the review comments on dangerzone#289.
- Added some extra environments for testing (Fedora 35, Debian Bookworm, Ubuntu Jammy).

Discussion:
- using "confine" approach for seccomp policy
- considering gvisor - doesn't need as many syscalls

# Monday - 2023-01-09

Deeplow:
- merge dangerzone.rocks#7
- Addressed comments in dangerzone#288 - ready for merge
- dangerzone#284 is ready to merge -- additonal changes will follow in another PR (as they were extra)
- dangerzone#248 - Implement Linux User Namespaces support - reviewed
- discussion with @eaon about SecureDrop integration details
- TODO: submit PR to dangerzone.rocks to add security note in about.html
- TODO: re-evaluate dependencies dangerzone#232

Alex:
- Made almost all of the changes we discussed on dangerzone#289.
  * We have some deps that where missing, and some system libraries that
- Testing those changes locally and on our CI.

# Wednesday - 2023-01-04

Alex:
- Going through the review comments on dangerzone#289

Deeplow:
- Moving on with dangerzone#229, where we have in the works a dummy driver for Dangerzone conversions
  * This dummy driver will not do any actual conversion, which means that it does not test a class of problems. It will merely test that pre-conversion / post-conversion works.
- TODO: outstanding tasks from last meeting
- dangerzone#229: add windows / macOS to CI and run the cli tests

# Monday - 2023-01-02

Deeplow:
- Reviewing QA semi-automation (dangerzone#289).
  - detect when platform build instructions changed (so qa.py and BUILD.md don't get out of sync)
- Reinstalled windows system and re-built testing environment
  - played a bit with installing dangerzone and onionshare at the same time to reproduce (unsuccessfully) dangerzone#153
- Show dangerzone version in CLI and GUI (dangerzone#295)
- Address PR feedback in DZ website (dangerzone.rocks#7)
- Make fix for "Open With" dialog on Windows showing the DZ description instead of the app name (dangerzone#297)
- Address comments about a potential DZ flatpak version
- Add PySide6 support to MacOS and Windows (dangerzone#296)
- Played around with bundling pyside6 with .debs (will be needed see dangerzone#211)
  - In particular, investigate bundling PySide6 .whl with an RPM  package
- start work in incapsulating isolation provider (containers) as a path towards dangerzone#229

Alex:
- Fixed an issue in the Circle CI builds (https://github.com/freedomofpress/dangerzone/pull/293)
- Review comments on some PRs (dangerzone#288)
- Stared working on using packages.freedom.press for Debian packages.

Action points:
- deeplow: update issue dangerzone#229 and continue working on it
- Alex: Address feedback on dangerzone#289.
- Alex: Review dangerzone#{295, 296, 297, 300}
- deeplow: merge dangerzone.rocks#7
- deeplow: address comments in dangerzone#288
- deeplow: ping ro about dangerzone#284

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
