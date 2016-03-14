---
layout: section
order: 8
title: Hacks
---

This is an area for Emacs Scala developers to contribute useful functions that they use as part of their custom workflow.

When adding a hack, please provide good lisp documentation along with some accompanying text explaining how you use the function as part of your workflow.

- TOC
{:toc}

## General

These are Emacs hacks that improve productivity in any programming language. Some further ideas can be found in: [Sam's .emacs.d](https://github.com/fommil/dotfiles/tree/master/.emacs.d)

### Projects

If you're not using [`projectile`](https://github.com/bbatsov/projectile), install it now!

```elisp
(use-package projectile
  :demand
  :init   (setq projectile-use-git-grep t)
  :config (projectile-global-mode t)
  :bind   (("s-f" . projectile-find-file)
           ("s-F" . projectile-grep)))
```

### Undo

Emacs' `C-/` undo can become very confusing if you want to go back to a previous state. [`undo-tree`](http://melpa.org/#/undo-tree) fixes that.

```elisp
(use-package undo-tree
  :diminish undo-tree-mode
  :config (global-undo-tree-mode)
  :bind ("s-/" . undo-tree-visualize))
```

### Flex Search

AKA the "ido vs Helm war".

The following will set up [ido](https://www.emacswiki.org/emacs/InteractivelyDoThings) for flexible searching anywhere where Emacs would give a normal text entry box. It takes some getting used to but is a game changer. A lot of Emacs hackers prefer to use [Helm](https://github.com/emacs-helm/helm).

```elisp
(use-package flx-ido
  :demand
  :init
  (setq
   ido-enable-flex-matching t
   ;; C-d to open directories
   ;; C-f to revert to find-file
   ido-show-dot-for-dired nil
   ido-enable-dot-prefix t)
  :config
  (ido-mode 1)
  (ido-everywhere 1)
  (flx-ido-mode 1))
```

### Highlight symbol

It's really nice to be able to highlight all the symbols in a file that match the current one.

```elisp
(use-package highlight-symbol
  :diminish highlight-symbol-mode
  :commands highlight-symbol
  :bind ("s-h" . highlight-symbol))
```

### Goto Last Change

Reading some code and want to return to where you last made some changes? `goto-chg` is for you...

```elisp
(use-package goto-chg
  :commands goto-last-change
  ;; complementary to
  ;; C-x r m / C-x r l
  ;; and C-<space> C-<space> / C-u C-<space>
  :bind (("C-." . goto-last-change)
         ("C-," . goto-last-change-reverse)))
```

and as noted in the comments, you might also want to learn about [Bookmarks](https://www.gnu.org/software/emacs/manual/html_node/emacs/Bookmarks.html) and how to use the [Mark](https://www.gnu.org/software/emacs/manual/html_node/emacs/Mark.html) effectively.


### Popup Summary

Want to see a visual summary of your current file?

```elisp
(use-package popup-imenu
  :commands popup-imenu
  :bind ("M-i" . popup-imenu))
```

start typing to narrow down the choices. If the view gets out of date, do an `M-x revert-buffer`.

In scala code, this summary is provided by ENSIME.

### Git

One of the best things about Emacs is [magit](http://magit.vc/manual/magit/#Top). It deserves a full read of the manual.

```elisp
(use-package magit
  :commands magit-status magit-blame
  :init (setq
         magit-revert-buffers nil)
  :bind (("s-g" . magit-status)
         ("s-b" . magit-blame)))
```

Some nice supporting packages are `git-gutter` and `git-timemachine`. If you decide to use `git-timemachine`, make sure to disable ENSIME when travelling in time (the compiler sees duplicate symbol definitions)

```elisp
(add-hook 'git-timemachine-mode-hook (lambda () (ensime-mode 0)))
```

### Completions

ENSIME uses the excellent [`company-mode`](http://company-mode.github.io/) for completion but it is a general completion framework that works in a variety of languages.

```elisp
(use-package company
  :diminish company-mode
  :commands company-mode
  :init
  (setq
   company-dabbrev-ignore-case nil
   company-dabbrev-code-ignore-case nil
   company-dabbrev-downcase nil
   company-idle-delay 0
   company-minimum-prefix-length 4)
  :config
  ;; disables TAB in company-mode, freeing it for yasnippet
  (define-key company-active-map [tab] nil))
```

Now all you need to do is enable `(company-mode)` in any buffer and you will get completions.

### Snippets

[Yet Another Snippet (YAS)](http://capitaomorte.github.io/yasnippet/) provides a wide range of snippets for various languages.

```elisp
(use-package yasnippet
  :diminish yas-minor-mode
  :commands yas-minor-mode
  :config (yas-reload-all))
```

See [github.com/AndreaCrotti/yasnippet-snippets](https://github.com/AndreaCrotti/yasnippet-snippets) for the snippets available broken down by language (including some sensible scala snippets).

Add your own templates using the same format in your `~/.emacs.d/snippets` folder.

### Templates

YAS can also be used to insert boilerplate when you create a new file. The following works well across projects when combined with [Local Variables](https://www.gnu.org/software/emacs/manual/html_node/emacs/File-Variables.html)

```elisp
(defvar-local yatemplate-owner user-full-name
  ;; https://github.com/mineo/yatemplate/issues/4
  "The copyright owner for the buffer.
Particularly useful when combined with `dir-locals.el'.")
(defvar-local yatemplate-license "http://www.gnu.org/licenses/gpl.html"
  "The license (usually a URL) for the buffer.
It is always better to explicitly list the license per file than
to refer to the LICENSE file. Particularly useful when combined
with `dir-locals.el'.")
(put 'yatemplate-owner 'safe-local-variable #'stringp)
(put 'yatemplate-license 'safe-local-variable #'stringp)

(use-package yatemplate
  :defer 2 ;; WORKAROUND https://github.com/mineo/yatemplate/issues/3
  :config
  (auto-insert-mode)
  (setq auto-insert-alist nil)
  (yatemplate-fill-alist))
```

e.g. create a file called `.dir-locals.el` at the base of your work project containing

```elisp
((nil . ((yatemplate-owner . "Robot Overlord Masters PLC")
         (yatemplate-license . "All Your Base Are Belong to Us!"))))
```

and create a template for scala files by creating `~/.emacs.d/templates/50:.*scala`

```elisp
// Copyright: `(format-time-string "%Y")` `yatemplate-owner`
// Licence: `yatemplate-license`

$0
```

and a similar one for Emacs Lisp like in [`50:.*el`](https://github.com/fommil/dotfiles/blob/master/.emacs.d/templates/50:.*el).

### Smart Parentheses

Once you realise that you can edit code on the level of code blocks, you'll wonder how you ever survived without it. Give [smartparens](https://github.com/Fuco1/smartparens) a try with some recommended keybindings. It's another package deserving of reading the full manual.

```elisp
(use-package smartparens
  :diminish smartparens-mode
  :commands
  smartparens-strict-mode
  smartparens-mode
  sp-restrict-to-pairs-interactive
  sp-local-pair
  :init
  (setq sp-interactive-dwim t)
  :config
  (require 'smartparens-config)
  (sp-use-smartparens-bindings)

  (sp-pair "(" ")" :wrap "C-(") ;; how do people live without this?
  (sp-pair "[" "]" :wrap "s-[") ;; C-[ sends ESC
  (sp-pair "{" "}" :wrap "C-{")

  (bind-key "C-<left>" nil smartparens-mode-map)
  (bind-key "C-<right>" nil smartparens-mode-map)
  (bind-key "s-{" 'sp-rewrap-sexp smartparens-mode-map)
  (bind-key "s-<delete>" 'sp-kill-sexp smartparens-mode-map)
  (bind-key "s-<backspace>" 'sp-backward-kill-sexp smartparens-mode-map))
```


## Scala

These are Emacs hacks that improve productivity in Scala only, some don't even require ENSIME.

### Tags

Before there was ENSIME, there was [exuberant-ctags](http://ctags.sourceforge.net/) (`sudo apt-get install exuberant-ctags`). But `ctags` doesn't support Scala syntax out of the box, so add this to your `~/.ctags` file

```
-e
--langdef=scala
--langmap=scala:.scala
--regex-scala=/\bclass\s+(\w+)/\1/c,classes/
--regex-scala=/\bobject\s+(\w+)/\1/c,objects/
--regex-scala=/\btrait\s+(\w+)/\1/t,traits/
--regex-scala=/\btype\s+(\w+)/\1/T,types/
--regex-scala=/\bdef\s+(\w+)/\1/m,methods/
--regex-scala=/\bval\s+(\w+)/\1/l,constants/
--regex-scala=/\bvar\s+(\w+)/\1/l,variables/
--regex-scala=/\bpackage\s+((\w|[.])+)/\1/p,packages/
```

Projectile adds support for building a `TAGS` file for your project with `C-c p R` and searching it with `C-c p j` but the experience is greatly improved if you install `etags-select` which will help resolve multiple hits

```elisp
(use-package etags-select
  :commands etags-select-find-tag)
```

And if `ensime-edit-definition` fails, you can automatically fall back to `TAGS` searching with

```elisp
(defun ensime-edit-definition-with-fallback ()
  "Variant of `ensime-edit-definition' with ctags if ENSIME is not available."
  (interactive)
  (unless (and (ensime-connection-or-nil)
               (ensime-edit-definition))
    (projectile-find-tag)))

(bind-key "M-." 'ensime-edit-definition-with-fallback ensime-mode-map)
```

A side effect of using ENSIME is that you forget that other modes don't have `ensime-edit-definition`, so 

```elisp
(global-set-key (kbd "M-.") 'projectile-find-tag)
(global-set-key (kbd "M-,") 'pop-tag-mark)
```

will at least give you something.

### Newlines in comments

Redefine what `RET` means in `scala-mode` so that comment blocks automatically get an asterisk.

```elisp
(defun scala-mode-newline-comments ()
  "Custom newline appropriate for `scala-mode'."
  ;; shouldn't this be in a post-insert hook?
  (interactive)
  (newline-and-indent)
  (scala-indent:insert-asterisk-on-multiline-comment))

(bind-key "RET" 'scala-mode-newline-comments scala-mode-map)
```

### Parenthesis Formatting

Above we introduced smartparens, and now we'll customise it for scala use. This will add some whitespace padding depending on what you type after opening `(` or `{` parenthesis.

```elisp
(sp-local-pair 'scala-mode "(" nil :post-handlers '(("||\n[i]" "RET")))
(sp-local-pair 'scala-mode "{" nil :post-handlers '(("||\n[i]" "RET") ("| " "SPC")))
```

and the following will restrict the smartparents navigation commands to just the brackets (ignoring Emacs' s-expression interpretation of the Scala language as provided by `scala-mode`. Bless him, he thinks everything is Lisp.)

```elisp
(defun sp-restrict-c (sym)
  "Smartparens restriction on `SYM' for C-derived parenthesis."
  (sp-restrict-to-pairs-interactive "{([" sym))

(bind-key "s-<delete>" (sp-restrict-c 'sp-kill-sexp) scala-mode-map)
(bind-key "s-<backspace>" (sp-restrict-c 'sp-backward-kill-sexp) scala-mode-map)
(bind-key "s-<home>" (sp-restrict-c 'sp-beginning-of-sexp) scala-mode-map)
(bind-key "s-<end>" (sp-restrict-c 'sp-end-of-sexp) scala-mode-map)
```

### Expand region

If `expand-region` is installed, e.g.

```elisp
(use-package expand-region
  :commands 'er/expand-region
  :bind ("C-=" . er/expand-region))
```

then the ENSIME specific backend may be enabled by importing the functionality

```elisp
(require 'ensime-expand-region)
```

this can be really useful when moving around code blocks.


### Jump to code

Some people find it useful to add the following function to their `scala-mode-hook` as it will jump over the boilerplate at the beginning of most files:

```elisp
(scala-mode:goto-start-of-code)
```

### All together now

Don't forget to add all these wonderful packages to your scala mode! Here's a fully worked example with a further hack to use `TAGS` completions in `company-mode` only for small(ish) projects, which might give you your own ideas.

```elisp
(add-hook 'scala-mode-hook
          (lambda ()
            (show-paren-mode)
            (smartparens-mode)
            (yas-minor-mode)
            (git-gutter-mode)
            (company-mode)
            (ensime-mode)

            ;; for small projects, use TAGS for completions
            (make-local-variable 'company-backends)
            (projectile-visit-project-tags-table)
            (setq company-backends
             (if (and tags-file-name
                      (<= 20000000 (buffer-size (get-file-buffer tags-file-name))))
                 '(ensime-company (company-keywords company-dabbrev-code company-yasnippet))
               '(ensime-company (company-keywords company-dabbrev-code company-etags company-yasnippet))))

            (scala-mode:goto-start-of-code)))
```


## Emacs Lisp

These are Emacs hacks that improve productivity in Emacs Lisp, e.g. when [contributing to `ensime-emacs`](/editors/emacs/contributing).
