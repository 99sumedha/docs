---
title: Command-Line Interface Setup
description: Learn how to set up the NativeScript command-line interface on Windows, macOS, and Linux development machines.
position: 20
slug: quick-start
previous_url: /setup/quick-setup
---

# Set Up Your System

With the open-source NativeScript command-line interface and an IDE or text editor of your choice, you can create, develop, store, and build your apps locally, free of charge and anonymously.

The NativeScript CLI allows you to build apps in two different ways.

* The [**Quick Setup**](#quick-setup) allows you to build and develop NativeScript apps without installing any iOS or Android dependencies on your local machine, and is perfect for getting started.
* The [**Full Setup**](#full-setup) walks you through installing the dependencies you need to compile iOS and Android apps locally. You’ll eventually need to complete the full setup to use certain NativeScript plugins, and to build apps for the App Store and Google Play.

## Quick Setup

### Step 1: Install Node.js

The NativeScript CLI is built on [Node.js](https://nodejs.org/en/), and as such you need to have Node.js installed to use NativeScript.

To check whether you have Node.js installed, open a terminal or command prompt and execute `node --version`. If there is an error, head to  <https://nodejs.org/> and download and install the latest “LTS” (long-term support) distribution and restart your terminal or command prompt.

> **TIP**:
> * If you’re on macOS and use [Homebrew](http://brew.sh/), you can alternatively install the Node.js LTS release by running `brew update` (to download the latest updates) and then `brew install node@10` in your terminal.
> * The NativeScript CLI supports a wide variety of Node.js versions, so if you already have Node.js installed you should be good to go. If, by chance, you’re running an unsupported version, the `tns doctor` command will run momentarily and flag the problem so you can upgrade.

> **NOTE**: Mac users, bear in mind that you might need to add the path to `node@10/bin` manually. If you execute `node --version` and get a “command not found” error, run `echo 'export PATH="/usr/local/opt/node@10/bin:$PATH"' >> ~/.bash_profile` and restart your terminal.

## Step 2: Install the NativeScript CLI

Open your terminal or command prompt and execute the following command to install the NativeScript CLI from 
[npm](https://www.npmjs.com/), which is the Node.js package manager:

<pre class="add-copy-button"><code class="language-terminal">npm install -g nativescript</code></pre>

> **NOTE**:
> * If you’re on macOS and receive an EACCES error, you either need to rerun the previous command with `sudo`—that is, `sudo npm install -g nativescript`—or take a moment to [fix your npm permissions](https://docs.npmjs.com/getting-started/fixing-npm-permissions) so that you don’t need admin rights to globally install npm packages.

After completing the setup there should be two commands available on the terminal/command prompt: `tns` (short for <b>T</b>elerik <b>N</b>ative<b>S</b>cript) and `nativescript`. The two commands are identical, so you’ll likely want to stick with the shorter `tns`.

Go ahead and verify that the installation was successful by running `tns` in your terminal or command prompt. You should see a long list of commands that starts with this section:

```
$ tns
# NativeScript CLI
┌─────────┬─────────────────────────────────────────────────────────────────────┐
│ Usage   │ Synopsis                                                            │
│ General │ $ tns <Command> [Command Parameters] [--command <Options>]          │
│ Alias   │ $ nativescript <Command> [Command Parameters] [--command <Options>] │
└─────────┴─────────────────────────────────────────────────────────────────────┘
```

## Step 3: Install the NativeScript Playground app

The NativeScript CLI quick setup lets you develop your apps locally, and test them on a physical Android or iOS device.

So to complete the setup, download and install the **NativeScript Playground** app on your mobile device. You can search for “NativeScript Playground” on the iOS App Store or Google Play, or, use the links below.

[![](/img/start/app-store.png)](https://itunes.apple.com/us/app/nativescript-playground/id1263543946?mt=8&ls=1)
[![](/img/start/google-play.png)](https://play.google.com/store/apps/details?id=org.nativescript.play)

At this point, you’re all set. As a next step, you’ll need to [**learn the basics of the NativeScript CLI**](../start/cli-basics).

Once you’ve learned the basics, feel free to return to this page and perform the full setup below, which will let you compile your NativeScript apps locally.

## Full Setup

## Step 1: Install iOS and Android requirements

NativeScript builds truly native iOS and Android apps, and as such, each target platform needs setting up on your development machine. To ease the pain of installing all of these requirements manually, the `tns` command provides quick-start scripts for Windows and macOS. Let’s look at how they work.

> **TIP**:
> * Setting up your machine for native development can be tricky, especially if you’re new to mobile development. If you get stuck, or if you have questions while going through these instructions, [Stack Overflow](https://stackoverflow.com/questions/tagged/nativescript) is a great place to get help.
> * If you’re not comfortable with a script automatically installing dependencies on your development machine, or if you’re on Linux, refer to one of the advanced setup guides below for details on manually installing the iOS and Android dependencies.
>     * [Advanced setup: Windows](/start/ns-setup-win)
>     * [Advanced setup: macOS](/start/ns-setup-os-x)
>     * [Advanced setup: Linux](/start/ns-setup-linux)

### Windows

#### Prerequisites
* Windows 7 Service Pack 1 or later

#### Setup

If running OS is Windows; copy and paste the script below into your command prompt as an administrator and press Enter:

Please be sure that you run this command in cmd as an administator (Windows key > type "cmd" > right click > Run as Administrator).

<pre class="add-copy-button"><code class="language-terminal">@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((new-object net.webclient).DownloadString('https://www.nativescript.org/setup/win'))"</code></pre>

During installation you may need to accept a User Account Control prompt to grant the script administrative privileges. Also, be aware that the script downloads and installs some big dependencies—so it’s common for the script to take a while to complete. When the script finishes, close and reopen your command prompt.

> **NOTE**: On Windows and Linux systems you can only use the `tns` command to develop Android apps. This is because the NativeScript CLI uses Xcode to build iOS apps, which is only available on macOS. If you’re interested in building iOS apps on Windows or Linux, you should download [NativeScript Sidekick](https://www.nativescript.org/nativescript-sidekick).

After the installation the system setup should have:
* The latest stable official release of Node.js (LTS) [10.x](https://nodejs.org/dist/latest-v10.x/) 
* Google Chrome 
* JDK 8
* Android SDK
* Android Support Repository
* Google Repository
* Android SDK Build-tools 28.0.3 or a later stable official release
* Android Studio
* Set up Android virtual devices to expand your testing options 

The two environment variables `JAVA_HOME` and `ANDROID_HOME` are required for Android development, which should have been automatically added as part of the installation:


> **NOTE** To check if `JAVA_HOME` and `ANDROID_HOME` are set:
- close any open Command Prompt windows,
- open a new Command Prompt
- execute `echo %JAVA_HOME%` and make sure a valid path is returned
- execute `echo %ANDROID_HOME%` and make sure a valid path is returned

> **Tip** You can [install and use custom Android Virtual Devices](https://docs.nativescript.org/tooling/android-virtual-devices) that are emulating different API levels and screens.

### macOS

If you’re using macOS, copy and paste the script below into your terminal and press Enter:

<pre class="add-copy-button"><code class="language-terminal">ruby -e "$(curl -fsSL https://www.nativescript.org/setup/mac)"</code></pre>

The macOS script needs administrative access to run some commands using `sudo`; therefore, you may need to provide your password several times during execution. The macOS script also may take some time to complete, as it’s installing the dependencies for both iOS and Android development. When the script finishes, close and restart your terminal.

## Step 2: Verify the setup

To verify the setup, run the `tns doctor` command which will check for any issues with the installation. If you see “No issues were detected” you’re good to go!
