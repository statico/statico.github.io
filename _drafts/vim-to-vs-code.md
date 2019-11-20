---
layout: post
title: "From Vim to VS Code After 17 Years"
description: Vim served as my primary text editor for almost two decades. But now, Microsoft's Visual Studio Code has progressed to being more functional and faster. I've switched and I haven't looked back. Mostly.
image: /images/vim/todo.png
---

After more than 17 years of near-daily Vim usage, I've switched to VS Code as my daily text editor. No, I can't believe it, either.

<figure>
<a href="/images/vscode-prose.png"><img src="/images/vscode-prose.png"/></a>
<figcaption>A recent VS Code session. Looks similar to Vim, eh?</figcaption>
</figure>

After more than 17 years since adopting Vim as my primary productivity tool, I'm compelled to give that title to Microsoft's Visual Studio Code development environment. This wasn't an easy decision, and I had a few false starts prior to my full-time adoption a year and a half ago. But VS Code has now progressed to the point where, if new engineers ask, "What editor should I use?" then my response will be VS Code.

TL;DR of why I've switched to VS Code: 

* Vim emulation is good enough. 🤷
* Everything just works with minimal configuration, if any.
* It's very fast, has a great UI, and is easily customizable.
* It works on macOS, Linux, and Windows.
* The ability to fuzzy-search commands and configuration options is something no person should live without.

Over the last few years I've documented how I've used Vim (and MacVim) in my day-to-day use. A lot of people seemed to find those articles extremely useful and were overjoyed to see a "modern, recent" Vim setup. I'm glad to have created helpful, educational content to make people more productive.

### How I Switched

Before committing to VS Code, I felt it necessary to customize VS Code as little as possible. I wanted to install the Vim emulation plugin, change the color scheme, hide some distractions, and be ready to work. I felt that this was important because I wanted to experience VS Code as closely as the community expects one to when using it. Surely the group of [hundreds][vscode-contributors] of developers have thought of a few crafty things, so I was prepared to retrain myself a little bit in order to get the full experience of what using a cutting-edge coding environment in 2019 is supposed to be.

### First thoughts: Appearance

After booting VS Code for the first time, it looks a little busy compared to the satisfying simplicity of a plain Vim window. But it's easy to fix this and get VS Code into showing what I think are the essentials and nothing more: file explorer, status line, tabs with open buffers, and the editor.

The first thing is to hide the silly "Activity Bar" with icons on the left, which does nothing more than waste horizontal space. Just remember that <kbd>Ctrl/Cmd-Shift-e</kbd> shows you the file explorer (with the current buffer highlighted) and <kbd>Ctrl/Cmd-b</kbd> hides the left pane completely until you do something that opens it.

Then there's the minimap. This feature somehow got popular with either TextMate or Sublime Text, but it's still a distracting waste of useful space. If you need to navigate quickly, use <kbd>Ctrl-d</kbd> or <kbd>Ctrl-u</kbd> to go up or down, respectively, or hold down <kbd>{</kbd> or <kbd>}</kbd> to scroll up and down by paragraphs. And if you don't know what the shape of the file looks like, make your files shorter.

And the most important part, themes. Most of the popular Vim themes are available for VS Code as well. This includes One, One Dark, Nord, Solarized, Monokai, and plenty more. I was already using the Nord theme for Vim so I felt right at home. However, comments seemed a little dim, but luckily the Nord theme authors added [a setting][XXX] which can adjust the contrast of comments. Perfect.

The most astounding thing about VS Code's appearance is how well space is used. After two decades of spending most of my time in monospaced, fixed-grid consoles, using an editor with a modern GUI framework is refreshing. Space can be used much more efficiently. More files can fit in the file explorer, more tabs can fit at the top of the screen, and the changes and warning indicators in the gutters of the editor are much more compact and useful.

### Vim Emulation in VS Code

It's clear that creating a Vim emulation layer isn't easy. The VS Code Vim extension projects has XXX open issues, XXX closed, and has had commits daily since XXXX. I don't envy the people who have dutifully created a working Vim emulation inside another editor, but I am thankful for them, as the Vim plugin in VS Code feels about 95% complete.

Most things work out of the box with VS Code. A few popular plugins, including some of those that I've [already installed][dotfiles-vim], are already emulated, such as [vim-surround][vim-surround] and [vim-commentary][vim-commentary]. [EasyMotion][easymotion] is emulated, too, but I could never get used to using it.

Additionally, a few favorite keybindings (e.g. mapping <kbd>j<kbd> to <kbd>g<kbd><kbd>j</kbd> and <kbd>k<kbd> to <kbd>g<kbd><kbd>k</kbd>) are easily portable in VS Code's [settings.json][dotfiles-vscode-settings]. I only had to add a few lines in my [settings.json][XXX] to fix this.


### Conclusion

I was discussing this topic on the Reactiflux Discord server, and a fellow community member decided to point out all the ways that Vim has the features I had described. "Just use `XXX-server`," I was told, "and then use `YYY` and `ZZZ` and you'll get all the best parts of VS Code type completion." So I installed those plugins, fired up Vim, and nothing worked. Because I still love Vim, I spent a good 15 minutes fully committed to _trying_ to make it work, but couldn't. This was in the middle of the day during a work day, so I had to stop and get back to work.

And, dear reader, that leads to one of my most powerful conclusions: I am being paid to get stuff done, not twiddle my text editor. Ten years ago I would have gladly spent half a day configuring Vim to work properly with all of the features I need. But I am no longer in a position where I can say, "Sorry, I didn't get that done because I was configuring my text editor." 