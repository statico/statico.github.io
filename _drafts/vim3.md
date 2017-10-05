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



## ag.vim + meta-k - quickfix + vim-unimpaired + \x + QFEnter mention

## tmux + autowrite + no gvim + Terminal.app vs iTerm, splits

## goyo + ProseMode + supertab + fzf for thesaurus?

## ale > syntastic

## lightline > airline/powerline

## fugitive + gitgutter + git-rhubarb + :Gbrowse + rootignore

## buffers not tabs - bufkill, eunuch

## polyglot - vim-markdown

## other plugins: commentary, endwise+closetag, fugtivie, repeat, sleuth

## about editors - vs code / intellij / vim + incsearch from emacs

