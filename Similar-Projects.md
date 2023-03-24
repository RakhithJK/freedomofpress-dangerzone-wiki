Other projects that aim to protect users from PDF exploits or privacy vulnerabilities.

## Qubes' Convert to Trusted PDF

Tktk. Source [here](https://github.com/QubesOS/qubes-app-linux-pdf-converter).

## Entrusted

A rust-based implementation of Dangerzone. Their homepage is available [here](https://github.com/rimerosolutions/entrusted).


## MediaWiki + PDF Handler Extension
MediaWiki software (the one Wikipedia runs on) converts PDF documents to images so that users visiting wikipedia don't run the risk of having their IP addressed leaked via an unsafe document viewer. They document PDF privacy risks [here](https://www.mediawiki.org/wiki/Help:Security/PDF_files) and the PDF converter implementation is available [here](https://github.com/wikimedia/mediawiki-extensions-PdfHandler/blob/7579f4f3adf069d84693c9a26414c16496ba4985/includes/PdfHandler.php).

As you can see on the picture below you, the software displays the PDF as a series of images and informs the users of the potential privacy risks of opening PDF documents directly.

![mediawiki](https://user-images.githubusercontent.com/47065258/227474044-4a1877d6-326d-4862-8e95-87ca034568d2.png)

## PDF Redact Tools (deprecated)

Arguable this is the predecessor of Dangerzone, but it focused more on safe redaction rather than santization of documents. The project is available [here](https://github.com/firstlookmedia/pdf-redact-tools).