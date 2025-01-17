# Calibre-Web Auto

![Calibre-Web Automated](README_images/CWA-banner.png "Calibre-Web Automated")

## This fork disables automatic cwa-ingest due to reliability issues. This is for users who prefers a button to start the ingest process rather than automatic ingestion.
Some users (including me) will have issues when ingesting loads of files when using auto ingestion, which is why I disabled it

## _Quick Access_

- [Features](#what-does-it-do-) 🪄
- [Releases](https://github.com/crocodilestick/Calibre-Web-Automated/releases) 🆕
- [Roadmap](#under-active-development-%EF%B8%8F) 🛣️
- [How to Install](#how-to-install-): 📖
  - [Quick Install](#quick-install-) 🚀
  - [Docker-Compose](#using-docker-compose-recommended) 🐋⭐(Recommended)
  - [For Developers](#for-developers---building-custom-docker-image) 🚀
- [Post-Install Tasks](#post-install-tasks)
- [Usage](#usage-)
- [Further Development](#further-development-️) 🏗️

### **_Features:_**

- **Automatic Imports** - of `.epub` files into your Calibre-Web library ✨. Press "Ingest Books" to automatically import all books in the ingest folder

- **Automatic Conversion** 🔃 - of newly downloaded books in 28 other formats into `.epub` **for optimal compatibility** with the widest number of eReaders, library homogeneity, and seamless functionality with Calibre-Web's excellent **Send-to-Kindle** Function.

- A **Weighted Conversion Algorithm:** ⚖️
  - Using the information provided in the Calibre eBook-converter documentation on which formats convert best into epubs, CWA is able to determine from downloads containing multiple eBook formats, which format will convert most optimally, ignoring the other formats to ensure the **best possible quality** and no **duplicate imports**
- **28 Supported file types for conversion:** 🤯
  - _.azw, .azw3, .azw4, .mobi, .cbz, .cbr, .cb7, .cbc, .chm, .djvu, .docx, .epub, .fb2, .fbz, .html, .htmlz, .lit, .lrf, .odt, .pdf, .prc, .pdb, .pml, .rb, .rtf, .snb, .tcr, .txtz_

![Cover Enforcement CWA](README_images/cwa-enforcer-diagram.png "CWA 1.2.0 Cover Enforcement Diagram")

- **Automatic Enforcement of Changes made to Covers & Metadata through the Calibre-Web UI!** 👀📔
  - In stock Calibre-Web, any changes made to a book's **Cover and/or Metadata** are only applied to how the book appears in the Calibre-Web UI, changing nothing in the ebook's files on some e-reader devices.
  - CWA's **Automatic Cover & Metadata Enforcement Feature** makes it so that **WHATEVER** changes you make to **YOUR** books, **_are made to the books themselves_**, as well as in the Web UI, **making what you see, what you get.**

- **One Step Full Library Conversion** 🔂 - Any format -> `.epub`
  - Calibre-Web Automated is designed with `.epub` libraries as central, primarily due to it being the most **Compatible with the Widest Range of Devices**, **Ubiquitous** as well as being **Easy to Manage and Work with**

- **Simple CLI Tools** for manual fixes, conversions, enforcements, history viewing ect. 👨‍💻

  - Built-in command-line tools now also exist for:
    - Viewing the Edit History of your Library files _(detailed above)_
    - Listing all of the books currently in your Library with their current Book IDs
    - **Manually enforcing the covers & metadata for ALL BOOKS** in your library using the `cover-enforcer -all` command from within the container **(RECOMMENDED WITH FIRST TIME USE)**
    - Manually Enforcing the Covers & Metadata for any individual books by using the following command
    - `cover-enforcer --dir <path-to-folder-containing-the-books-epub-here>`
  - Full usage and documentation for all new CLI Commands can be found [here](#the-cover-enforcer-cli-tool)
    ![CWA Database](README_images/cwa-db-diagram.png "CWA 1.2.0 Cover Database Diagram")

- **Change Tracking Database** 📊 - In combination with the **New Cover & Metadata Enforcement Features**, a database now exists to keep track of any and all enforcements, both for peace of mind and to make the checking of any bugs or weird behaviour easier, but also to make the data available for statistical analysis or whatever else someone might want to use the data for
  - Full documentation can be found below [here](#checking-the-cover-enforcement-logs)

- ### Library Auto-Detect 📚🕵️
  - **New Users without existing Libraries:** 🆕
    - New users without existing Calibre Libraries no longer need to copy and paste `metadata.db` files and point to their location in the Web UI, CWA will now automatically detect the lack of Library in your given bind and automatically create a new one for you! It will even automatically register it with the Web UI so you can really hit the ground running
  - **New or Existing Users with Existing Libraries:**
    - Simply bind a directory containing your Calibre Library (search is done recursively so it doesn't matter how deep in the directory it is) and CWA will now automatically find it and mount it to the Web UI
    - Should you bind a directory with more than 1 Calibre Library in it, CWA will intelligently compare the disk sizes of all discovered libraries and mount the largest one
      - _CWA supports only one library per instance though support for multiple libraries is being investigated for future releases_
      - _In the meantime, users with multiple libraries who don't want to consolidate them are advised to run multiple, parallel instances_
- ### Dark/Light Mode Button Switcher ☀️🌙
- ### Internal Update Notification System 🛎️
  - Automatically triggered once per day by a difference between the version number of the most recent GitHub release and the version installed
  - _Visible to Admin users only_
- ### Automated Book Ingests ♻️
  - Press the `Ingest Books` button on the navbar of the Web UI and anything in the ingest folder will be automatically ingested recursively!
- ### Batch Editing & Deletion! 🗂️🗄️
  - To use, simply navigate to the `Books List` page on the left hand side of the Web UI, and select the books you wish to edit/delete 
  
  ![Calibre-Web Automated](README_images/cwa-bulk-editting-diagram.png "Calibre-Web Automated Bulk Editing & Bulk Deletion")

# UNDER ACTIVE DEVELOPMENT ⚠️

- Please be aware that while CWA currently works for most people, it is still under active development and that bugs and unexpected behaviours can occur while we work and the code base matures
- For any others that wish to contribute to this project in some way, please reach out on our Discord Server and see how you can best get involved:\
    \
    [![](https://dcbadge.limes.pink/api/server/https://discord.gg/Ss93aURywf)](https://discord.gg/Ss93aURywf)


### Roadmap 🛣️🌱

- Release CWA also as a Docker Mod
- Support for `arm64` architectures
- Adding tracking of ebook imports & deletions to the `cwa.db`
- Improved metadata handling and conversion for comics & manga

# How To Install 📖

## Quick Install 🚀

1. Download the Docker Compose template file using the command below:

```
curl -OL https://raw.githubusercontent.com/crocodilestick/calibre-web-automated/main/docker-compose.yml
```

2. Move the compose file to an empty folder (e.g. ~/docker/calibre-web-automated/docker-compose.yml). This will be used to store the server data and library

3. Navigate to where you downloaded the Compose file using `cd` and run:

```
docker compose up -d
```

And that's you off to the races! 🥳 HOWEVER to avoid potential problems and ensure maximum functionality, we recommend carrying out these [Post-Install Tasks Here](#post-install-tasks).

---
## Using Docker Compose 🐋⭐(Recommended)

### 1. Setup the container using the Docker Compose template below: 🐋📜

```yml
---
services:
  calibre-web-automated:
    image: crocodilestick/calibre-web-automated:latest
    container_name: calibre-web-automated
    environment:
      - PUID=1000
      - PGID=100
      - TZ=UTC
      - DOCKER_MODS=lscr.io/linuxserver/mods:universal-calibre-v7.16.0
    volumes:
      - ./config:/config
      - ./cwa-book-ingest:/cwa-book-ingest
      - ./calibre-library:/calibre-library
      #- ./books:/books #Optional
      #- gmail.json:/app/calibre-web/gmail.json #Optional
    ports:
      - 8084:8083 # Change the first number to change the port you want to access the Web UI, not the second
    restart: unless-stopped
    
```

- **Explanation of the Container Bindings:**
  - Make sure all 3 of the main bindings are separate directories, errors can occur when binds are made within other binds
  - `/config` - Can be any empty folder, used to store logs and other miscellaneous files that keep CWA running
  - `/cwa-book-ingest` - **ATTENTION** ⚠️ - All files within this folder will be **DELETED** after being processed. This folder should only be used to dump new books into for import and automatic conversion
  - `/calibre-library` - This should be bound to your Calibre library folder where the `metadata.db` file resides within and will be mounted automatically. If there are multiple libraries, it will find and mount the largest one.
    - If you don't have an **existing** Calibre Database, you can use the Docker Template folder for a quick install mentioned above.
  - `/books` _(Optional)_ - This is purely optional, I personally bind /books to where I store my downloaded books so that they accessible from within the container but CWA doesn't require this
  - `/app/calibre-web/gmail.json` _(Optional)_ - This is used to setup Calibre-Web and/or CWA with your gmail account for sending books via email. Follow the guide [here](https://github.com/janeczku/calibre-web/wiki/Setup-Mailserver#gmail) if this is something you're interested in but be warned it can be a very fiddly process, I would personally recommend a simple SMTP Server

And just like that, Calibre-Web Automated should be up and running! HOWEVER to avoid potential problems and ensure maximum functionality,
we recommend carrying out these [Post-Install Tasks Here](#post-install-tasks).

<!-- - By default, `/cwa-book-ingest` is the ingest folder bound to the ingest folder you entered in the Docker Compose template however should you want to change any of the default directories, use the `cwa-change-dirs` command from within the container to edit the default paths -->
---
## For Developers - Building Custom Docker Image
If you want to contribute to this project, you can build the docker file by running `build.sh` in the repository.
With your modifications, this will build the docker image of the cloned repository. The `build/` folder
will be created, primarily housing the development docker-compose.yml file and its mount points. Add a calibre library here for testing if necessary. 

```bash
$ chmod +x build.sh
$ ./build.sh
```

After the script is done executing, `cd` into the `build/` folder, then run `docker compose up -d`. This will use `build/docker-compose.yml`. Modify it if necessary.

Check out [Post-Install Tasks Here](#post-install-tasks) when necessary.

---

# Post-Install Tasks:

## _Calibre-Web Quick Start Guide_

1. Open your browser and navigate to http://localhost:8084 or http://localhost:8084/opds for the OPDS catalog
2. Log in with the default admin credentials (_below_)
<!-- 3. If you don't have an existing Calibre database, you can use the `metadata.db` file above
   - This is a blank Calibre-Database you can use to perform the Initial Setup with
   - Place the `metadata.db` file in the the folder you bound to `/calibre-main` in your Docker Compose -->
<!-- 4. During the Web UI's Initial Setup screen, Set Location of Calibre database to the path of the folder to `/calibre-main` and click "Save"
5. Optionally, use Google Drive to host your Calibre library by following the Google Drive integration guide -->
3. Configure your Calibre-Web instance via the admin page, referring to the Basic Configuration and UI Configuration guides
4. Add books by having them placed in the folder you bound to `cwa-book-ingest` in your Docker Compose

**⚠️ ATTENTION ⚠️**

- _Downloading files directly into `/cwa-book-ingest` is not supported. It can cause duplicate imports and potentially a corrupt database. It is recommended to first download the books completely, then transfer them to `/cwa-book-ingest` to avoid any issues_

## Default Admin Login:

> **Username:** admin\
> **Password:** admin123

## Configuring CWA ⚙️

- If your Calibre Library contains any ebooks not in the `.epub` format, from within the container run the `convert-library` command.
  - Calibre-Web Automated's extra features only work with epubs and so **failure to complete this step could result in unforeseen errors and general unreliability**
  - Full usage can be found below in the Usage Section however the following command will automatically convert any non-epubs to epubs and store the original files in `/config/processed_books`:

```
convert-library --keep
```

- Drop a book into your ingest folder and check everything is working correctly!

# Usage 🔧

## Adding Books to Your Library

- Simply move your newly downloaded or existing eBook files to the ingest folder which is `/cwa-book-ingest` by default or whatever you designated during setup if using the Script Install method. Then on the website, press "Ingest Books" at the top right of the navigation bar. Anything you place in that folder will be automatically analyzed, converted if necessary, then imported into your Calibre-Web library.
  - **⚠️ ATTENTION ⚠️**
    - _Downloading files directly into `/cwa-book-ingest` is not supported. It can cause duplicate imports and potentially a corrupt database. It is recommended to first download the books completely, then transfer them to `/cwa-book-ingest` to avoid any issues_
    - Be sure that the books you are transferring to `/cwa-book-ingest` are owned by your user rather than root. Otherwise, permission errors may occur and may result in incomplete importing. 
    - Original and converted book files are backed up into `/config/processed_books`

## The Cover-Enforcer CLI Tool

```
usage: cover-enforcer [-h] [--log LOG] [--dir DIR] [-all] [-list] [-history] [-paths] [-v]

Upon receiving a log, valid directory or an "-all" flag, this script will enforce the covers and metadata of the corresponding books, making sure that each are correctly stored in
both the epubs themselves and the user's Calibre Library. Additionally, if an epub happens to be in EPUB 2 format, it will also be automatically upgraded to EPUB 3.

options:
  -h, --help     show this help message and exit
  --log LOG      Will enforce the covers and metadata of the books in the given log file.
  --dir DIR      Will enforce the covers and metadata of the books in the given directory.
  -all           Will enforce covers & metadata for ALL books currently in your calibre-library-dir
  -list, -l      List all books in your calibre-library-dir
  -history       Display a history of all enforcements ever carried out on your machine (not yet implemented)
  -paths, -p     Use with '-history' flag to display stored paths of all epubs in enforcement database
  -v, --verbose  Use with history to display entire enforcement history instead of only the most recent 10 entries
```

![CWA Cover-Enforcer History Usage](README_images/cwa-db-diagram.png)

## The Convert-Library Tool

```
usage: convert-library [-h] [--replace] [--keep] [-setup]

Made for the purpose of converting ebooks in a calibre library not in epub format, to epub format

options:
  -h, --help     show this help message and exit
  --replace, -r  Replaces the old library with the new one
  --keep, -k     Creates a new epub library with the old one but stores the old files in /config/processed_books
  -setup         Indicates to the function whether or not it's being ran from the setup script or manually (DO NOT USE MANUALLY)
```

<!-- ## Changing the Default Directories

- If you ever need to change the locations of your **ingest**, **import** and/ or **calibre-library** folders, use the `cwa-change-dirs` command from anywhere within the container's terminal to open the json file where the paths are saved and change them as required. -->

## Checking the Monitoring Services are working correctly

- Simply run the following command from within the container: `cwa-check`
- If all 3 services come back as green and running they are working properly, otherwise there may be problems with your configuration/install

# Further Development 🏗️
We are always open to suggestions and ideas from the community so if there's something you'd like to see, don't be afraid to ask!
- To do so you can either open an Issue here on the project page or get in touch with us on our Discord Server [here](https://discord.gg/Ss93aURywf)!
- We're always looking for new people to help out too so if that sounds like you, let us know!
