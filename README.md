# up - the Ultimate Plumber

**up** is the **Ultimate Plumber**, a tool for writing Linux pipes in a
terminal-based UI interactively, with instant live preview of command results.

The main **goal** of the Ultimate Plumber is to help **interactively and
incrementally explore textual data** in Linux, by making it easier to quickly
build complex pipelines, thanks to a **fast feedback loop**. This is achieved
by boosting any typical **Linux text-processing utils** such as `grep`, `sort`,
`cut`, `paste`, `awk`, `wc`, `perl`, etc., etc., by providing a quick,
**interactive, scrollable preview** of their results.

[![](up.gif)](https://asciinema.org/a/208091)

## Usage

**[Download *up* for Linux](https://github.com/akavel/up/releases/download/v0.1/up)**

To start using **up**, redirect any text-emitting command (or pipeline) into it
— for example:

    $ lshw |& ./up

then:

- use ***PgUp/PgDn*** and ***Ctrl-[←]/Ctrl-[→]*** for basic browsing through
  the command output;
- in the input box at the top of the screen, start **writing any bash
  pipeline**; the Ultimate Plumber will **execute the command as you type it**,
  and immediately show you the output of the pipeline in the **scrollable
  window** below (replacing any earlier contents)
    - For example, you can try writing:
      `grep network -A2 | grep : | cut -d: -f2- | paste - -`
      — on my computer, the screen then shows the pipeline and a scrollable
      preview of its output like below:

             | grep network -A2 | grep : | cut -d: -f2- | paste - -
             Wireless interface      Centrino Advanced-N 6235
             Ethernet interface      RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller

- when you are satisfied with the result, you can **press *Ctrl-X* to exit**
  the Ultimate Plumber, and the command you built will be **written into
  `up1.sh` file** in the current working directory (or, if it already existed,
  `up2.sh`, etc., until 1000, based on [Shlemiel the Painter's
  algorithm](https://www.joelonsoftware.com/2001/12/11/back-to-basics/)).
  Alternatively, you can press ***Ctrl-C*** to quit without saving.
- If the command you piped into *up* is long-running (in such case you will see
  a tilde `~` indicator character in the top-left corner of the screen, meaning
  that *up* is still waiting for more input), you may need to press
  ***Ctrl-S*** to temporarily freeze *up*'s input buffer (a freeze will be
  indicated by a `#` character in top-left corner), which will inject a fake
  EOF into the pipeline; otherwise, some commands in the pipeline may not print
  anything, waiting for full input (especially commands like `wc` or `sort`,
  but `grep`, `perl`, etc. may also show incomplete results). To unfreeze back,
  press ***Ctrl-Q***.

## Additional Notes

- The pipeline is passed verbatim to a `bash -c` command, so any bash-isms should work.
- The input buffer of the Ultimate Plumber is currently fixed at **40 MB**. If
  you reach this limit, a `+` character should get displayed in the top-left
  corner of the screen. (This is intended to be changed to a
  dynamically/manually growable buffer in a future version of *up*.)

## Future Ideas

- This is version 0.1 of *the Ultimate Plumber*: a minimal viable product I was
  comfortable to release to the public, hoping it might be of use to some of
  you already.
- I have quite a lot of ideas for further experimentation of development of
  *up*, including but not limited to:
    - [RIIR](https://rust-lang.org) (once I learn enough of Rust... at some
      point in future... maybe...) — esp. to hopefully make *up* be a smaller
      binary (and also to maybe finally learn some Rust); though I'm somewhat
      afraid if it might ossify the codbase and make harder to develop
      further..? ...but maybe actually converse?...
    - Maybe it could be made into an UI-less, RPC/REST/socket/text-driven
      service, like gocode or [Language Servers](https://langserver.org/), for
      integration with editors/IDEs (emacs? vim? VSCode?...) I'd be especially
      interested in eventually merging it into [Luna
      Studio](https://luna-lang.org/); RIIR may help in this. (Before this, as
      a simpler approach, multi-line editing may be needed, or at least
      left&right scrolling of the command editor input box. Also, some kind of
      jumping between words in the command line; readline's *Alt-b* & *Alt-f*?)
    - Make it possible to [capture output of already running
      processes](https://stackoverflow.com/a/19584979/98528)! (But maybe that
      could be better made as a separate, composable tool! In Rust?)
    - Adding tests... (ahem; see also
      [#1](https://github.com/akavel/up/issues/1)) ...also write `--help`...
    - Making it work on Windows,
      somehow[?](https://github.com/mattn/go-shellwords) Also, obviously, would
      be nice to have some CI infrastructure enabling porting it to MacOSX,
      BSDs, etc., etc...
    - Integration with [fzf](https://github.com/junegunn/fzf) and other TUI
      tools? I only have some vague thoughts and ideas about it as of now, not
      even sure how this could look like.
    - Adding more previews, for each `|` in the pipeline; also forking of
      pipelines, merging, feedback loops, and other mixing and matching (though
      I'd strongly prefer if [Luna](https://luna-lang.org) was to do it
      eventually).
- If you are interested in financing my R&D work, contact me by email at:
  czapkofan@gmail.com, or [on keybase.io as akavel](https://keybase.io/akavel).
  I suppose I will probably be developing the Ultimate Plumber further anyway,
  but at this time it's purely a hobby project, with all the fun and risks this
  entails.

— *Mateusz Czapliński*  
*October 2018*
