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
