# GitHub Pages

##### Jun 6, 2020

I decided to give [GitHub Pages](https://pages.github.com/) a try so that I do not have to bother with hosting these posts 
elsewhere. Take-off was quite smooth but not without a few hiccups.

### GitHub 

First I hit the dreaded *"my github page doesn't show"* problem. No idea what fixed it
because according to GitHub it may take 10 minutes for the page to first come online and who has that
much time to test every single attempt. It must have been a combination of

* having an index.html file
* adding a README.md file
* picking a Jekyll theme
* waiting long enough

Curiously, after deleting README.md later, the original index.html finally showed up just fine, 
even without any`<!DOCTYPE html>`in it :o

### Jekyll

Of course, I wanted to run Jekyll [locally](https://help.github.com/en/github/working-with-github-pages/testing-your-github-pages-site-locally-with-jekyll) next. 
So I 

* cloned my GitHub Pages repo
* installed [rvm](https://rvm.io/rvm/install) (to keep everything in user dirs)
* installed bundler
* went on to install Jekyll...

but native gem compilation during Jekyll installation kept failing. 
Maybe only an issue under [WSL](https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux),
or maybe generic - cannot tell.
Once I figured the solution out, it was just a chore:

* correct`INSTALL = ./install`to`INSTALL = install`in the Makefile
* also insert`mkdir -p $(RUBYARCHDIR)`whenever`make install`fails to create the gem dir
* invoke`make`by hand to compile the gem
* perform the *"write specification by hand"* step printed by`gem help install`
* retry Jekyll installation and coast until the next error
 
Eventually you win, 
do`bundle exec jekyll serve --no-watch` and tada!, you have a server on localhost:4000.
(The flag is required due to WSL polling [limitations](https://github.com/microsoft/WSL/issues/216) and, of course, 
you also need to say OK when Windows asks whether the server can bind to the port.)

*EDIT 2020/07/09:* a recent Windows update borked my Win 10, so I had to redo everything in Linux. 
For future reference, here are the steps that worked:

* install rvm - download the [installer script](https://get.rvm.io) and run it via

    `bash ./nvm_installer.sh stable --ruby --autolibs=0`

    The last option ensures that it does not ask for sudo access. If it ends up compiling ruby for you, then you need to 
have the requisite packages installed, of course.

* activate rvm:`source ~/.rvm/scripts/rvm`
* switch to rvm's current ruby:`rvm use current`
* check that it is the correct version:`ruby -v`
* install bundler:`gem install bundler`
* install jekyll:`gem install jekyll`   (might be obviated by the github-pages gem installation later but who has time to test that...)
* running jekyll fails if you do not have a file named Gemfile in the top level directory of your site.
I forgot to add this file to the git repo when I first got Jekyll working, so I had to recreate it with contents

    `gem 'github-pages', group: :jekyll_plugin`


* install github-pages:`gem install github-pages`
* run jekyll:`bundle exec jekyll serve` (being on real Linux, the`--no-watch`option is no longer necessary)


---

[back](/)
