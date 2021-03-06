# foreplay.vim

There's a REPL in foreplay, but you probably wouldn't have noticed if I hadn't
told you.  Such is the way with foreplay.vim.  By the way, this plugin is for
Clojure.

## Installation

Foreplay.vim doesn't provide indenting or syntax highlighting, so you'll want
[a set of Clojure runtime files](https://github.com/guns/vim-clojure-static).
You might also want [classpath.vim][] to run code when no REPL is available.

If you don't have a preferred installation method, I recommend
installing [pathogen.vim](https://github.com/tpope/vim-pathogen), and
then simply copy and paste:

    cd ~/.vim/bundle
    git clone git://github.com/tpope/vim-foreplay.git
    git clone git://github.com/tpope/vim-classpath.git
    git clone git://github.com/guns/vim-clojure-static.git

Once help tags have been generated, you can view the manual with
`:help foreplay`.

## Features

This list isn't exhaustive; see the `:help` for details.

### Transparent setup

Foreplay.vim talks to nREPL.  With Leiningen 2, it connects automatically
based on `target/repl-port`, otherwise it's just a `:Connect` away.  You can
connect to multiple instances of nREPL for different projects, and it will
use the right one automatically.

The only external dependency is that you have either a Vim with Python support
compiled in, or `ruby` in your path. (Don't ask.)

Oh, and if you don't have an nREPL connection, installing [classpath.vim][]
lets it fall back to using `java clojure.main`, using a class path based on
your Leiningen or Maven config.  It's a bit slow, but a two second delay its
vastly preferable to being forced out of my flow for a single command, in my
book.

[classpath.vim]: https://github.com/tpope/vim-classpath

### Not quite a REPL

You know that one plugin that provides a REPL in a split window and works
absolutely flawlessly, never breaking just because you did something innocuous
like backspace through part of the prompt?  No?  Such a shame, you really
would have liked it.

I've taken a different approach in foreplay.vim.  `cq`  (Think "Clojure
Quasi-REPL") is the prefix for a set of commands that bring up a *command-line
window* — the same thing you get when you hit `q:` — but set up for Clojure
code.

`cqq` prepopulates the command-line window with the expression under the
cursor.  `cqc` gives you a blank line in insert mode.

### Evaluating from the buffer

Standard stuff here.  `:Eval` evaluates a range (`:%Eval` gets the whole
file), `:Require` requires a namespace with `:reload` (`:Require!` does
`:reload-all`), either the current buffer or a given argument.  There's a `cp`
operator that evaluates a given motion (`cpp` for the expression under the
cursor).

Any failed evaluation loads the stack trace into the location list, which
can be easily accessed with `:lopen`.

### Navigating and Comprehending

I'm new to Clojure, so stuff that helps me understand code is a top priority.

* `:Source`, `:Doc`, `:FindDoc`, and `:Apropros`, which map to the underlying
  `clojure.repl` macro (with tab complete, of course).

* `K` is mapped to look up the symbol under the cursor with `doc`.

* `[d` is mapped to look up the symbol under the cursor with `source`.

* `[<C-D>` jumps to the definition of a symbol (even if it's inside a jar
  file).

* `gf`, everybody's favorite "go to file" command, works on namespaces.

Where possible, I favor enhancing built-ins over inventing a bunch of
`<Leader>` maps.

### Omnicomplete

Because why not?  It works in the quasi-REPL too.

### FAQ

> Why does it take so long for Vim to startup?

The short answer is because the JVM is slow.

The first time you load a Clojure file from any given project, foreplay.vim
sets about trying to determine your class path, leveraging either
`lein classpath` or `mvn dependency:build-classpath`.  This takes a couple of
seconds or so in the best case scenario, and potentially much longer if it
decides to hit the network.  (I don't understand why "tell me the class path"
requires hitting the network, but what do I know?)

Because the class path is oh-so-expensive to retrieve, foreplay.vim caches it
in `g:CLASSPATH_CACHE`.  By default, this disappears when you exit Vim, but
you can save it across sessions in `.viminfo` with this handy option:

    set viminfo+=!

The cache is expired when the timestamp on `project.clj` or `pom.xml` changes.

## Contributing

More than any other plugin, I'm in over my head here.  I tried to do my
homework, but you don't learn best practices overnight.  Please, open
[GitHub issues][] for bug reports and feature requests.  Even better than a
feature request is just to tell me the pain you're experiencing, and perhaps
some ideas for what might eliminate it.  I know Vimscript; you know Clojure.
Let's synergize.

I'm a stickler for [commit messages][], so if you send me a pull
request with so much as superfluous period in the subject line, I will
reject it, then TP your house.

[GitHub issues]: http://github.com/tpope/vim-foreplay/issues
[commit messages]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html

## Self-Promotion

Like foreplay.vim? Follow the repository on
[GitHub](https://github.com/tpope/vim-foreplay). And if
you're feeling especially charitable, follow [tpope](http://tpo.pe/) on
[Twitter](http://twitter.com/tpope) and
[GitHub](https://github.com/tpope).

## License

Copyright © Tim Pope.  Distributed under the same terms as Vim itself.
See `:help license`.
