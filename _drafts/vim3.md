---
draft: true
layout: post
title: Vim After 15 Years
description: XXX
image: /images/vim/screen.png
---

It's been about four years since my earlier [Vim After 11 Years](XXX), and since I've been writing a lot of code with Vim lately, I figured that it's about time for an update.

My core configuration hasn't changed much. However, the last few years have seen an advent of new plugins, new Vim features (like asynchronous operations), and a new website to let us explore new plugins.

## On Plugins

There always seems to be a backlash against using Vim with a slew of plugins.
Part of this is reasonable speculation -- any system which allows users to add
unordered extensions which patch the core system unsympathetically can easily
become a shit show. Just look at WordPress, or XXX, or even Mac OS Classic if
you were there 20 years ago. Extensions become patches on top of patches,
there's no formal system for declaring dependencies, and debugging
incompatibilities becomes the norm. Vim plugins aren't that bad -- debugging an interaction between X and Y usually involves googling "vim X with Y" -- and it's certainly not as bad a situation as binary-searching your way through conflicts like with [Conflict Catcher](XXX) on Mac OS Classic.

Another resistance towards plugins is the resistance towards straying away from the core set of Vim functionality, such as movement. If you use Vim, you're now in the subset of people who demand that editing text be fast and efficient. If you use a Vim movement plugin like [EasyMotion](XXX) or [vim-sleuth](XXX), you probably have the opinion that the tool you're using is more efficient than vanilla Vim, whose users in turn think that their use of Vim is more efficient than whatever text editors they larger group of people are using.

Yet plugins have always been a part of Vim, despite the lackluster programming language VimScript that's behind it. And with the recent improvements to Vim and VimScript, such as [asynchronous operations](XXX) and [modern types](XXX), the plugin ecosystem is thriving. A new plugin site, [VimAwesome](XXX), has finally cropped up to supplant the anachronistic [www.vim.org](XXX), and it allows you to peruse popular plugins.

My opinion on plugins is thus: If a plugin provides useful functionality that I wish were built into Vim, it's worth installing. Otherwise, keep the number of plugins at a minimum to avoid interaction problems and slow load times of the application and files. Thus, the plugins you see listed here are the result of 

## FZF (Fuzzy Finder)

After TextMate made us understand that the quickest way from firing neurons to opening a file is to use an interactive fuzzy search. So us Vim people used [Command-T](XXX) for a while but nobody liked the Ruby dependency, so we all started using [Ctrl-P](XXX). But the new hotness is [FZF](XXX), and for good reason.

FZF is _astoundingly_ fast. In the codebase that I'm currently working on, Ctrl-P is laggy with fuzzy file search and nearly unusable with fuzzy tag search. FZF is nearly instant. Ctrl-P had to be configured with a long [`wildignore`](XXX) block, but FZF inherits your

TextMate and Sublime Text have taught us that the fastest way to go from firing neurons to viewing a file is by _fuzzy finding_, or rather, typing part of a filename or path to open a file, even if the characters aren't adjacent or you making a spelling error. This is such a useful technique that most editors now implement it. For a long time the best solution for Vim was the Ctrl-P plugin, but a new tool, FZF, is much faster, and thus moreforgiving.

Footnote: While FZF and Ctrl-P and other editors support partial/fuzzy searching for pathnames, I'm really hoping for someone to implement a first-character search for Vim. For example, if you're an Intelli-J IDEA user and you want to open the class FooFactoryGeneratorBean, all you have to do is hit a key and type <kbd>FFGB</kbd> and FooFactoryGeneratorBean will be highlighted immediately. This would be useful for tag searching since class names are often camel case regardless of language. Or at least treat characters before underscores get treated as the first character, so typing something like <kbd>fbbq</kbd> would highlight a file like `lib/core/foo_bar_baz_quux.js`. Just some ideas.

Getting started with FZF unfortuanltely involves a few steps. On macOS with [Homebrew][homebrew], it's basically:

1. `brew install fzf --head`
1. Add the separate `vim-fzf` plugin
1. Add the following to your vimrc:

XXXX

You see, the `fzf` utility, which is editor- and platform-independent, comes with a Vim plugin, but its functionality is minimal and very opinionated. Luckily, the [vim-fzf](XXX) plugin provides all of the fuctionality you'd expect coming from Ctrl-P or similar. The most useful commands are `:Files` and `:Buffers`, which I've bound to <kbd>&lt;leader&gt;t</kbd> and <kbd>;</kbd>. 

Binding <kbd>;</kbd> is important to me because I live and breathe buffers -- I never use tabes -- so being able to switch focus to the exact place I'm thinking of is important. Combine this with the [`RestoreLineNumber()`](XXX) function I added is also important because it means that the cursor will be restored to the last position I was editing when I swap between buffers. Also, don't forget the previous post where I [outlined](XXX) binding <kbd>Ctrl-E</kbd> to swapping to the previously-opened buffer. Once you're using a one-keystroke `:Buffers` binding and another bidning to take you back where you were -- either <kbd>Ctrl-Shift-6</kbd> or <kbd>Ctrl-o</kbd> -- you'll be able to navigate within the current set of brain context at lightspeed.

So here's what it looks like:

XXX pic of using fzf

A few important points about FZF:

* You can tell it to use `ag` (the [silver searcher](XXX)), which will in turn respsect your `.gitignore` and your `.agignore` files. You no longer need to keep a giant `wildignore` string in your `vimrc`. Though, if you were doing that, at least consider the [rootignore](XXX) plugin which configures `wildignore` according to your `.gitignore` and other ignore files. The point is, `ag` is blisteringly fast, and FZF can make use of what you've already configured.

* FZF is really, really fast. Ctrl-P used to do OK on a 30,000-file codebase on my 2015-era MacBook Pro, but it really started to croak during a fuzzy search of our taglist. It was slow to the point of being unusable. FZF, however, shows no speed difference between files or tags -- it's blazingly fast either way. And as [the docs](XXX) say, if you really need speed when using `:Files`, try [setting it up](XXX) to use `git ls-files` to generate a list of files to match against.

* It works in the shell, too. FZF comes with bindings for Zsh, Bash, and the Fish shell. In Zsh, I can hit <kbd>Ctrl-t</kbd> to instantly fuzzy-find any file in the current directory. And since I've configured FZf to use `ag`, it'll ignore anything excluded by `.gitignore`. It's beatiful.

* It doesn't work well with MacVim. The [current fix](XXX) is to install XQuartz so it can run an xterm. Ugh. Gross. But I [don't use MacVim much anyway], so this point is moot for me.

In short, FZF is the new way to quickly locate and work on files and directories, whether in the shell or within Vim. 

## ag.vim + meta-k - quickfix + vim-unimpaired + \x + QFEnter mention

If FZF is the new way to locate and act upon a _single_ path, `ag` is the new way to act upon _multiple_ paths. Well, it's not really new, and I've heard of some newfangled things that are allegedly _faster_, but I find that hard to believe because `ag` is so fast that it seems impossible. I feel like `ag` is one of those things that is so fast that I need to [spend a weekend trying to recreate it to figure out why it's so impossibly fast].

Anyway, the best way to use `ag` from within Vim seems to be [ack.vim](XXX), which gives you an `:Ag` command. Using it plus any args is like running `ag` from the command line, except that it opens the quickfix window with the list of search results:

XXX image of using :Ag and quickfix

Note that `:Ag` by default will jump to the first result in the quickfix list. If you don't want this to happen, use `:Ag!`. Personally, I wish this had been reversed, because I often want to page through the search results before making a selection of where to go.

The obvious way to navigate through the quickfix window is by moving the cursor there and hitting <kbd>Enter</kbd>. It is much faster, however, to check out the (vim-unimpaired)[XXX] plugin, which adds useful bindings like <kbd>[</kbd><kbd>q</kbd> and <kbd>]</kbd><kbd>q</kbd>, which our bound to `:cprev` and `:cnext`, which cycle to the previous and first quickfix search result, respectively.

vim-unimpared has a bunch of other bindings, and I don't remember them offhand. But the main reaoson I installed was that I couldn't find a satisfactory cross-platform single key binding for `:cprev` and `:cnext`. Once I saw the vim-unimpared pattern of leading with <kbd>[</kbd> and <kbd>]</kbd> followed by a letter, such as <kbd>q</kbd> for _quickfix_ and <kbd>b</kbd> for _buffer_, I realized that that was a much smarter way of doing things.

This method of searching for things and using the quickfix window is so useful to me that I wrote a quick binding, <kbd>Meta-k</kbd>, which searches for the current word under the cursor using `:Ag!`. This is great because, as good as Exhuberant Ctags is for finding tags in Ruby and CoffeeScript, it's far from complete, and sometimes you just need to search for that damned word that you're staring at, and <kbd>Meta-k</kbd> will do that.

XXX gif of using Meta-k

Finally, after I'm done searching and navigating, I've bound <kbd>\</kbd><kbd>x</kbd> to close the quickfix window. Usually a few presses of <kbd>Ctrl-o</kbd>, a built-in binding to pop the location stack, will get me back to where I was originally. Other times I'll have to use my <kbd>;</kbd> binding to bring up the buffer list. Maybe I should set a [global mark](XXX) in my <kbd>Meta-k</kbd> binding, like `"O`, so that <kbd>'</kbd><kbd>O</kbd> will always take me back to where I started.

## tmux + autowrite + no gvim/MacVim + Terminal.app vs iTerm, splits

I mentioned in my earlier posts about how I'm not a heavy MacVim user. I've only had three reasons to use MacVim:

1. It's a better default application than TextEdit to open `.txt` files on macOS.
1. I couldn't deal with a single-window line selection with the mouse in the terminal for some reason, probably to how there's sometimes a maximum column limit supported by the mouse in the termianl.
1. I wanted the _true_ Solarized color scheme, and not the [slightly-off color scheme created when Solarized is stepped-down to 256 colors](XXX) (Gosh! The agony!) (But seriously, the terminal-based Solarized schemes are just fine.)

A few months ago I saw a (random tweet)(XXX) where a Vim user showed off using tmux to create a kind of IDE-with-REPL solution. I felt inspired, so I started using tmux again after years of avoiding it, and I've been pleasantly surprised. **As of now, tmux is now my nearly-fulls-screen working environment, and Vim usually takes up one of the tmux panes.** Or it will take up a whole pane temporarily when I press <kbd>Ctrl-t</kbd><kbd>z</kbd> to "zoom" a pane temporarily.

Side note: Why did I avoid tmux for so long? When it was new in 2010 or so, I experienced a few crashes on Linux and Mac OS X, and compared to the incumbent GNU Screen multiplexer, I felt that that was unacceptable. I also experienced massive input lag when typing into Vim via tmux. Both of these were unnaceptable. But as of 2017, I haven't had a crash, nor do I notice any input latency, so it looks like those bugs have been fixed. (Thanks!)

What's the big deal about tmux? Well, vertical splits are cool, but you probably already get that with your terminal emulator, like iTerm. The real win, however, is the ability to (send keys)[XXX] to other tmux panes. That means I can set up a fancy REPL loop where I can edit in one pane and execute commands and see results in another pane, like most IDEs.

Many people have tried to add terminal emulators _inside_ Vim to achieve an IDE-like setup, but the results have been lackluster. Sometimes you need to learn a new keystroke and remember what state you're in, or sometimes the terminal emulation is broken, or both. But with tmux, you have a real terminal next to your Vim, or what feels like _inside_ Vim, and there's no additional overhead to memorizing what mode you're in if you're already inside tmux.

But back to the big win: Sending keys to tmux. As an example, here's what my terminal looked liked for a recent project:

XXXX pick of gamotron

On the right I have Vim, and on the left I have a few panes. One is running the server I'm working on, and another pane is used to send commands to it. The command is a simple `curl` command. Running the command lets me see the output in nicely formatted JSON (thanks to [jq](XXX)), and keeping the server pane open lets me make sure there weren't any errors.

The normal loop would be to make a change in Vim, hit <kbd>:</kbd><kbd>w</kbd><kbd>Enter</kbd> to save, the <kbd>Ctrl-t</kbd><kbd>h</kbd> to move to the left pane, then <kbd>Up</kbd><kbd>Enter</kbd> to repeat the command, and <kbd>Ctrl-t</kbd><kbd>l</kbd> to go back to Vim. But with a simple binding, that can be accomplished with a single keystroke from _within Vim_:

XXX nmap tmux send-keys

To explain, this executes the `tmux send-keys` command which tells it to send keys to session, window and pane (`0:0.2`) where I had run `curl` previously. It then sends <kbd>Ctrl-j</kbd>, which is equivalent to hitting <kbd>Up</kbd>, which pulls the previous command from history, and then <kbd>Enter</kbd> to execute it.

I've used this so much that I set up a little global variable to make this workflow easier. When I start a new Vim session, instead of typing `:nmap` and hitting <kbd>Up</kbd> a few times to find my previous `nmap` binding command, all I need to do is `:let g:tmux_current_pane = X`, where `X` is the `<session>:<window>:<pane>` identifier already shown in the bottom-right corner of my tmux sesison. Then <kbd>\</kbd><kbd>r</kbd> works as I'd expect.

Finally, a note on terminals: I've been using iTerm2 instead of macOS's default Terminal.app for... a decade? It's been so long that I can't remember. But I recently noticed that typing in Vim inside tmux inside iTerm2 felt a bit sluggish. For comparison I tried using `urxvt` inside XQuarts, and it felt light lightning, so something was clearly adding latency, but I wasn't about to make `urxvt` my main terminal on macOS (you know, clipboard and focus issues and lack of high-DPI support, etc.). Coincidentally, a few days after noticing this, a [post on Hacker News](XXX) outlined input latency between terminals, and pointed out that Terminal.app is now significantly faster than iTerm2. So I switched to Terminal.app and haven't looked back. It's very, very fast, and since I'm using tmux, I don't miss the vertical splits at all. And since I was using a fancy [custom iTerm2 color theme](XXX), I was pleased to find a project which has [converted all of the iTerm2 color themes to Terminal.app](XXX). Nice!

Finally, I lied a little. I do miss one thing about iTerm2 vertical splits, and that's the very occasional case where I want a different font size in one pane than another. It's easy to do this with iTerm, or in fact _any_ editor other than those inside terminals.

## goyo + ProseMode + supertab + fzf for thesaurus?

Distraciton-free writing is pretty popular, and for good reason: it works. But I want to do my writing in Vim. So what's the solution?

A great plugin is [Goyo](XXX), which adds lots of padding to your buffer and hides all the cruft. It's aware of airline/powerline/lightline and whatever, so those get hidden too ([mostly](XXX)). I've combined that with a few other custom settings which I call **Prose Mode**:

XXX insert snippet of ProseMode

Basically, turn on Goyo, get rid of any funny source-code like indenting, and make autocompletion pull words from the thesaurus and dictionary to write even faster. It also changes the color scheme from my normal dark Molokai theme to the light version of Solarized. The latter is important because it becomes a mental reminder that I'm in "writing mode," and unlike in coding mode, I haven't given myself permission to mess around or get distracted, since my goal is to produce words and later edit them. Also, having (different highlight colors)[XXX] for misspellings in code and text feels like a useful stylistic choice.

## ale > syntastic

One of best modern additions to Vim is asynchronous functions. This was the most sorely needed feature in Vim and it is wonderful that Bram and the Vim maintainers finally implemented it.

The best manifestion of this new asynchronous behavior is a new lint checker, (ALE)[XXX], the Asynchronous Line EXXXXX. Now, when you write a file, the linter is run in the background. I've been writing a lot of Ruby for work, specifically in JRuby, and running the linter in JRuby can take a while to start up, so this is a welcomed addition.

XXX image of ruby linting with ALE

## lightline > airline/powerline

I used Powerline, which the newer "lighter-weight" Airline has replaced, for the last few years. After a while I decided that the information and widgets are more distracting than useful, so I switched to (lightline)[XXX] and spent a little extra effort to get my lightline status bar to look the way I wanted -- minimal information and distractions, and to show me the number of errors and warnings when things are wrong, and to show a solid ✓ when everything is peachy. I don't need to know the current file encoding or syntax.

XXX image of lightline

## fugitive + gitgutter + git-rhubarb + :Gbrowse + rootignore

If you're using Git, a few plugins are important.

[gitgutter](XXX) is a plugin that shows you the current git change status on a line-wise basis, like most editors do nowadays. I modified the defaults, which use distracting `+` and `-` characters, to simply show me a colored dot (`·`) for each line change. XXXXXXXXXXXXXXXX HERE

[vim-fugitive](XXX) is currently the single 

## buffers not tabs - bufkill, eunuch

## polyglot - vim-markdown

## other plugins: commentary, endwise+closetag, fugtivie, repeat, sleuth

## about editors - vs code / intellij / vim + incsearch from emacs

