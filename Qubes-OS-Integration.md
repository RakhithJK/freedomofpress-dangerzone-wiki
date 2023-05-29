*(Re-)integration with Qubes OS is the big step for Dangerzone and the primary goal of the 0.5.0 release.*

Integrating Dangerzone into Qubes OS is indeed a significant step for the project and aligns with its primary goal for the 0.5.0 release. Dangerzone was originally inspired by [Qubes Trusted PDF](https://blog.invisiblethings.org/2013/02/21/converting-untrusted-pdfs-into-trusted.html), which focused on converting untrusted PDFs into trusted ones within the Qubes OS environment. Dangerzone expanded on this concept by bringing the sanitization capabilities of Qubes to Windows, macOS, and Linux operating systems through the use of Docker and containers, and implemented features that are crucial for journalists and other users. These features include the ability to convert files beyond PDFs, such as images and office files, as well as compressing and making files searchable through optical character recognition (OCR).

By reintegrating Dangerzone back into Qubes OS, the project can leverage the superior isolation provided by Qubes’ virtual machines for document conversion but also serves as an essential stepping stone for the [next-generation workstation](https://securedrop.org/news/piloting-securedrop-workstation-qubes-os/). The SecureDrop project, which focuses on secure communication between journalists and anonymous sources, has been piloting the use of Qubes OS as the underlying operating system for the workstation. By incorporating Dangerzone into Qubes OS, the SecureDrop Workstation can benefit from enhanced file sanitization capabilities, ensuring the files are sanitized before being exported to less secure newsrooom machines.

  
Since Qubes integration requires some core changes in the way Dangerzone works, we propose doing the integration in three stages:

- [Alpha](https://github.com/freedomofpress/dangerzone/issues/411): Provide a rudimentary integration with Qubes OS. E.g., may not have GUI, or other important (but not security-critical) features, but the core functionality is there.
- [Beta](https://github.com/freedomofpress/dangerzone/issues/412): Improve the integration with Qubes OS. E.g., add GUI support and other important features, make the installation easier.
- [Stable](https://github.com/freedomofpress/dangerzone/issues/413): Finalize the integration with Qubes OS. There should be feature parity in all platforms, and the Qubes integration should be tested against a QA platform.


## Design Overview


We have two use cases in mind:

1. The typical Qubes user, who installs Dangerzone as a more feature-rich TrustedPDF
2. SecureDrop workstation, which uses Dangerzone as a sanitization VM.

In this section, we’ll focus on the first use case.

Dangerzone should be available to all qubes that a user will handle documents in. The easiest way to accomplish this is to install Dangerzone on the main template, i.e., fedora-37. This way, our conversion tools (LibreOffice / GraphicsMagick) will be available to disposable qubesVMs as well.  
  
Since Dangerzone also runs on Linux (Debian and Fedora), we need to differentiate between running Dangerzone using container isolation vs using Qubes isolation, and allow Qubes users choosing between the two methods. We recognise that the container isolation is subpar in a Qubes environment, but for alpha stage / dev purposes, allowing users to choose is important.  
  
To autodetect if we run in Qubes, the canonical way is to check if `/usr/share/qubes/marker-vm` exists (see [“What is the canonical way to detect Qubes VM?“](<https://www.qubes-os.org/faq/#what-is-the-canonical-way-to-detect-qubes-vm>)). If a Qubes user wants to use the container isolation instead, we can offer an environment variable that will enable it.  
  
Part of the initial configuration that a user needs to do in dom0 is to define an RPC policy that will allow any qube to start a disposable qube, and run the Dangerzone conversion service in it (let’s call it dz.Convert). This policy is detailed in its own section.

### Binary Protocol

![DZ - Qubes overview(1)](https://github.com/freedomofpress/dangerzone/assets/47065258/23275a4d-c043-48db-9a96-331c8dc919a3)

Once the Qubes isolation provider (think of it as the “client” component) starts, it needs to do the following things:

- Spawn a disposable VM not connected to the Internet using qrexec-client-vm @dispvm dz.Convert.

- Pass the document to be converted via qrexec from the app qube to the disposable qube.

- Read the page count and then for each page, read the page data (dimentions and pixels) through the read end of qrexec, and use them to show progress reports

  - Any data received by the app qube should be considered as untrusted.
  - The read operation should be performed in a non-blocking fashion

- Perform the second stage of the Dangerzone conversion (pixels to documents) in the app qube.

Once the Qubes disposable qube starts (think of it as the “server” component) it should do the following:

- Detects if it's running on Qubes, same way as the isolation provider does.

- Receive the document from the read end of the qrexec.

  - Containers read the mounted document from the filesystem instead.

- Convert the document to PDF, in case it’s in any other format

- Get the page count of the document, and send it through the write end of the qxexec

  - Containers send a progress report to the isolation provider

- Break each page into pixels . For each page, immediately convert it to pixels, and send the width and height and immediately afterwards,send the pixels through the write end of the qrexec.

  - Containers do things differently:

    - First they separate all the pages into PPM files.
    - Then they convert all the PPM files to pixels (RGB files), and store them in a mounted directory.
    - Finally, they store the page and height of each page alongside the RGB files.


## [WIP] Implementation Details

In this section, we’ll zoom in on specific parts of the Qubes integration that require a bit more explanation.


### MVP Differences

The above design overview aims to show the architecture of the final product. In order to have an MVP first, we need to take some shortcuts though. We’ll present these shortcuts here:

- Dangerzone will lack some important features: GUI, OCR, Progress report, Compression, Timeouts, Error handling
- The disposable VM will not stream data during conversion, but rather at the end. This will allow us not to change the `container/dangerzone.py` file and affect the container workflow for now, but wrap it with our Qubes code.


### Packaging

We can remove the container image from the final Dangerzone package, since it’s not necessary. Qubes users that want to use Dangerzone with Podman support can use our other packages.

On the other hand, we also need to add dependencies to our packages that were previously only on our container image (e.g., LibreOffice, GraphicsMagick). Besides the extra dependencies, our Qubes package should include an extra file called /etc/qubes-rpc/dz.Convert, which will act as the Qubes RPC service that will call the Dangerzone conversion code in the disposable qube.

Also, we need to create two packages for Qubes; one for Fedora 37 templates, and one for Debian 11 templates. Qubes-specific packages can be named `dangerzone-qubes-*`, and they should conflict with the regular Dangerzone packages for Fedora 37 and Debian 11.  
  
Finally, there may be use cases where users have different qube templates for disposable qubes and app qubes. These users may want to install the “client” dependencies of Dangerzone on the app qube template, and the “server” dependencies of Dangerzone (e.g., LibreOffice) in a separate disposable qube, to reduce the attack surface of their app qubes. If such a use case comes up, we’ll need to break the packages packages we mentioned above in two parts (`dangerzone-qubes-client-*` / `dangerzone-qubes-server-\*`).


### Qubes RPC Policy

The easiest way to configure a Qubes RPC policy for Dangerzone is to ask a user to create a file called /etc/qubes/policy.d/30-dz.policy in dom0, with the following contents:

```
dz.Convert    *    @anyvm    @dispvm    allow
```

This is a manual action by the user though that is performed during the first time of installing Dangerzone. If we want to update that file during one of our subsequent releases, we won’t be able to do so, as it is located in dom0.

In this case, it may be worth experimenting with the [Policy Admin API](https://www.qubes-os.org/doc/admin-api/#policy-admin-api).


### [WIP] Error Handling

Read stderr, sanitize non-printable, non-ASCII characters, log stderr if error code is 1 or dev mode.


### [WIP] Progress Reports

Client-side progress reports:

- “Getting page count” -> This phase includes LibreOffice / GraphicsMagic conversion and pdfinfo
- “Converting page x/N to pixels” -> Client reports progress while reading the full page data.


### [WIP] Binary Protocol

Read first 2 bytes for page count -> N pages

For each page:

- Read 2 bytes for page height
- Read 2 bytes for page width
- Read (width * height * 3) bytes for page data (the 3 is due to 3 color channels)


### Parallel Conversions

Our existing container implementation handles multiple files sequentially, but we have support for parallel conversions as well, by creating a separate container for each conversion.

In Qubes, spawning different disposable qubes per conversion does not scale well, so we may need to consider converting the whole batch of documents in the same dispVM. The security implications of this is that a malicious document may be able to tamper with other document’s appearance. Depending on how batches of documents are obtained, this may be a safe enough assumption to make.


### [WIP] Timeouts

Consider client timeouts if the server has not sent a byte for a while. These timeouts can be calculated client-side

  
  
  
