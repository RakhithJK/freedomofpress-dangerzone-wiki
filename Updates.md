Dangerzone users are recommended to always install the latest updates, since
they contain stability and security fixes. In order to get notified about new
updates, we have introduced a new mechanism in version 0.4.2 that checks our
[GitHub releases page](https://github.com/freedomofpress/dangerzone/releases)
and informs the users about new versions.

In this page, we will explain how this mechanism works, how users can disable
update checks, as well as some common causes of errors.

## How it works

The first time a user runs Dangerzone, the Dangerzone UI will not ask them if
they want to enable update checks. The second time, it will present a dialog
window where they can choose if they want to enable update checks or not.

Enabling update checks means that Dangerzone will contact GitHub to learn about
new releases when a user starts the application. This will happen at most twice
a day.

If Dangerzone detects a new release, it will show a notification bubble in the
UI. Users can click on the notification and learn more about the new release.
They are then instructed to go to our [downloads page](https://dangerzone.rocks/#downloads)
and install the update.

If an error occurs while checking for updates, users will be notified about it
with a red notification bubble. Then, they can click on the notification and
find the cause of the error.

For more technical details, read the
[corresponding issue](https://github.com/freedomofpress/dangerzone/issues/189).

## Q&A

### I see an error that points me to this page, what should I do?

Dangezone has tried to check about new updates by contacting this site, but it
was not able to do so. This may happen if your device is not connected to the
Internet, or if GitHub is experiencing an outage.

If you are unsure about the error message that you see in your screen, please
contact us by [opening an issue](https://github.com/freedomofpress/dangerzone/issues/new/choose).

### Can Dangerzone auto-install updates?

Dangerzone cannot auto-install updates, or auto-update itself. It instructs
instead the user to go to our [downloads page](https://dangerzone.rocks/#downloads)
and install an update from there.

### How can I enable/disable update checks?

You can always change your preference by opening Dangerzone, clicking on the
hamburger menu (☰) on the top-right corner, and clicking on the "Check for
updates" checkbox.

### Is there an alternative way to get notified about updates?

You can add our [Mastodon feed](https://fosstodon.org/@dangerzone.rss) to your
RSS reader, or click on the "Watch" button on the main page of this repo and
learn about new releases via email.

### I saw an error notification but I failed to copy the message. Where can I retrieve it?

Dangerzone contacts the GitHub servers at most every 12 hours. In between, it
doesn't perform any update check. So, for the time being, if an error persists,
you need to wait for 12 hours to pass before you trigger it again.
