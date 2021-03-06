# Introduction

__livefrog__ is a utility written in [Racket](http://racket-lang.org),
used to migrate LiveJournal posts and comments to the
[Frog](https://github.com/greghendershott/frog/) blogging platform. It
uses the files dumped by either [ljdump](http://hewgill.com/ljdump/)
or [ljmigrate](http://github.com/ceejbot/ljmigrate) to create the
corresponding source files for Frog.


# Installation

__livefrog__ is available via Racket's
[Planet2](http://pkg.racket-lang.org).

```
raco pkg install livefrog
```

If that doesn't work, install the dependencies, and __livefrog__
itself, from the local disk.

```
git clone https://github.com/jbclements/sxml.git
git clone https://github.com/greghendershott/frog.git
git clone https://github.com/ebzzry/livefrog.git
raco pkg install frog/ sxml/ livefrog/
```

The trailing slashes are important, to tell `raco` that you are
installing from local directories. Without it, it will try to fetch
the sources from the internet.


# Usage

This sections contains instructions for creating files suitable for
use with Frog.

## Basics

To create a Markdown file from the file entry.xml

```
raco livefrog -m entry.xml
```

That, however, becomes cumbersome if you're going to manage more than
a hundred entries. To automatically "pick up" the files created by
[ljdump](http://hewgill.com/ljdump) or
[ljmigrate](http://github.com/ceejbot/ljmigrate), and convert them to
Markdown:

```
raco livefrog -am
```

Bear in mind, though, that [ljdump](http://hewgill.com/ljdump) and
[ljmigrate](http://github.com/ceejbot/ljmigrate) differs how the trees
for the data are created. [ljdump](http://hewgill.com/ljdump) has the
following tree format, where USERNAME is your LiveJournal account
name:

```
ljdump/
  build
  ChangeLog
  convertdump.py
  USERNAME/
    L-1
    L-2
    C-2
    L-3
    ...
  ljdump.config
  ljdump.config.sample
  ljdump-gui.py
  ljdump.py*
  README.txt
  TODO
```

[ljmigrate](http://github.com/ceejbot/ljmigrate), on the other hand,
uses a different format:

```
ljmigrate/
  LICENSE.text
  ljmigrate.cfg
  ljmigrate.cfg.sample
  ljmigrate.py*
  README.md
  README_windows.txt
  TODO
  www.livejournal.com/
    USERNAME/
      entry00001/
        entry.xml
      entry00002/
        entry.xml
        comment.xml
      html/
      metadata/
      userpics/
```

After creating the Markdown Frog source files, you may now copy them
to your Frog source directory, designated at `_src/posts/`

## Comments

Frog, by default, uses [Disqus](http://disqus.com) to handle the
comments. To import comments to this platform, we need to generate an
XML file that must adhere to Disqus' comment import rules.

To create a file, named `comments.xml` that will be used for importing
comments, to be used with
[http://import.disqus.com/](http://import.disqus.com/), using
`foo.bar.com` as the root site:

```
raco livefrog -s foo.bar.com -c comments.xml
```


# Updating

If you installed __livefrog__ using the first method described in the
section *Introduction*, you can update it by running:

```
raco pkg update livefrog
```

However, if you used the latter method, you may update it by pulling
the updates, uninstalling __livefrog__, then installing it
again:

```
cd livefrog
git pull origin master
cd ..
raco pkg remove livefrog
raco pkg install livefrog/
```


# Miscellany

To reduce typing, you may optionally create an alias to `raco
livefrog` in your shell.

Sh-like shells:
```
echo 'alias livefrog="raco livefrog"' >> ~/.bashrc
```

Csh-like shells:
```
echo 'alias livefrog raco livefrog' >> ~/.cshrc
```

Replace `.bashrc`, and `.cshrc`, with the appropriate init file for
your shell.
