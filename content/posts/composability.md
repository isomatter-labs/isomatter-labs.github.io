---
title: Same Bean, Different Burrito
subtitle: It's all just composability, do what you want
author: M Cooper Healy
date: 10 Feb 2024
---

# The Editor(s) of a Lifetime

I first started using Emacs in 2010, as a 12 year old. At the time, my
reasoning was simple: I didn't want to figure out Vim modes. I had just
gotten into Linux for the first time, and was overwhelmed with excitement.

I learned the basic movement commands, and I learned to save and open files,
but other than that, I mostly used it like Microsoft Notepad.

Fast-forward to college, and Sublime, Atom, and later VSCode were all the rage.
I, like essentially all of my peers, picked a favorite to cling to
(I was an Atom fanboy, but that was mostly for the aesthetic of the marketing,
if I'm being honest with myself). Around this time, though, my CS department
was slowly starting to divide -- Vimmers on one side, Non-Vimmers on the other.

It wasn't so serious as that, but I fell prey to FOMO, and dived in.
Movement came quickly, and, armed with a config that fit in about 100 lines
of Vimscript, I was living the POSIX dream. Having an editor that only
edited encouraged me to live in the terminal, finding tools to fit my problems
and combining them into solutions, rather than searching APM for a package
to solve my problem.

Vim made me think about computing differently, and I was all the better for it.

At the time, mostly writing code for school projects in C, C++, and Haskell,
I had little need for serious linting or tooling beyond what was included
in the `g++` cli or `stack`'s tooling.

Then VSCode became the most popular editor in the world, and I got excited
about the modern features and pretty interface -- and used Atom instead
because Teletype was cool and the marketing made me happy.

Then I went to work, and my mentor used Emacs.

I had begun with Emacs, and had been using `readline` keybindings in the
terminal for years, so it didn't take me long to get back up to speed.

This time, however, I was determined to do it right.
I spent hours on my config -- writing and re-writing it.
I avoided distrobutions, and was determined to `elisp` my way to mastery.
I read Mikey Petersen's book _Mastering Emacs_, and I used it to dive deep
into Emacs' workings.
I made it pretty, creating my own theme, and my own
mode-line package.
I made a literate config, and documented everything I added.
I even published it on a [website](https://starmacs.isomatter-labs.com),
and got a few stars on GitHub.

I was in deep.

I was productive.

I was in love.

# Emacs, an Ode

I fell in love with Emacs when it really _clicked_ with me what it was.
I've heard countless of its proponents go on ad nauseum about how configurable
it is, how customizable it is.
The thing is, that in itself isn't unique.

I knew firsthand that I could achieve essentially any practical level of
customization with Vim, Atom, VSCode, what have you.

The beaty of Emacs is hidden in an oft-repeated joke:

> Emacs is a wonderful operating system, lacking only a decent editor.

Emacs is a Lisp Machine.

Emacs is a living, breathing workspace, where you can look at everything,
touch everything,
manipulate everything -- all without needing to reboot or re-source a config.

`M-:`, the keymap for  `eval-expression`, became my favorite command overnight.
The ability to open an `elisp` repl and just _do stuff_ was an intoxicating
level of power. It was like seeing into the matrix.

Most editors have something like a command palette:

- VSCode has its `Ctrl-Shift-P` "Command Pallette"
- Vim/Neovim has its `:` "Command Mode"
- Emacs has its `M-x` `execute-extended-command` (its a command palette for interactive functions)

But of those, only Emacs has `M-:`.

It's a way to slip past the defined user-API for the editor and manipulate the
internals directly. To really _live_ in editor like a craftsman in their
workshop.

I could see why so many emacsen before me had slowly pulled everything into
Emacs -- mail, web browsers, organization, etc. With the ability to just reach
into the internals and create whatever you wanted in real time, it was like
playing in a sandbox. Once you had exactly what you wanted, all you had to do
was save the scratch buffer you were using for `elisp` evaluation into a file,
and add it to your config. Easy peasy.

Emacs was my OS, and I had learned how to really _use_ it.

# Doubts Creep In

So I'd created my own perfect workshop in Emacs, custom built from scratch.
I had assembled it line-by-line for around 5 years until it felt like my
true home on the computer.

In the back of my mind, a thought:

> Am I... _using_ any of this power?

I was never one to put mail in Emacs,
or forum reading,
or web browsing,
or window management.
Not because I had a philosophical problem with it, I just always held out
that I wanted to use my operating system. Emacs was my IDE, not my computer[^1].

Because of that line, however, I wasn't really using Emacs the way it was
meant to be used.

I wasn't living in my "Lisp Machine" -- I never had been.

I was only experiencing that phenomenal introspection when I was updating my
config. It wasn't part of my workflow, it wasn't part of my tooling.

Emacs was becoming a tool that I only _really_ used well when working on...
Emacs.

The only exception was when I was programming in Lisp, but in that instance
SLIME was doing all of the work. The debugging and introspection was a part
of SBCL, not of Emacs.

I started to realize that as much as I _loved_ the lisp machine mentality in
theory, the actual application was never quite what I wanted.

# Composability is Composability

For all the work I'd put in to make Emacs a living ecosystem with packages
I could use together to accomplish tasks, I'd forgotten that I'd already had
a way to do that before.

When I was in college, living the POSIX dream.

Moreover, by relying on a bunch of different command-line tools, rather than
Emacs packages, it was much easier to onboard coworkers to a workflow, since
it wasn't Emacs-dependent.

Sure, the Emacs packages did a heroic job of allowing Emacs to take the place
of essentially every other tool I'd ever used, but it didn't do _any_ of those
things better than the dedicated tools I'd left behind.

I felt like I was using a leatherman multitool instead of a woodworking shop,
and I was tired of trying to use the dinky little saw on 2x4s.

There were only really two advantages to doing everything in Emacs:

1. I got to use Emacs keybindings for everything, making my experince consistent everywhere
2. My config auto-installed every tool I used, so I didn't have to remember what every tool in my toolbox was.

For point 1, I was solving an already solved problem -- I use a Mac for work,
and MacOS already has GNU `readline` keybindings baked into essentially
everything.

As for the second point, I could spend just as much labor to use any other
config management system (Nix comes to mind) to manage individual tools, so
this was more sunk-cost fallacy than anything.

# Freedom

I decided to venture out, and to start anew with Neovim.

It was shockingly easy.

I still had the muscle memory for the movements, to my surprise.
Packages already existed for _everything_ I wanted Neovim to do.
The community was larger, and was very excited about posting their own conigs,
so I just shamelessly stole most of mine.

It took about a day to get to the same level of productivity as my
Emacs config[^2].

The real beauty was, I got to pick my own tools again.

For every problem I had, I could find (or make!) a simple CLI tool,
_composable_ with others, to just solve it.

I could script those solutions! I could share them!

Suddenly, a weight I didn't know I was carrying lifted.

It took all of a week to feel like I could just... stop thinking so much about
my tooling, and just do work.

Sometimes it's best to let your editor be an editor,
and your operating system, an operating system.

[^1]: I want to clarify that I can totally understand why people _do_ kitchen sink Emacs; it just isn't for me.
[^2]: This is the point where rather veteran Emacsen say
    "Well, then you weren't _really_ using Emacs!" Okay, sure, let's do the
    no-true-scotsman argument. The reality of the situation was that I had spent,
    in all honesty, more time than was reasonable in customizing and tweaking
    my Emacs workflow, going all the way from minimal to kitchen sink and back,
    with frequent stops at having my Linux boxes boot directly into it.
    I was using it constantly, and for everything, and i genuinely _loved_ it.
    Living in the alternate reality of the Lisp Machine Cinematic Universe just
    got really old after having to reinvent _every_ wheel for 5 years. Take
    this as my personal apology that I didn't understand Emacs as well as you,
    and that you are simply too wise to learn anything from this essay.
