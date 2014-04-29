# emacs and other schemes #

I've done some additional playing with some Lisps and wanted to document the effort I went to set those up with emacs on my Mac OS X machine.  Again I am using emacs installed with [Homebrew](http://brew.sh) and see below for using Geiser with Racket and Guile which are other programming language environments.

## Geiser and Racket and Guile setup ##

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

### Connect to an already running Racket ###

1. Start Racket using this command so the search path includes the Racket server stuff in geiser.

        racket -i -S "/Users/mcallist/.emacs.d/elpa/geiser-0.4/racket"
        
2. run this in your racket REPL:

        (require geiser/server)
        (start-geiser 8000 "127.0.0.1")

3. In emacs run `M-x connect-to-racket RET localhost RET 8000 RET`

### Connect to an already running guile ###

1. Start guile using this commmand

        guile --listen
        
2. In emacs `M-x connect-to-guile` and the default guile port is 37146

Currently this bombs out using guile 2.0.11 on my Mac using geiser-0.4.  So I suspect some sort of conflict or mismatch, but I'm not troubleshooting it at the moment.
