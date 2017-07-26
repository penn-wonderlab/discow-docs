This document describes the steps needed to install a development version of the Hypothesis web annotation tool. It is primarily interested in enhancing existing documentation, paying special attention to pitfalls and troubleshooting issues I have run into while working with the code.

##System Requirements

###Operating system
Before beginning any actual setup, you will need to determine which operating system you will use while working with the Hypothesis code. The official Hypothesis documentation lists only instructions for Ubuntu and Mac OS X. While it may be possible to set up the environment on Windows, it is not officially supported, and many of the operations assume access to command-line tools not available on Windows.

The environment that I used to write this guide was Ubuntu 16.04 LTS.

##Setup and install

###File structure

The code that makes up the Hypothesis server is divided between three separate components:

[h](https://github.com/hypothesis/h), the server code, which handles all the back-end information and ensures all users have access to each others' (public) annotations

The [client](https://github.com/hypothesis/client) code, which interacts with a user and handles all of the information that they see

The [browser extension](https://github.com/hypothesis/browser-extension), which plugs into a user's browser (as of writing only Chrome is supported, with a Firefox version in development) and allows Hypothesis to interact with the information on the web


The project currently makes use of all three components, and so they should all be installed. Instructions for installing each can be found [here for the h server code](https://h.readthedocs.io/en/latest/developing/install/), [here for the client code](https://h.readthedocs.io/projects/client/en/latest/developers/developing/), and [here for the browser extension](https://github.com/hypothesis/browser-extension/blob/master/docs/building.md). The linked guides all provide a fairly complete set of instructions for obtaining all necessary dependencies and getting a local installation of Hypothesis running. The end result should be a browser extension installed on Chrome which is built using a local repository of the client code, which communicates with a server running on the local machine. While the client code and browser extension only need to accessed when changes are made to either set of code, the server needs to be run from the command line every time the computer is rebooted in order to access it from the browser extension.

The commands which need to be run to start up the local server, after completing the initial install, are as follows, from the main github/h repository:

```
sudo docker-compose up -d
source .venv/bin/activate
make dev
```

The existing documentation up until this point is rather well developed, and any issues encountered with the installation so far should be readily available.

Once installed, there are a few issues with actually running the development build to watch out for. First of all, the development extension does not appear to function properly on sites served via HTTPS, displaying the basic GUI of the program but not communicating with the server at all. It appears to be an issue with Chrome itself blocking the scripts from running- a solution is currently in the works, but until then the application runs without complaint on sites served via plain HTTP.

If you need an example HTTP page, try the [Apache software license](http://www.apache.org/licenses/LICENSE-2.0).

Another issue to keep in mind is that, as the service is running through a local server, it cannot access the account data from the main hypothesis service, and new accounts do not register properly through the sign-up form in this mode. To get around this, new users can be added via the command line as follows, from the main h directory:
```
./bin/hypothesis --dev user add
```
(Note: the python virtual environment must be set up before running this command through `source .venv/bin/activate`)
The terminal will then prompt you to provide a username, email address, and password for the account. If you wish to make a given user an admin, follow up with the command:
```
./bin/hypothesis --dev user admin <username>
```
where <username> is the name of an existing account.

These steps should be enough to get a development install of hypothesis running, with real-time testing active and available. More will be added to this document in the future as issues continue to arise and be resolved.
