# REDCap Development Class

These instructions were written for a REDCap Development Class presented at the University of Arkansas Medical School in February 2020. They describe how to set up a development environment that works on both Mac OSX and Windows computers. While this version presents some issues for Windows computers, future versions should address that shortcoming through the use of `git-bash`.

These notes are the first step towards a revision to the external module development guide that will provide separate instructional modules for software setup, software concepts, and development exercises all centered around developing modules for REDCap.

## Required Resources
- Laptop
- Admin rights to that laptop
- Slack client - or just use the web client. That's at [https://slack.com/](https://slack.com/).
- GitHub account - [https://github.com/](https://github.com/)
- GitHub Desktop - [https://desktop.github.com/](https://desktop.github.com/)
- Docker Desktop - [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)
- A little experience with docker and docker-compose
- redcap-docker-compose - A docker environment tailored for running and developing for REDCap. See [https://github.com/123andy/redcap-docker-compose](https://github.com/123andy/redcap-docker-compose)
- redcap9.3.5.zip - Ask your local REDCap Admin for `redcap9.3.5.zip` downloaded from the REDCap Community. If you _are_ that REDCap Admin and have access to the community, the download link is [https://community.projectredcap.org/page/download.html](https://community.projectredcap.org/page/download.html). Please be generous with access to the resources your employees and customers need to learn and develop for REDCap while reminding them of the need to not redistribute REDCap software outside the entity that has licensed it.
- Text editor or PHP IDE - VS Code, PHPStorm, or Atom are good.
- Firefox or Chrome - we will be using the developer tools.


## Pre-req Setup Tasks

- Accept Slack invite in your email inbox.
- Set a Slack password for our workspace.
- Keep [https://uf-and-uams.slack.com](https://uf-and-uams.slack.com) open.
- (Philip will send this list of instructions via our Slack workspace so you can click on the links. No transcription required.)
- Install [https://code.visualstudio.com/Download](https://code.visualstudio.com/Download) (Because friends don't let friends use Notepad.)
- Create a GitHub account - [https://github.com/](https://github.com/)
- Install GitHub Desktop - [https://desktop.github.com/](https://desktop.github.com/)
- Feed GitHub credentials into GitHub Desktop.
- Install Docker Desktop from [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)


## REDCap-Specific Setup

- Use GitHub Desktop to clone `123andy/redcap-docker-compose` (i.e., [https://github.com/123andy/redcap-docker-compose](https://github.com/123andy/redcap-docker-compose))
- Open `redcap-docker-compose` in VS Code
- Open the file `.env`
- Paste this block at the end of the file and save it.

```
TZ=America/Chicago
DOCKER_PREFIX=redcap
WEB_PORT=1935
MYSQL_PORT=2935
PHPMYADMIN_PORT=3935
MAILHOG_PORT=4935
```

- In GitHub Desktop open a terminal cd'd to the `redcap-docker-compose` folder with the keystroke \<ctrl\>```
- Change directories into the `rdc` folder with the command `cd rdc`
- In the terminal start the containers with this command `docker-compose up -d`
- Wait for the containers to build. This step takes a few minutes the first time.
- Wait one more minute.
- Access web-based redcap installer at [http://localhost:1935](http://localhost:1935) You should see this:

![REDCap Docker-Compose Installer](/assets/img/installer.png)

- Select 'Use a local copy of the full zip installer...'. Click Browse, locate redcap9.3.5.zip, and upload it.
- Select 'Prepopulate with table-based users and activate table authentication' and provide _your_ email address.
- Click 'INSTALL REDCAP'
- Wait a couple of minutes.
- When you see the many green dialog boxes, the installation is complete.

![Successful installation](/assets/img/successful_installation.png)

- Ignore the instructions to access the REDCap installer and access your fully functional local REDCap at [http://localhost:1935](http://localhost:1935)
- Login as admin. Your password is 'password'. Every user's account is 'password'.


## It didn't work!

Sometimes things are weird, and you'll have to rebuild from scratch. To do that, you'll need to open a terminal to the `redcap-docker-compose` folder and issue these commands:

```
# Preserve your modules folder because it might have stuff worth preserving
cp -r www/modules .

# The installer will get confused if any part of your old installation exists
rm -rf www
rm -rf logs

# You have to be in the right folder for docker-compose to find its config files
cd rdc

# Destroy the containers and their volumes
docker-compose down -v

# Rebuild the containers and volumes from scratch
docker-compose up -d
```

If it fails again, you'll need to dig deeper to find out why. It's probably just a small thing, but knowing which small thing is hard to anticipate. Sorry about that.

## Playtime

Now that you have a REDCap system of your own, play with it. Know that you can only hurt yourself at this point. In the worst-case scenario, you can rebuild from scratch. Yet know that no one is protecting your work on this personal REDCap except you. Don't do any work you aren't willing to lose.

Here are some things you might try:

- Access the Control Center > Browse Users. Look at the accounts that the installer made for you.
- Access the Control Center > General Configuration.
- Access the Control Center > User Settings.
- Access the Control Center > File Upload Settings.
- Access the Control Center \> External Modules \> View modules available.... Download a few modules. Try _Admin Dashboard_, _MySQL Simple Admin_, _Image Map_, and _Date Calculated Fields_. They are popular and easy to implement.
- Access the Control Center > Modules/Services Configuration. These "Modules" are not _External Modules_. They are big features in REDCap that are turned off by default.
- Access the Control Center > Field Validation Types. Do you know these are just rows in a MySQL table? Did you know you can add your own?
- Access the Control Center > Project Templates. You can add your own.


## Getting the development exercises

- Use GitHub Desktop to clone `ctsit/redcap_external_module_development_guide` (Go to File > Clone Repository, then paste `ctsit/redcap_external_module_development_guide` in the "github.com" tab).
- Use GitHub Desktop to open this new repo (REMDG) in Finder/Explorer. Copy everything in the `exercises/` folder to folder `redcap-docker-compose/www/modules/`. Don't copy the `exercises` folder; copy its _contents_.  These files should be inside the `redcap-docker-compose/www/modules/` folder

```
accessing_variables
hello_world_v0.0.0
intro_to_hooks
intro_to_js
intro_to_plugins
intro_to_queries
README.md
record_wrangling
```

## Add an extension to VS Code

VSCode can show you syntax errors as you type them if you add an extension. Add the _PHP Intellisense_ extension to VS Code to get this functionality.  On Mac access _Code > Preferences > Extensions_ and type "PHP". You'll see _PHP Intellisense_ in the list. Install it.

Open some PHP code, delete a semicolon or a comma and watch for the wiggly red lines under the text. Hover on one next to the text you just changed and consider what the editor is telling you.
