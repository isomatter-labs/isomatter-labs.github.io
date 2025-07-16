---
title: Editors and IDEs
author: M Cooper Healy
date: 15 Jul 2025
---

# Introduction: The Pondering

I've been doing my traditional semi-annual pondering of my tooling, and
I've noticed that I'm going round and round on the same subjects:

1. IDEs are a Good Idea, at their core
2. Text Editors are a Better Solution to editing
3. Language tooling ≫ third-party tooling
4. The mouse is not the problem (menus are)

## IDEs are a Good Idea

This thought mostly comes from thinking about Smalltalk[^1], and how
much it got right. The idea of an Integrated Development Environment
originally didn't mean a monolithic application you pointed at a folder
containing your codebase that could perform some static analysis on it
and thereby paint everything pretty colors. The Integrated Development
Environment, as was invented by Smalltalk-72, was a running image of the
application/system that you were working on, which happened to also
itself contain tools for inspecting and modifying that image/code.

This was revolutionary, and as a side effect invented much of modern
development tooling, not least among them the debugger. Working _inside_
the system that you are working _on_ gives you an incredible amount of
introspection and perspective, and also allows things like testing to
become second nature. It is like being in a woodshop, where all of your
tools and jigs surround you on the workbench, your project neatly
nestled among them. If you need to be able to manipulate that project in
a new way, it is seemless to either grab, assemble, or hack together a
tool or jig to allow you to do just that. There is a kind of focus,
where the rest of the computer is completely irrelevant, and you are in
your own zen garden.

Modern IDEs, however, do not work like this. Firing up Eclipse is not
stepping into some Java image where you can reach out and twiddle with
your project at will, constructing your own tools as you go. Modern IDEs
work like a pickup truck laden with power tools, driven to a worksite.
Additionally, the workbench is bolted to the floor of the bed of the
truck, and all of the tools are bolted to that workbench. There are no
woodscraps lying around to make your own tools and jigs, only what you
bring with you. While this gives you access to a very powerful set of
tools, and a very nicely crafted workbench, it feels almost like lying
to refer to it as an \"environment\". That having been said, the
philosophy has some merit even in the real world, where Workshops cannot
exist in most production codebases (for reasons I'll touch on later).

### A Real Try™ With an IDE

I recently activated a 30-day Trial of JetBrains' WebStorm, and have
been using it to work on the two main codebases at my current job. While
initially balking at the startup time and the complexity of getting
everything set up, I've been largely impressed. WebStorm makes a
genuine effort to know _everything_ about how your code and how it
works, and attempts to give you insights at every level. The tooltips,
suggestions, and refactoring support are top notch, and I can tell that
under the hood, a not-insignificant amount of memory is being used to
map out exactly how everything works, so that I can edit such a large
codebase with confidence that I can know _exactly_ where the code I
touch is being used, and how it's used, and when it went there. The
experience is surprisingly polished, and I think it will become my
primary editor for large TypeScript codebases in the future.

### Why We Can't Have _Real_ IDEs

At this point, it is natural to wonder \"what keeps us from having the
IDEs of Smalltalk and InterLisp, where the code is the editor is the
workshop is the code? There is one, very large, very glaring issue: you
can't ship the whole workshop. While having a magic workshop in which
you can build anything, and the tools you use are built out of the same
stuff as the project you're working on is magical and empowering, it
requires that one of two things happen:

1.  You spend a bunch of time \"trimming for release,\" wherein you
    remove everything from your workshop that _isn't_ part of the
    project (which notably includes removing the workshop itself)
2.  You ship the entire workshop to your customer, and hope they don't
    mind that it uses a Gorillian GB of memory and includes it's own
    (secret)[^2] editing suite

Neither of these work in any environment that includes multiple
releases, or continuous development, because you either need to make (1)
a simple and repeatable enough process that it does not impede the
deploy cycle, and does not cause conflicts with other \"pruning runs\",
or you need to ship an application with system requirements that would
make most user's head spin. There is some leeway in the latter, as
Electron apps have shown us, but users' patience for even that has run
thin[^3].

## Text Editors are a Better Solution

Text editors attempts to fix the \"you can't ship the entire Workshop\"
problem by asserting that the Operating System is your workshop, and
that all you really need is a well designed bench. This is (surprisingly
for some) a shockingly compatible philosophy with the original Smalltalk
system, once you realize that the editor in question is analagous to the
\"System Browser\", not the whole of Smalltalk.

It is at this point that I would like to point out that there are, in
fact, very few text editors. \"Nuh-uh!\" I hear you cry, \"Most modern
editors aren't IDEs!\"

This is largely a side effect of my definitions of editors and IDEs, and
how they differ from more common usage (where IDE exclusively refers to
applications that ship with as many batteries included as possible).
Most applications referred to as \"editors\" are, in fact, particularly
emaceated IDEs (I say this with love, as I use many of them). Emacs is
an IDE, or attempts to be, asking that you plug as many tools as you can
into it so that it becomes your perfect creation studio. NeoVim gently
requests that you install a litany of plug-ins that beef up your
experience to rival that of other IDEs, but with you in control[^4].
VSCode is an IDE that just installs fewer things out of the box.

As far as \"True\" editors are concerned, I know of few, and none are
particularly popular:

1.  Sam (from Plan 9)
2.  Acme (also from Plan 9)
3.  Notepad (with a possible extension to include Notepad++)

This list is not necessarily exhaustive, but is meant to illustrate a
point: it is very rare that you get an editor that chooses to do almost
nothing, leaving _all_ code-manipulation to external tools. These tools
are very rarely used, and notably their usage tends to break along
programming language lines.

## The Importance of Language Tooling

What kind of editors you can (or can't) use is often determined by what
language your working in, and how good the CLI tooling for that language
is. For example, Go (and most languages developed alongside or after it)
includes _excellent_ command line tooling for formatting, linting,
compiling, testing, performance profiling, etc. Because of this, it's
very easy to use an editor that simply sends the contents of it's text
buffer to the specified tool, and nothing more, acting almost like a
graphical shell.

For languages like TypeScript, Ruby, Python, etc, tools exist, but are
often third-party and have complicated integration in order to get an
entire workflow put together (this is the group for which I posit that
an IDE is necessary, as stronger glue than piping commands together is
needed).

## The Mouse Is Not The Problem

There is a tendency, especially among computer power-users, to eschew
the mouse entirely, treating it as the source of all inefficiency. There
have been studies, both valid and invalid, that have shown that the
keyboard both is and is not faster than the mouse. In all honesty, i'm
not interested in the argument.

In terms of _cognitive load_, the mouse is _almost_ always the shortest
path between two points. This comes from the fact that the computer
monitor is a 2D plane, and the mouse is the only peripheral connected to
a computer that gives a 2D input.

Hotkeys have their place, and I will not for one second act like the
muscle memory associated with hotkeys on common operations isn't a time
saver. I don't think there's a computer user alive who doesn't
reflexively slam the `save`{.verbatim} hotkey everytime they finish a
thought, be it in code or prose.

For selection, however, and for passive scrolling, I find it hard to
approach anywhere _near_ the ergonomics of mouse-use. I will note,
however, that choosing a good mouse is _very_ important for this, as a
bad mouse will do more to cause RSI than a sub-par keyboard ever will,
despite the latter being the rallying cry of many a techbro with
disposable income[^5].

Instead, I posit that the inefficiency being so vehemetly avoided comes
not from mouse use itself, but the atrophy of mouse-menus into a
multi-layered, hierarchical mess of options, often with no fast or
keyboard-driven equivalent. Menus can often require you to hover through
two or more \"tiers\" before you reach the option you want. In contrast,
the menu of Plan 9's `rio`{.verbatim} window manager is a breath of
fresh air.

Rio, in contrast to other mouse based systems, solved the problem in the
same way as Smalltalk. There are different menus for each of the
three[^6] mouse buttons, and each menu has only a top level. The left
mouse button is used for selection, and selection alone. The middle
mouse button is used for text-manipulation actions, such as cut, copy,
paste, and sending the selection to the plumber (read: command line,
more or less). The right mouse button is used for top-level operations,
like creating, moving, resizing, and destroying windows. This top level
action is contextual, and can be overriden by different behavior in
different programs. For example, in both the Sam and Acme editors, it
executes a \"find\" of sorts, where the cursor (and the current
selection) move to the next occurance of the selection.

While this alternate mouse system takes some getting used to, it is not
long before the user has transformed their mouse from a simple
point-and-select tool to something as versatile as the keyboard, and
just as nuanced.

The difference here is that the mouse is treated as the tool of a power
user, and the user is empowered to use it in ways that give them more
ability, rather than as a proxy for jamming a finger against the screen,
as they are often used now. It is my belief that most of the frustration
that power users experience with the mouse has almost nothing to do with
the mouse itself, and much more to do with it's criminal
under-utilization in most modern systems.

# A Proposal: The Best of Both Worlds

Many articles that cover the same material as this one end with waxing
poetic about computing systems gone by, and missed opportunities, with a
teary eyed plea to the tech-powers that be to bring back the magic. I
have no interest in following suit.

This whole rant would be pointless without some kind of actionable
suggestion, so I'll present my own workaround to attempt to learn from
the past without being stuck in it.

## Two Editors to Rule Them All

It is my position that the best way to address the holes in modern
tooling is simply to use more of them. As somewhat of a tool junky
myself, I am often tempted to just use every possible tool, but the
context switching required to maintain that is... inefficient to say the
least.

Instead, I believe that one should have a go-to tool for each category:

- One IDE
- One Editor

For me, this IDE has historically been Emacs (although WebStorm/the
wider JetBrains ecosystem is becoming a likely alternative), and the
editor has been something from the Vi family (Vi, vanilla Vim, vanilla
NeoVim, Helix, etc). Other than recently playing with using Plan 9's
Sam as my \"editor\", and working on building my own replacement[^7],
this has worked fairly reliably for me for 7 years now.

I do not posit that my tool choices are optimal, simply that including a
tool from each category is. The most common split I see in the developer
community right now is Plugin-heavy NeoVim as the IDE, and vanilla
Vi-like as the editor, which seems like a pretty good set of choices to
me.

## Mouse well-supported, but not required

It is important that _both_ of these tools support the mouse, ideally as
ergonomically as possible. In leiu of that, the tooling should at the
very least:

1.  Be able to click to snap cursor to point in _any_ program to intend
    to use seriously
2.  Have keyboard-driven options for anything you can do by clicking
    menus

Just with these two rules, we ensure that the reflexive
grab-mouse-to-jump-cursor-across-screen will always work, and inversely
that anything that you are used to doing with a mouse can be eventually
moved into hotkey muscle-memory. These requirements may make it seem
like the bar is practically on the floor, but that essentially because
it is.

Don't overthink it, it isn't that important.

# Conclusion

There isn't a super tidy way to wrap this up, but it's worth touching
on what I consider to be the Main point here: find tools that work for
you. This is a pretty common refrain for me, and I've written it before
in other articles, but it's worth repeating. No amount of optimal tool
choice or use is going to give you magic productivity gains, or make
your life suddenly easier. Just use the tools that work for you, and
that you enjoy using, and don't worry about it too much.

In that vein, it's always worth it to shop around and try new things,
and it's _definitely_ always worth it to look to the past for
inspiration; sometimes you find things you never knew you wanted.

[^1]:
    Squeak(<https://squeak.org>) is my favorite Smalltalk
    implementation at the moment, but Pharo(<https://pharo.org>) is
    often cited as more modern/cutting edge.

[^2]: Assuming that you dont _want_ to ship users an editing suite.
[^3]:
    Notably, Electron apps and/or web apps are the closest thing to
    this mythical IDE that still exists, but exist solely on the virtue
    that everyone has already bought a Workshop and installed it into
    their house, and that all Workshops of this sort are mostly
    compatible.

[^4]:
    This is both admirable and the reason TJ DeVries\[fn:tj\] refers
    to NeoVim as a Programmable Development Environment, rather than an
    editor.

[^5]:
    I myself use mostly trackballs, which, after an adjustment period
    of a week or so, give the tactile feedback of a good mouse, as well
    as the freedom of placement and position of a trackpad. In my
    opinion, a good trackball is the absolute optimum for
    pointer-movement on a computer.

[^6]:
    Yes, three. The middle mouse button is often criminally underused
    outside of CAD applications, or simply as a passthrough to scrolling
    behavior.

[^7]:
    It's called Bugs, and it's (mostly) a port of sam in StreetLISP.
    You almost definitely do not want it. Please do not use it.
