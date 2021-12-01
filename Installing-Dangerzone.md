Dangerzone 0.3 is available for:

- Ubuntu 21.10 (impish)
- Ubuntu 21.04 (hirsute)
- Ubuntu 20.10 (groovy)
- Debian 12 (bookworm)
- Debian 11 (bullseye)
- Fedora 35
- Fedora 34
- Fedora 33

Dangerzone 0.2.1 is available for:

- Ubuntu 21.04 (hirsute)
- Ubuntu 20.10 (groovy)
- Debian 11 (bullseye)
- Fedora 34
- Fedora 33
- Fedora 32

Dangerzone 0.2 is available for:

- Ubuntu 20.04 (focal)
- Debian 10 (buster)
- Linux Mint 20 (ulyana)
- Linux Mint 20.1 (ulyssa)

If you use one of these older Linux distros you must [install Docker CE first](https://docs.docker.com/engine/install/).

### Debian, Ubuntu, Linux Mint

Add our repository following [these instructions](https://packagecloud.io/firstlookmedia/code/install#manual-deb), or by running this script:

```
curl -s https://packagecloud.io/install/repositories/firstlookmedia/code/script.deb.sh | sudo bash
```

Install Dangerzone:

```
sudo apt update
sudo apt install -y dangerzone
```

### Fedora

Add our repository following [these instructions](https://packagecloud.io/firstlookmedia/code/install#manual-rpm), or by running this script:

```
curl -s https://packagecloud.io/install/repositories/firstlookmedia/code/script.rpm.sh | sudo bash
```

Install Dangerzone:

```
sudo dnf install -y dangerzone
```

## Build from source

If you'd like to build from source, follow the [build instructions](https://github.com/firstlookmedia/dangerzone/blob/master/BUILD.md).
