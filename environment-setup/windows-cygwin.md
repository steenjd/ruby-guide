# How to Set Up a Ruby Development Environment in Windows under Cygwin

These instructions were written and tested with:
- Windows 10, 64-bit, version 1803
- Cygwin, 64-bit, DLL version 2.10.0
- Ruby 2.3.6p384 (Cygwin package)
- rbenv, 2018-06-07 master branch (release 1.1.1 plus changes)
- ruby-bulid, 2018-07-03 master branch (release v20180618 plus changes)
- Ruby 2.5.1p57, built with ruby-build


## Cygwin

Go to https://cygwin.com/ and download the setup .exe file appropriate for your architecture (64- or 32-bit).

Run the Cygwin setup .exe you downloaded and select an installation folder.
- **Simple, recommended option.** Enter `C:\PROGRA~1\cygwin64` as the installation path, assuming your "Program Files" folder is on the C drive and you are using 64-bit Cygwin.
- **Advanced option.** You may install Cygwin wherever you like, but all of the folders in the installation path should _not_ contain spaces in their names. If you want to install to `C:\Program Files\cygwin64`, for example, then you should use the 8.3 ("8 dot 3") filename form of the path instead: `C:\PROGRA~1\cygwin64`. To find a folder's 8.3 name, run `cmd` (Command Prompt) and use `dir /x` on its parent folder, like `dir /x c:\`.

Select a download mirror. Any mirror should work, but you might want to make your choice based on geographic proximity, trust, or known speed.

At the package selection step, choose View: Full to reveal all available packages. To select a package for installation, click on "Skip" under the New column so that the box in the Bin column becomes selected and the "Skip" becomes a package version number. Select the following packages to install. You can use Search to quickly locate a package.
- autoconf
- automake
- gcc-core
- git
- libffi-devel
- libgdbm-devel
- libncurses-devel
- libreadline-devel
- libyaml-devel
- make
- openssl-devel
- ruby
- zlib-devel

Click Next and then Next again to confirm changes to installed packages.

After installation is complete, you can launch Cygwin64 Terminal from the Start Menu or Desktop, which will give you a `bash` shell under Cygwin.


## rbenv

rbenv is a utility that manages multiple installed Ruby versions and lets you seamlessly switch between them as needed. The version of Ruby installed by Cygwin is your "system" Ruby and will be updated by Cygwin. It is installed to a location similar to other packages managed by Cygwin. With rbenv and ruby-build (see below), you can install other Ruby versions into your home directory.

Install rbenv by following the "Basic GitHub Checkout" steps here:  
https://github.com/rbenv/rbenv#basic-github-checkout

Exit your Cygwin Terminal shell and launch a new one so that the shell init commands you just added can take effect.


## ruby-build

The [ruby-build](https://github.com/rbenv/ruby-build#readme) utility is a plugin for rbenv that lets you install a wide variety of Ruby versions beyond the limited number of Ruby versions available through your system's software package manager. This allows you to run Ruby projects on a specific required version of Ruby, test your projects against multiple Ruby versions, and try out the latest version of Ruby or another Ruby implementation (e.g., JRuby, mruby) altogether.

Follow the [ruby-build installation instructions](https://github.com/rbenv/ruby-build#installation), using the "rbenv plugin" install option.


## Install the latest Ruby version

The Ruby versions available in system software package managers are often one or two minor versions behind the latest Ruby release version. While this provides stability for other system packages that depend on Ruby, it doesn't let you try out the latest Ruby features available in [new releases](https://www.ruby-lang.org/en/news/).

To see the currently installed version of Ruby, try running the following commands at the terminal:
```
rbenv versions
ruby --version
```

To see which versions of Ruby can be installed by ruby-build, run:
```
rbenv install --list
```

Scroll back through the output and look for version numbers that aren't prefixed by a name, e.g., `2.5.1`. These are versions of the "reference" Ruby implementation, known as MRI or CRuby, which is the Ruby interpreter we will be using. (You can read more about [other Ruby implementations](https://www.ruby-lang.org/en/about/) at ruby-lang.org.)

To install one of the Ruby versions listed:
```
rbenv install 2.5.1
# or the Ruby version of your choice
```

Installation will take a while because it compiles the Ruby interpreter from source code. If the process exits with an error, you might need to run the Cygwin installer again and install the missing package(s) referenced in the error message.

After a successful installation, the new Ruby version will appear in the output of `rbenv versions`.

To use the newly installed Ruby version as the default Ruby when you run the `ruby` command at the terminal:
```
rbenv global 2.5.1
# or whichever Ruby version you just installed
```

To force a project to use a specific version of Ruby, create a plain-text file named `.ruby-version` in the project's root folder, and use the Ruby version as the sole content of the file, like this:
```
2.5.1
```


## Integrate your Cygwin and Windows folders

When you launch Cygwin Terminal after installation, you will get a Bash shell that behaves much like a command line shell under Linux or macOS.

Your home directory, from Cygwin's point of view, is `/home/username`, where `username` is your Windows username. Actually, that `/home` directory is a subdirectory of the folder you installed Cygwin to.
* To access your Windows files from Cygwin, use the mounted drive paths under `/cygwin`. For example, if your Windows username is `username`, then you would access your Windows profile ("home") folder from Cygwin as `/cygdrive/c/Users/username`.

You may find it useful to link some of your frequently used Windows folders, such as `Downloads` or `Documents`, to your Cygwin home directory so both environments can share the same folders.  
In Cygwin Terminal, enter:
1. `cd` _(to switch to your home directory)_
1. `ln -s /cygdrive/c/Users/$USER winhome`
1. `ln -s winhome/Downloads .`
1. `ln -s winhome/Documents .`
1. ...

After entering the above commands, the command `cat ~/Documents/notes.txt` will display the contents of `notes.txt` from the `Documents` folder of your Windows user profile.


## Code editor

Download and install the Atom editor:  
https://atom.io/  
Atom has syntax highlighting and automatic indentation to help the programmer visually parse the code and write fewer typos. Atom is also available for Linux and macOS, so you do not need to change editors if you change platforms.

If you will be running your Ruby code exclusively under Cygwin or copying your code files directly to a Linux or macOS environment, then you might want to change the default line-ending character to LF from the default Windows-style CRLF so your lines of code are terminated with `\n` instead of `\r\n`:
1. File menu: Settings
1. Packages
1. search: `line-ending-selector`, Settings (for that package)
1. Default line ending: LF
