---
title: Scheme Environment Setup
layout: page
---

All of the work in [SICP](http://mitpress.mit.edu/sicp/) is done in in [mit-scheme](http://en.wikipedia.org/wiki/MIT/GNU_Scheme).
The following instructions will help you set up an environment to follow along with the coding examples and exercises.
If you have or need instructions on how to get set up on an environment not listed below, feel free to let us know or send us a pull request.

## Ubuntu Linux

### Installing Emacs

I recommend using the latest and greatest version of emacs, which can be had from [Damien Cassou's PPA](https://launchpad.net/~cassou/+archive/emacs). To install, just add the ppa, update, and install:

    sudo add-apt-repository ppa:cassou/emacs
    sudo apt-get update
    sudo apt-get install emacs24 emacs24-el emacs24-common-non-dfsg

### Installing MIT Scheme

#### 32-bit Ubuntu

If you are on a 32-bit system, all you need to do is install the `mit-scheme` package from the ubuntu repository:

    sudo apt-get install mit-scheme

#### 64-bit Ubuntu

If you are on a 64-bit system, you'll need to find a 64-bit package. Lucky for us, the folks at the University of Minnesota [have one available we can use](https://wiki.umn.edu/CSCI1901/InstallingMITScheme):

    sudo apt-get install xutils-dev libx11-dev libncurses5-dev
    sudo wget http://www-users.cselabs.umn.edu/classes/Fall-2010/csci1901/mit-scheme-x64_9.0.1-1_amd64.deb
    sudo dpkg -i mit-scheme-x64_9.0.1-1_amd64.deb

The 64-bit package installs mit scheme as `mit-scheme-x86-64`, so to launch it using the more familiar `mit-scheme` and `scheme` commands, we'll symlink it:

    sudo ln -s -T /usr/local/bin/mit-scheme-x86-64 /usr/local/bin/mit-scheme
    sudo ln -s -T /usr/local/bin/mit-scheme-x86-64 /usr/local/bin/scheme

Once installed, you can start an interactive scheme session in emacs by running `M-x run-scheme`.

## Mac OSX

### Installing Emacs

I use [Homebrew](http://brew.sh) for my emacs install since most of the other ones for Mac OS X have annoyed me in one way or another so here's how I set that up.

1. Install Homebrew

        ruby -e "$(curl -fsSkL raw.github.com/mxcl/homebrew/go)"
        brew install automake
    
2. Install emacs

        brew install emacs --HEAD --use-git-head --cocoa --srgb

3. Then get the mac to recognize your installation 

        ln -s /usr/local/Cellar/emacs/HEAD/Emacs.app /Applications
    
4. Tell Launchbar to find it in /usr/local/Cellar
5. Then fix the path adding this to .bash_profile:

        alias emacs=/usr/local/Cellar/emacs/HEAD/bin/emacs
        alias emacsclient=/usr/local/Cellar/emacs/HEAD/bin/emacsclient
        
        export EDITOR=emacs
        export EMACS=/usr/local/Cellar/emacs/HEAD/bin/emacs

6. Then `source .bash_profile`

### Installing MIT Scheme

1. Download and install [MIT/GNU Scheme](http://www.gnu.org/software/mit-scheme/) for Mac OS X x86-64.
2. Download and install [XQuartz](http://xquartz.macosforge.org/landing/) if you don't already have it.
3. Install a symlink to the binary

        sudo ln -s /Applications/MIT\:GNU\ Scheme.app/Contents/Resources/mit-scheme /usr/local/bin/scheme
        
4. Add the following to your `.emacs` (N.B. the backslash escaping)

        (setenv "MITSCHEME_LIBRARY_PATH" "/Applications/MIT\\:GNU\\ Scheme.app/Contents/Resources")
        
5. Add the following to your `.bash_profile`

        export MITSCHEME_LIBRARY_PATH="/Applications/MIT\:GNU\ Scheme.app/Contents/Resources"

6. `source .bash_profile`
7. To run scheme in emacs do `M-x run-scheme` You need to make sure your emacs knows to use `/usr/local/bin` for a path and has setup the `MITSCHEME_LIBRARY_PATH` you should see something like this:

        MIT/GNU Scheme running under MacOSX
        Type `^C' (control-C) followed by `H' to obtain information about interrupts.

        Copyright (C) 2011 Massachusetts Institute of Technology
        This is free software; see the source for copying conditions. There is NO warranty; not
        even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

        Image saved on Tuesday November 8, 2011 at 10:45:46 PM
          Release 9.1.1 || Microcode 15.3 || Runtime 15.7 || SF 4.41 || LIAR/x86-64 4.118
          Edwin 3.116

        1 ]=> 

8. To run it from the command line and enter the REPL just run `scheme` again assuming your bash knows to use /usr/local/bin as part of it's path.  `^D` to exit the REPL.  You should see the same thing as above but not in an emacs buffer.

### References
* [How to run scheme with Emacs?](http://stackoverflow.com/questions/4259894/how-to-run-scheme-with-emacs)
* [some question and info on emacs Path](http://stackoverflow.com/questions/2266905/emacs-is-ignoring-my-path-when-it-runs-a-compile-command/2566945#2566945)

