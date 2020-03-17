## Install in Windows

Download Windows binaries from the [Releases](https://github.com/firstlookmedia/dangerzone/releases) page.

## Install in macOS

Download macOS binaries from the [Releases](https://github.com/firstlookmedia/dangerzone/releases) page.

## Install in Linux

### Debian or Ubuntu

Make sure you have `apt-transport-https` installed, and add the FLM code repository key:

```
sudo apt update
sudo apt install -y curl gnupg apt-transport-https
curl -L https://packagecloud.io/firstlookmedia/code/gpgkey | sudo apt-key add -
```

Add the repository, depending on your operating system:

- Ubuntu 19.04 (disco)
  ```
  echo "deb https://packagecloud.io/firstlookmedia/code/ubuntu/ disco main" | sudo tee -a /etc/apt/sources.list.d/firstlookmedia_code.list
  ```
- Ubuntu 19.10 (eoan)
  ```
  echo "deb https://packagecloud.io/firstlookmedia/code/ubuntu/ eoan main" | sudo tee -a /etc/apt/sources.list.d/firstlookmedia_code.list
  ```
- Debian 10 (buster)
  ```
  echo "deb https://packagecloud.io/firstlookmedia/code/debian/ buster main" | sudo tee -a /etc/apt/sources.list.d/firstlookmedia_code.list
  ```
- Debian 11 (bullseye)
  ```
  echo "deb https://packagecloud.io/firstlookmedia/code/debian/ bullseye main" | sudo tee -a /etc/apt/sources.list.d/firstlookmedia_code.list
  ```

Install Dangerzone:

```
sudo apt update
sudo apt install -y dangerzone
```

### Fedora

Follow [these instructions](https://docs.docker.com/install/linux/docker-ce/fedora/) to install Docker CE. Make sure enable the service. (Docker CE appears to work better than Docker that comes in the Fedora repositories.)

We have repositories for Fedora 30 and 31. Add this repository following [these instructions](https://packagecloud.io/firstlookmedia/code/install#manual-rpm), or by running this script:

```
curl -s https://packagecloud.io/install/repositories/firstlookmedia/code/script.rpm.sh | sudo bash
```

Install Dangerzone:

```
sudo dnf install -y dangerzone
```

### Build from source

If you'd like to build from source, follow the [build instructions](https://github.com/firstlookmedia/dangerzone/blob/master/BUILD.md).
