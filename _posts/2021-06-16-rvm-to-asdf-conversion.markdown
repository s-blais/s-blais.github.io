---
layout: post
title:      "RVM to ASDF conversion"
date:       2021-06-03 14:00:00 +0000
permalink:  rvm-to-asdf-conversion
---

Over a year ago, when I started my full-stack bootcamp, I diligently followed the school's instructions for preparing my Mac for my developer journey. As I'm sure is the case with many new coding students, I had only a passing understanding of what I was doing with each step of the preparations. 

Recently, upon encouragement from a couple developers I know through a networking group, I decided to convert my Ruby installation management from the bootcamp-sanctioned [RVM](https://rvm.io/) to the more diversified [asdf](https://asdf-vm.com/). 

Soon after making this decision, I realized that I needed to shore up my grasp of how Ruby and RVM exist and function on my Mac. I can understand why the bootcamp setup didn't go into the gory details, however, at the point of trying to figure out what needs to happen for the conversion, I really wish it had!

This post will present some of the things I wish I'd known – things that would have not only made troubleshooting a little easier at several points during my bootcamp, but also would have helped me accomplish the switch from RVM to asdf much faster.

## Your Mac ships with Ruby – but you don't want to use it

macOS ships with Ruby (and some other programming languages) installed. That Ruby isn't there for you to program with, even though you could. It's there to support "legacy software" that relies on Ruby. This Ruby lives at /usr/bin/ruby, and generally requires `sudo` to do anything to it (like install gems). Of course, the rule of thumb about `sudo` stands – unless you really know what you're doing, you don't want to `sudo`. Before you've installed any other Rubies with a utility like RVM or RBenv or asdf, this is the Ruby your Mac uses. The OS even indirectly suggests you stay away from it; if you drop into irb with this system Ruby, you get this message:

> WARNING: This version of ruby is included in macOS for compatibility with legacy software. In future versions of macOS the ruby runtime will not be available by default, and may require you to install an additional package.

Additionally, this system Ruby is "old;" on Big Sur, it's version 2.6.3, when Ruby is currently available up to 2.7.3 and 3.0.1.

## What RVM does

RVM creates an .rvm folder into your home folder (`~/.rvm/`) where it stores the different versions of Ruby that you install (at `~/.rvm/rubies`), and the gems you've installed for those Rubies. 

RVM also modifies other "dot" files in your home folder, so that any terminal sessions that you open in various applications will automatically know that they want to use an RVM Ruby, rather than the system Ruby. These files may include `.bashrc`, `.bash_profile`, `.profile`, `.zshrc`, and `.zlogin` (this is one aspect of RVM that seems to get a lot of side-eye from its detractors, usually accompanied by the term "invasive")

Another place where RVM adds some stuff is in any folder where a specific Ruby version has been specified for use. Suppose you have projects on your system that use different versions of Ruby. RVM "remembers" the version of Ruby for a folder via a .ruby-version file in each folder, so any time a shell session is active in that folder (or any of its subfolders), it knows which version of Ruby to use. This .ruby-version file can remain useful even after switching to asdf (discussed later). If RVM can't determine which version of Ruby to use from the current shell session location, it will bubble up through the file structure until it finds a `.ruby-version`.

One aspect of the process which was not clear to me, and for which I couldn't find any guidance anywhere, was when to remove RVM, and when to install asdf. Remove first, then install? Install, then remove? Should I save or repurpose anything?

My experience suggests the following:

## Buh-bye RVM

1. Back your shit up.

2. Make a note of the versions of Ruby you have installed in `~/.rvm/rubies`. Will you need to reinstall all of them? That's for you to figure out based on your knowledge of which versions you've used and if you think you'll need them again anytime soon. I did read one account that said Ruby version folders can be repurposed later in the (not yet present) `~/.asdf/installs/ruby` folder with some name changes, but I chose to reinstall my Ruby versions from fresh downloads after the switch.

3. You could also make note of the versions of gems like Rails that each of your Ruby installs utilize; this *could* make things a little smoother later, but I'm on the fence about this. If you're anything like me, you could have a slew of different Rails gems on each installed Ruby version, thanks to countless bootcamp labs that had wildly varying Gemfiles. I decided to merely make note of the Ruby/Rails version pairings of my "big" projects. 

4. If you're feeling squirrely about any of this as you prepare to make big changes, you can copy your `~/.rvm` folder to an external drive. 

5. Close any and all terminal sessions. Reopen one for your home directory.

6. `rvm implode` will delete your `~/.rvm` folder (which includes all versions of Ruby that were installed installed with RVM, and all the gems that have been installed from those versions). My `~/.rvm` folder was over 2GB; thanks to the wildly inconsistent versioning implemented by my bootcamp labs, I often had a half-dozen or more different versions of any given gem for each version of Ruby. Ugh. Simply imploding RVM, however, doesn't extract the hooks it placed into the environment. Basically, the user environment has to "unlearn" that it ever used RVM. Because this was a user-level RVM implementation, all the necessary file modifications happen at the user directory level (if you're advanced enough to have known how to implement RVM at the system level, for all users, you probably aren't reading this blog post anyway!).

7. Find and comment out all lines that reference RVM in any of the following files you find in your home folder: `.bashrc`, `.bash_profile`, `.profile`, `.zshrc`, and `.zlogin`. Yes, you could delete the lines entirely, but my preference is to leave a historical trail. I even added an extra comment line explaining why the lines are commented out. Note that I have seen scripts that appear do this step, but I preferred doing it manually, as it helped me understand how RVM worked (a bit after the fact, but whatever).

8. Close the terminal session and open a new one. At this time, the only Ruby remaining should be the system Ruby described earlier. If you `ruby -v` and `which ruby`, you should be shown the system default Ruby in `usr/bin`. If you drop into irb, you will likely see the legacy-compatibility warning, also described earlier.

## Implementing asdf

1. Follow the instructions [here](https://asdf-vm.com/#/core-manage-asdf) to get asdf installed via your preferred method. I homebrewed mine!

2. Add an `.asdfrc` file to your home folder with a `legacy_version_file = yes` entry so asdf knows to look for, and honor, any version-setting files that RVM (or other versions managers) implemented. (If you don't do this, asdf won't bother switching to your previously-chosen Ruby version for the location you are in your file structure, until you manually set the Ruby version for the location, which results in asdf setting a `.tool-versions` file) More info [here](https://asdf-vm.com/#/core-configuration?id=homeasdfrc). Similar to RVM, asdf will look in your current shell session location for a .asdfrc or .tool-versions file, or bubble up until it finds one.

3. Add the Ruby plugin for asdf: `asdf plugin add ruby`

4. Use asdf to install needed Ruby version(s) (from the handy list created earlier): `asdf install ruby x.x.x`

5. Confirm newly installed Ruby versions and asdf's switching abilities with `asdf list ruby` and by navigating a shell session around to different project folders (assuming they used different versions in the first place), then `ruby -v` and/or `which ruby` (the latter will say ~/.asdf/shims/ruby, which is where your Rubies live now). 

6. Pair up specific gem versions (like Rails or Bundler) to your newly installed Ruby versions by first setting/confirming your current shell Ruby version and then `gem install [gem name] -v x.x.x`. For example, to get the Rails 6.0.3.7 gem installed for Ruby 2.6.1, first `asdf shell ruby 2.6.1` (this temporarily sets the Ruby version for this shell session only), confirm with `ruby -v`, then `gem install rails -v 6.0.3.7`.

7. For some satisfying "I see what you did there" validation, snoop around your installed Rubies and their respective installed gems by navigating to, for example, `~/.asdf/installs/ruby/3.0.1/lib/ruby/gems/3.0.0/gems/

## What's next

I think I'm going to retire NVM and use [asdf's Node.js plugin](https://github.com/asdf-vm/asdf-nodejs) instead!