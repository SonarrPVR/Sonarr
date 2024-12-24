# <img width="24px" src="./Logo/256.png" alt="Sonarr"></img> Sonarr

[![Backers on Open Collective](https://opencollective.com/Sonarr/backers/badge.svg)](#backers)
[![Sponsors on Open Collective](https://opencollective.com/Sonarr/sponsors/badge.svg)](#sponsors)
[![Mega Sponsors on Open Collective](https://opencollective.com/Sonarr/megasponsors/badge.svg)](#mega-sponsors)

Sonarr is a personal video recorder tailored for Usenet and BitTorrent users. It can track several RSS feeds to find new episodes of your favorite series, then download, organize, and rename them automatically. It also offers the option to upgrade the quality of already downloaded files when a higher-quality version becomes available.


To install Sonarr, follow the instructions according to your operating system.


| Jump to: | [Windows](https://github.com/SonarrPVR/Sonarr?tab=readme-ov-file#Windows) |[MacOS](https://github.com/SonarrPVR/Sonarr?tab=readme-ov-file#MacOS) | [Linux](https://sonarr.tv/#downloads-linux) | [Docker](https://github.com/SonarrPVR/Sonarr?tab=readme-ov-file#Docker) | [Other platforms](https://sonarr.tv/#downloads-linux) |
| :--- | :---: | :---: | :---: | :---: | :---: |


## Windows

Sonarr is supported natively on Windows. Sonarr can be installed as Windows Service or System Tray Application.

A Windows Service runs even when the user is not logged in, but special care must be taken since Windows Services cannot access network drives (`X:\` mapped drives or `\\server\share` UNC paths) without special configuration steps.
Additionally the Windows Service runs under the **Local Service** account, by default this account does not have permissions to access your user's home directory unless permissions have been assigned manually. This is particularly relevant when using download clients that are configured to download to your home directory.

It's therefore advisable to install Sonarr as a System Tray Application if the user can remain logged in. The option to do so is provided during the installer.

**Supported Versions:** Windows 8.1, Server 2012, or later.

### 1. Install Sonarr
Download the Windows Installer with the following link and execute it:
   - [Download Windows (x64) Installer](https://github.com/SonarrPVR/Sonarr/releases/download/4.0.11.2680/Sonarr.main.4.0.11.2680.win-x64-installer.zip)
   - [Download Windows (x86) Installer](https://github.com/SonarrPVR/Sonarr/releases/download/4.0.11.2680/Sonarr.main.4.0.11.2680.win-x86-installer.zip)

### 2. View Sonarr
Browse to `http://localhost:8989` to start using Sonarr.

**Manual Installation for advanced users:** It is possible to install Sonarr manually using the `.zip` downloads. However, in that case, you must manually deal with dependencies, installation, and permissions.

## MacOS
The easiest way to install Sonarr on macOS is to use the App archive with the steps described below.

### 1. Download App package
Download the app package:
   - [Download macOS App (ARM)](https://services.sonarr.tv/v1/download/main/latest?version=4&os=macos&arch=arm64&installer=true)
   - [Download macOS App (Intel)](https://services.sonarr.tv/v1/download/main/latest?version=4&os=macos&arch=x64&installer=true)

### 2. Install App
Open the archive and drag the Sonarr icon to your Application folder.

`In macOS 10.12+ Gatekeeper App Translocation will prevent Sonarr from updating if it's being run directly from the download location. Therefore it MUST be moved to the Application folder.`

### 3. Self-sign Sonarr
Open Terminal and run:
   ```bash
   codesign --force --deep -s - /Applications/Sonarr.app && xattr -rd com.apple.quarantine /Applications/Sonarr.app
   ```

### 4. Start Sonarr
Open `Sonarr.app` in your Application folder.

#### View Sonarr
Browse to `http://localhost:8989` to start using Sonarr.

## Docker

The Sonarr team does not offer an official Docker image. However, a number of third parties have created and maintain their own.
These instructions provide generic guidance that should apply to any Sonarr Docker image.

Updating Sonarr v3 to v4:

Most docker containers use /config volume to mount the data directory and supply that path to Sonarr as parameter. Sonarr will convert the given directory on startup if an existing database is found. Of course, it is always advisable to make a backup first.

### 1. Avoid common pitfalls
##### Volumes and Paths

There are two common problems with Docker volumes: Paths that differ between the Sonarr and download client container and paths that prevent fast moves and hard links.  
The first is a problem because the download client will report a download's path as `/torrents/My.Show.S01E01/`, but in the Sonarr container that might be at `/downloads/My.Show.S01E01/`. The second is a performance issue and causes problems for seeding torrents. Both problems can be solved with well planned, consistent paths.

Most Docker images suggest paths like `/tv` and `/downloads`. This causes slow moves and doesn't allow hard links because they are considered two different file systems _inside_ the container. Some also recommend paths for the download client container that are different from the Sonarr container, like `/torrents`.  
The best solution is to use a single, common volume _inside_ the containers, such as `/data`. Your Series would be in `/data/tv`, torrents in `/data/downloads/torrents` and/or usenet downloads in `/data/downloads/usenet`.

If this advice is not followed, you may have to configure a Remote Path Mapping in the Sonarr web UI (Settings › Download Clients).

##### Ownership and Permissions

Permissions and ownership of files is one of the most common problems for Sonarr users, both inside and outside Docker. Most images have environment variables that can be used to override the default user, group and umask, you should decide this before setting up all of your containers. The recommendation is to use a common group for all related containers so that each container can use the shared group permissions to read and write files on the mounted volumes.  
Keep in mind that Sonarr will need read and write to the download folders as well as the final folders.

_For a more detailed explanation of these issues, see [The Best Docker Setup and Docker Guide](https://wiki.servarr.com/docker-guide) wiki article._

### 2. Install Sonarr

To install and use these Docker images, you'll need to keep the above in mind while following their documentation. There are many ways to manage Docker images and containers too, so installation and maintenance of them will depend on the route you choose.

- ##### [ghcr.io/hotio/sonarr:release](https://ghcr.io/hotio/sonarr:latest)
    
    [hotio](https://hotio.dev/) doesn't specify any default volumes, besides `/config`. Images are automatically updated multiple times per hour if upstream changes are found. Read the [instructions](https://hotio.dev/containers/sonarr) on how to install the image.
    
- ##### [ghcr.io/linuxserver/sonarr:latest](https://ghcr.io/linuxserver/sonarr:latest)
    
    [linuxserver.io](https://www.linuxserver.io/) is one of the most prolific and popular Docker image maintainers. They also maintain images for most of the popular download clients as well. LinuxServer specifies a couple of optional default volumes such as `/tv` and `/downloads`. The default volumes are not optimal nor recommended. Our recommendation is to use a single volume for the data, as mentioned above.


### Others
For other platforms, refer to the specific installation instructions on the [Sonarr website](https://sonarr.tv).
