# How to Install Ruby Development Tools in Ubuntu Linux

Software versions used when writing this guide,
referenced here in case future versions are incompatible with the instructions:
- Ubuntu Desktop 19.04
- Atom 1.39.1
- rbenv commit 4e92322 (1.1.2 + 2 commits)
- ruby-build commit 0e9094b (v20190615 + 7 commits)
- Ruby 2.6.3

## Atom

Atom is a text editor app with many useful features for writing code.

Open the Ubuntu Software app and search for Atom by tapping the
magnifying glass icon in the top toolbar.
The app icon for Atom is circular and green with a white atomic symbol.

Install Atom.

## Prerequisite Ubuntu packages

The following commands will be run in Terminal (i.e., in a `bash` shell).

First, install Git, which will let you interact with versioned code repositories:  
```
sudo apt install git
```

Then, install a set of tools that will let you build (compile) Ruby
via ruby-build later:  
```
sudo apt install autoconf bison build-essential libssl-dev libyaml-dev
sudo apt install libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm-dev
```
The above package list is derived from the
[ruby-build wiki's "Suggested build environment"](https://github.com/rbenv/ruby-build/wiki#suggested-build-environment).

## rbenv

rbenv is a utility that lets you easily switch between Ruby versions
as needed by different projects or tasks. Together with ruby-build (see below),
you can install multiple, specific Ruby versions into your home directory.
This removes dependence on the Ruby version bundled with the operating system,
which would change unexpectedly when Ubuntu upgrades it.

Install rbenv by following the manual steps under "Basic GitHub Checkout" here:  
https://github.com/rbenv/rbenv#basic-github-checkout

If the instructions tell you to edit your `~/.bashrc` shell initialization file,
you can do this with Atom by entering this command:  
```
atom ~/.bashrc
```

Every time you edit your `~/.bashrc` file, you must launch a new Terminal shell
for the new settings to take effect. Do this by tapping the new-tab button
in the Terminal toolbar or by pressing the key combination `Ctrl-Shift-t`.


## ruby-build

ruby-build is a plugin for rbenv that lets you install specific versions of Ruby.

Follow the [ruby-build installation instructions](https://github.com/rbenv/ruby-build#installation)
under the heading "As an rbenv plugin".


## Install the latest Ruby version

The Ruby versions packaged with Ubuntu are usually older
(on some systems, much older) than the latest Ruby release version.
While this provides stability for system packages that depend on Ruby,
it doesn't let you try out the latest Ruby features available
in [new releases](https://www.ruby-lang.org/en/news/).

To see the currently installed version of Ruby,
try running the following commands in Terminal.
It's okay if Ruby is not installed; you will install it shortly.  
```
rbenv versions
ruby --version
```

_If you are revisiting this guide from the future, you'll need to
[upgrade ruby-build](https://github.com/rbenv/ruby-build#upgrading)
so the following command can list the latest Ruby versions._

To see which versions of Ruby can be installed by ruby-build, run:  
```
rbenv install --list
```

Scroll back through the output and look for versions that are
purely numbers and dots, e.g., `2.6.3`.
These are versions of the "standard" or "reference" Ruby implementation,
known as MRI or CRuby, which is the Ruby interpreter we will be using.
(You can read more about
[other Ruby implementations](https://www.ruby-lang.org/en/about/)
at ruby-lang.org.)
Versions with a suffix such as "pre", "dev", or "rc"
are not considered production-ready but are available
if you would like to try out features available in future Ruby releases.

To install one of the Ruby versions listed:  
```
rbenv install 2.6.3
# or the Ruby version of your choice
```

Installation will take a while because it downloads and compiles
the Ruby interpreter from source code. If the process exits with an error,
you might need to install a missing prerequisite system package
(using `sudo apt install PACKAGENAME`) referenced in the error message.

After a successful installation, the new Ruby version will appear
in the output of the shell command  `rbenv versions`.

To use the newly installed Ruby version as the default Ruby
when you run the `ruby` command at the terminal:  
```
rbenv global 2.6.3
# or whichever Ruby version you just installed
```

To force a project to use a specific version of Ruby,
create a plain-text file named `.ruby-version` in the project's root folder,
and use the Ruby version as the sole content of the file, like this:  
```
2.6.3
```
