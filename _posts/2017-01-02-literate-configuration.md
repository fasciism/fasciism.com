---
layout: post
---

The first file that Emacs runs when it loads is `~/.emacs.d/init.el`. This file is written using [Emacs Lisp (elisp)](https://en.wikipedia.org/wiki/Emacs_Lisp). It is important to get one thing clear in your head: Emacs is just a Lisp runtime. This means that while Emacs is primarily considered a text editor, that's just its user interface, not its application.

The init file that I'm using right now takes every file matching `~/.emacs.d/*.org` and converts it to a corresponding `~/.emacs.d/*.el' file, and then evaluates the files altering the Emacs active session. Comments are prefixed with [varying numbers of semicolons](https://www.gnu.org/software/emacs/manual/html_node/elisp/Comment-Tips.html). In classic Unix fashion, [you are not expected to understand this](https://en.wikipedia.org/wiki/Lions%27_Commentary_on_UNIX_6th_Edition,_with_Source_Code#.22You_are_not_expected_to_understand_this.22).

    ;;; init.el --- Where all the magic begins
    ;;
    ;; Originally from:
    ;;     http://orgmode.org/worg/org-contrib/babel/intro.html#literate-emacs-init
    ;;
    ;; This file loads Org-mode and then loads the rest of our Emacs initialization
    ;; from Emacs lisp embedded in literate Org-mode files.

    ;; Emacs    25.1.1
    ;; Org-Mode 9.0.3

    ;; Load up Org Mode and Org Babel for elisp embedded in Org Mode files.
    (setq dotfiles-dir (file-name-directory (or (buffer-file-name) load-file-name)))

    (let* ((org-dir (expand-file-name
                 "lisp" (expand-file-name
                         "org" (expand-file-name
                                "src" dotfiles-dir))))
           (org-contrib-dir (expand-file-name
                         "lisp" (expand-file-name
                                 "contrib" (expand-file-name
                                            ".." org-dir))))
           (load-path (append (list org-dir org-contrib-dir)
                          (or load-path nil))))
      ;; Load up Org-mode and Org-babel.
      (require 'org-install)
      (require 'ob-tangle))

    ;; Load up all literate org-mode files in this directory.
    (mapc #'org-babel-load-file (directory-files dotfiles-dir t "\\.org$"))
