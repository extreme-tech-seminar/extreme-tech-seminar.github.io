---
title: Scheme Environment Setup
author: Kevin McAllister
layout: default
---

Since [SICP](http://mitpress.mit.edu/sicp/) work is done in [scheme](http://en.wikipedia.org/wiki/Scheme_(programming_language)) it makes sense to put some notes together on how I got that setup.  I'll write that up for emacs on Mac OS X since that is the compute environment I use.  

I'll cheerfully accept pull requests for other environments.

## emacs scheme ##

While I'm sure there is someone who will give us the details on doing this in emacs on an Ubuntu install, I'm going to focus on doing it on Mac OS X.

### Ubuntu ###

PLACEHOLDER for someone to tell us how they set it up on their Ubunts.

### Mac OS X ###

#### emacs setup ####

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

#### MIT scheme setup ####

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

##### References #####
* [How to run scheme with Emacs?](http://stackoverflow.com/questions/4259894/how-to-run-scheme-with-emacs)
* [some question and info on emacs Path](http://stackoverflow.com/questions/2266905/emacs-is-ignoring-my-path-when-it-runs-a-compile-command/2566945#2566945)

