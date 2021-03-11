# Introduction



Hello and welcome! If you've been linked here, it probably means you're interested in Fire Emblem GBA ROMhacking. This tutorial in particular is about **using a Buildfile** to make modifications to **FE8**.

You can also visit [Fire Emblem Universe](https://feuniverse.us/) for more resources and information.

## Why use a Buildfile?

You may be familiar with other methods of FE hacking, such as **FEBuilder**, **FEditor**, or **Nightmare**. These methods all use GUIs, which can seem more intuitive. So what's the big deal? Well, using a Buildfile allows you to avoid having a "working ROM". This means you can **never brick your ROM**, and you can **never lose progress** from making a mistake, or have to comb through your ROM looking for whatever it is you broke. You **don't have to keep a million backups** of your work, and it also allows for **collaborative editing** without having to pass a ROM around several people. Forgoing an editor software also gives you **complete control** of your actions, instead of leaving it to the software's mercy. In short, while requiring more effort to grasp, it will grant you **ultimate power over your ROM!**

## What exactly is a Buildfile?

A Buildfile is a set of event \(or text\) files that give instructions on how to assemble changes to a ROM. Now, those are some confusing words that might not mean anything to you, so let me break it down further. When using traditional editing software, you make changes **in place**. That is, you change some data, hit save, and now your old file is your new file. If my ROM were a book, it'd be the equivalent of slapping on some white-out and putting in my own writing overtop. As I said earlier, if I were to make a mistake somewhere along the way, I'd need to flip back to that page and do that again to put it back to the way it was. A Buildfile is more like editing a word document. When I need it, I print it out. If I make a mistake, instead of getting out the old white out, I change the text on my computer and print a new copy. **Every time you make changes, you will be making a fresh "copy", or build, of your work.**

## Why use FE8?

Those more familiar with the older hacking scene may be familiar with FE7 being the go-to version for hacking. With recent developments, FE8 is now far and away **the most supported version** to build on. With a bit of tinkering, **it can do everything FE7 could and more**. Since FE8 was the last GBAFE game, it also has **the most advanced native features**, including a **much more powerful event engine.**

{% hint style="info" %}
I will note that FE6 and FE7 are supported by Event Assembler and you can use a Buildfile to edit them if you so choose - though this guide will not make addendums for any differences.
{% endhint %}

