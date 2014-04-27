# Scheme Environment Setup #

Since [SICP](http://mitpress.mit.edu/sicp/) work is done in [scheme](http://en.wikipedia.org/wiki/Scheme_(programming_language)) it makes sense to put some notes together on how I got that setup.  I'll write that up for emacs on Mac OS X since that is the compute environment I use.  

I'll cheerfully accept pull requests for other environments.

## emacs scheme ##

While I'm sure there is someone who will give us the details on doing this in emacs on an Ubuntu install, I'm going to focus on doing it on Mac OS X.

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

#### Geiser and Racket setup ####

I've seen fancy [Clojure](http://clojure.org/) guys connect their emacs process to a REPL on a running JVM process.  Using a bunch of technologies that I don't understand like [Leiningen](https://github.com/technomancy/leiningen) and [Swank](https://github.com/technomancy/swank-clojure/) and [SLIME](http://common-lisp.net/project/slime/).  

Anyway I thought part of the LISP world was connecting to a running LISP REPL, so I did a little hunting and saw that [Geiser](http://www.nongnu.org/geiser/) seems to be a way to do that with a Scheme and it works with a modern language implementing a Scheme plus a whole lot more called [Racket](http://www.racket-lang.org) and another one called [Guile](http://www.gnu.org/software/guile/).  So I decided to try and set that up to and document the effort.  This way when working through SICP I can use MIT/GNU Scheme for cannonical study, and Racket for *fancy stuff™*

1. [Download](http://download.racket-lang.org) and install Version 6.0 of Racket for Mac OS X 64 bit.  Put the Racket directory under /Applications and rename it to remove the space.
2. Install Guile 2.0.9 because Geiser docs say it supports multiple REPLs

        brew update
        brew install guile --devel
        

3. Install Geiser using it's [ELPA](http://www.emacswiki.org/emacs/ELPA) package.

        M-x package-install RET geiser RET
        
4. Update .emacs to tell geiser where Racket is, this line should go after the package system loading happens which will load geiser.

        (setq geiser-racket-binary "/Applications/Racket/bin/racket")
        
5. Now I can run racket by doing `M-x run-racket` or `M-x run-geiser RET racket RET`
6. Or I can do Guile by doing `M-x run-guile` or `M-x run-geiser RET guile RET`

##### Connect to an already running Racket #####

1. Start Racket using this command so the search path includes the Racket server stuff in geiser.

        racket -i -S "/Users/mcallist/.emacs.d/elpa/geiser-0.4/racket"
        
2. run this in your racket REPL:

        (require geiser/server)
        (start-geiser 8000 "127.0.0.1")

3. In emacs run `M-x connect-to-racket RET localhost RET 8000 RET`

##### Connect to an already running guile #####

1. Start guile using this commmand

        guile --listen
        
2. In emacs `M-x connect-to-guile` and the default guile port is 37146

Currently this bombs out using guile 2.0.11 on my Mac using geiser-0.4.  So I suspect some sort of conflict or mismatch, but I'm not troubleshooting it at the moment.
