---
draft: true
layout: post
title: Vim After 15 Years
description: XXX
image: /images/vim/vim3.png
---

It's been about four years since my earlier [Vim After 11 Years](XXX) post, which was well-received. I've been writing a lot of more code with Vim lately and I figured that it was about time for an update.

Since my previous post my core configuration hasn't changed much. However, the last few years have seen impressive growth in the Vim community. There have been an advent of new plugins, new Vim features (like asynchronous operations), and a new website to let us explore new plugins.

<figure>
<img src="images/vim/vim3.png"/>
<figcaption>A recent Vim session</figcaption>
</figure>


## On Plugins

There always seems to be a backlash against using Vim with lots of plugins.  Part of this is understandable suspicion -- any system which allows users to add unordered extensions to patch itself without hesitation can easily become a mess. Just look at WordPress. Or, if you were around 20 years ago, try remembering extensions in Mac OS Classic, which were patches on top of patches. With these systems there's no formal way to declare dependencies and debugging incompatibilities becomes the norm.

Vim plugins aren't that bad. Debugging an interaction between plugins _X_ and _Y_ usually involves googling "vim X with Y." It's certainly not as bad as binary-searching your way through extension conflicts like [Conflict Catcher](https://tidbits.com/article/1496) used do on Mac OS Classic.

Further resistance towards plugins seems to a kind of purist animosity against straying away from some _core set_ of Vim functionality. However, if you use Vim, you're already in a subset of people who demand that editing text be fast and efficient, so this is like a group of savants arguing about which of them is more eccentric. If you use one of the movement plugins like [EasyMotion](https://vimawesome.com/plugin/easymotion) or [vim-sneak](https://vimawesome.com/plugin/vim-sneak) then probably agree that the tool you're using is more efficient than vanilla Vim, and those users think that using Vim is more efficient than the other text editors the larger set of people in the world are using. And so on.

Plugins have always been a part of Vim. With the recent improvements to Vim and VimL, such as [asynchronous process control](https://vimhelp.appspot.com/channel.txt.html) and [useful types](https://vimhelp.appspot.com/version7.txt.html#new-7), the plugin ecosystem is thriving. A new plugin site, [VimAwesome](https://vimawesome.com/), shows the most popular plugins as well, lets your browse and search, and has well-formatted documentation and install instructions using the myriad of plugin managers that now exist.

My opinion on plugins is thus: If a plugin provides useful functionality that I wish were built into Vim, it's worth installing. Otherwise, I try to keep the number of plugins at a minimum to avoid interaction problems and maintain crisp performance when starting Vim and viewing files. Thus, the plugins and configuration I list here are more about efficiency and getting stuff done instead of difference for the sake of difference.

## fzf

TextMate and Sublime Text taught us that the fastest way to go from firing neurons to viewing a file is by _fuzzy finding_, or rather, typing part of a filename or path to open a file, even if the characters aren't adjacent or you making a spelling error. This is such a useful technique that most editors now implement it. For a long time the best solution for Vim was the Ctrl-P plugin, but a new tool, FZF, is much faster, and thus moreforgiving.

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

XXX Faster and shows underscores

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

[gitgutter](XXX) is a plugin that shows you the current git change status on a line-wise basis, like most editors do nowadays. I modified the defaults, which use distracting `+` and `-` characters, to simply show me a colored dot (`·`) for each line change, which I think looks a lot cleaner.

[vim-fugitive](XXX) is currently the single most popular Git plugin for Vim and has lots of capability. I rarely use anything other than `:Gblame`, but it's got a lot of other nice things you'd expect from in-editor Git tools.

[git-rhubarb](XXX) provides support for GitHub, and most importantly, a `:Gbrowse` command which opens the current file with optional line selection in the browser on GitHub. This is extremely useful because GitHub now inlines links to commits and line numbers are snippets from within issues and pull requests. So all you have to is select a few lines with <kbd>Shift-v</kbd>, run `:Gbrowse`, copy the URL that opens, and paste it into a GitHub comment to get something like this:

XXX pic of inline code snippet in GitHub issue.

[rootignore](XXX) deserves a metion because it populates your `wildignore` setting with the contents of `.gitignore`, among other things. This is useful for the many things that use the `wildignore` setting, such as [NERDTree](XXX) file listings, Ctrl-P, and Vim itself when choosing what files to edit.

## buffers not tabs - bufkill, eunuch

I'm a staunch user of buffers, and I've never found the use of tabs. All tabs do is create an additional way of hiding context from visibility and it requires you to memorize another keybinding or command to get at them. If you're using tmux, it's a lot easier to open Vim in another pane. And if you're making good use of buffers, it's easy to get at the file you're thinking of with a few keystrokes using FZF as described above.

If you don't really use buffers, it's easy to understand: Once you start Vim, any file you open or create becomes a named buffer. You can view them using the `:buffers` command, and navigate to one of them using `:buf <name>`, where `<name>` is any part of the filename of the buffer. They also have numbers, which `:buffers` shows you, and you can refer to them that way. If you start Vim from the command line with multiple files as command line arguments, each file will already be open in a buffer for easy access. If you've installed [vim-unimpaired](XXX), you can use the <kbd>[</kbd><kbd>b</kbd> and <kbd>]</kbd><kbd>b</kbd> keybindings to navigate between them easily. As I mentioend above, I've sped this process up considerably by binding the <kbd>;</kbd> key to the FZF `:Buffers` command so that a single keystroke brings up a buffer list with fuzzy-finding. A few more keystrokes, then <kbd>Enter</kbd>, and I'm there.

Sometimes you add a buffer and want to get rid of it. Maybe you opened a similarly named test file instead of the main class, or maybe you hit `:e foo` and then <kbd>Enter</kbd> too quickly and created a new bfufer iwith a file that doesn't really exist. In this case, the `:bd` command (short for `:bdelete` or _buffer delete_) can be used to delete the buffer and remove it from the list. 

However, `:bd` will close the Vim split if you use it on the currenttly visible buffer, so a great plugin is [bufkill](XXX), which lets you kill buffers and keep the split open, reverting to the previously edited buffer in its place. This is so useful that I bound it to <kbd>Meta-w</kbd> so I have a single-keystroke way of deleting a buffer from my list of open buffers.

Speaking of buffer management, sometimes you need to do something external to a file, such as rename it or chmod it, but the open buffer in Vim gets out of sync. NERDTree helps with this by letting you navigate to the current open file using `:CD` (XXXX true?), highlighting it, hitting <kbd>m</kbd> to modify it, and then choosing an action like rename. However, a much more convenitnet plugin is [vim-eunuch](XXX), which gives you a host of commands to modify files but keep the buffers in sync: `:Chmod` chmods the current file, `:Rename` renames the file in its parent direcotry, `:Move` moves it to a new path completely. There are a [few more](XXX) commands but those are the most useful to me.

## polyglot - vim-markdown

A plugin that a [friend](XXX) turned me onto is [vim-polyglot](XXX), which singlehandedly replaces over 150 (XXX verify) different language plugins by installing them as a single plugin. This is a nice convenience, especially since the author of the plugin keeps its dependencies up to date. The plugins chosen when there are multipel to choose from, such as the case with JavaScript, seems to be the most popular and widely-accepted choice.

The only problem with vim-polyglot so far is a problem inherant to Vim's loose plugin architecture: ordering. I already found [one bad interaction](XXX) when trying to use the [vim-css-jsx](XXX) plugin, and my solution was to [reanme the plugin when installing it](XXX) so that it loads before vim-polyglot. I'm not proud of that solution, but so far it appears to be the only plugin ordering problem I've encountered.

## other plugins: commentary, endwise+closetag, fugtivie, repeat, sleuth

Commenting out code is a common activity, so it makes sense to use a plugin that is smart enough to comment lines or blocsk of code in multipel lanaguges. Usually you can get away with something like `:s/^/#` if you're in a language that uses hashes to comment out lines, but if you use the [vim-commentary](XXX) plugin, commenting is a simple <kbd>g</kbd><kbd>c</kbd> away, regardless of language. The same key combination also uncomments the selected lines if they're already commented. Of course, if some lines are commented and others aren't, it doesn't work the smartes, but that's hard logic to figure out.

ANother common activity is inserting end-block statements, like `end` if you're writing Ruby, or `fi` if you're writing shell, or `endfunction` if you're hacking on VimL. I've been mostly content by doing this by hand for over a decade, but the [endwise](XXX) plugin has really turned me on to the auto-insertion of these keywords. I already use the bastardly-essential [closetag](XXX) plugin to complete HTML and XML tags in any kind of file, so using endwise felt like a natuarl extenstion.

Many of us already use [vim-surround](XXX), which adds keybindings to add, remove, and change the surrounding characters of any bit of text, such as changing single quotes to double quotes or brackets to parentheses. Unfortunately, by default, the <kbd>.</kbd> key doesn't repeate these substitiutions, but the [repeat](XXX) plugin makes that work. Need to change quotes in _multiple_ strings? Do the <kbd>c</kbd><kbd>S</kbd><kbd>'</kbd><kbd>"</kbd> or similar combination once, then use the familiar <kbd>.</kbd> key to repeate the substitiution anywhere else.

In the original post I mentioned my [tab and space fixing macros](XXX) which are handy when dealing with codebases that aren't your own and use differnent settings for tabs and spaces. What's really useful, however, is the [vim-sleuth](XXX) plugin, which automatically scans files when they're opened and tries to figure out the indentation settings used. It works 90% of the time, and pretyt much removes the most uses of the macros I set up. I especially don't need to insert [Vim modelines](XXX) at the tops of files anymore.

## about editors - vs code / intellij / vim + incsearch from emacs

It's worth talking about editors for a moment. Yes, I'm fully aware of the holy war nature of these arguments, but I feel that this decade we can at least agree on the right tool for the right job. We've never had such a swath of tools to get anything done -- editors, languages, frameworks, serializaztion formats -- so I think we're all a bit better about respecting other choices. (Is he kidding? That's an excercise for the reader.)

I've encountered a few handfuls of people over the years that are getting into programming, and inveitably they ask which text editor to use for writing code. My response is easy and immeidate: Just use Sublime Text. It's a great editor, it's got a great plugin community with up-to-date syntax highlighting, and it works well on macOS, Windows, and Linux.

XXX pic of sublime text

Why is Sublime Text my first recommendation? Because learning Vim just adds extra overhead. If you're learning programming, a pedantic and brain-abraisve trade by default, learning the strange and seemiingly-arcane cvombination _just to enter and edit text on the screen_ can be maddening. Sublime Text words like word processors for normal humans and is instantly useable.

I have a few more caveates, however. For Java, the leader is probably IntelliJ IDEA. Ten years ago it required a commercial license, but the free [Community Edition](XXX) is now available to anybody, and has the features a modern Java or Kotlin developer wants and needs, like Maven build support and Git integration. The real reasons to use IntelliJ with these languages is its amazing refactoring support, intelligent completion, [function signature annotations](XXX), smart indexing and searching (yes, way better than ctags), and its interactive debugger. In fact, when writing Ruby with JRuby, if I need to debug anything more than a simple `puts` will give me, I fire up IntelliJ and use the debugger. Besides, the free [IdeaVIM](XXX) plugin gives you Vim keybindings if you want them, and it works reasonably well.

XXX pic of intelli-J

Finally, [NeoVim](XXX) has caught my attention, but I'm far from making the switch. The big thing it advertised when it first came out was asynchronous function support, which has since been added in Vim 8. Other than that, I don't see a real advantage of using it over vanilla Vim, but I'm interested in seeing where it goes and who ends up using it.

## concluseion

I hope this has been useful to you. Vim has been a major part of my prgoramming life, and it's probably responsible for the successful partrs of my career -- maybe even the unsuccessful parts where I wasted too much time.

I'm well past 30 years old now, which makes me about 150 in programmer years, and my focus now is on _getting stuff done_ instead of mucking about with my tools. But Vim is one of those tools where doing a little work and a little research, such as browsing [VimAwesome.com](XXX) or reading a few lines in a help page, can dramatically improve your effectiveness. In the most eggregious cases, when your brain needs a break from programming, it's usually pretty easy to think, "Hey, what's one thing that's really bugged me in Vim while I was doing X," and fine a solution for it quicikly.

I hope that this has been useful. Also check out my earlier posts, [Vim After 11 Years](XXX) and [Everything I Missed in _Vim After 11 Yeras_](XXX). Let me know what you think in the comments.

