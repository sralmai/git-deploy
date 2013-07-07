# git-deploy

Deploy things via git repo.


##Install

    $ ln -s git-deploy <someplace in your $PATH>

Yep. You should link it from this directory, so when you fix issues or add features, 
there is zero bar for committing.


## Usage

Oh. I have this unique problem. I have this git repository that I want to deploy somewhere.
I want to push code and then maybe run some stuff.

### Maybe a website?

I've got a server named *snarky* somewhere running a website. It serves pages from $HOME/mywebsite/www.

Right now I copy pages via scp or rsync. *But*... since my whole
project is a git repo... (with those web pages living in the "www"
subdirectory in my project), I can delete that static $HOME/mywebsite
project and enchant.

      git deploy enchant snarky mywebsite

Boom. Done.

Now I hack for a bit, commit some changes, and want to redeploy.

    git deploy

Done.


#### But... my website runs on node via npm or some magic like that, and I want to reload.

Simple. Add a script named ".deploy" to your project. This will be executed after every deploy.

(Of course, restarting a process requires a bit of coordination. Hopefully you save the PID somewhere?)


### Or some backend process?

I've got a server named *pig* somewhere, and I've got a service running on it that's got some home-baked dependencies.
(Hopefully, this is in test or staging, and you are actually doing something else in production.)

And let's say that dependency is a python project named pylatinizer.

Of course, this is a git repo on your dev machine. And you are hacking on it daily.

Let's make this better.

Add ".deploy" to your git repo, and fill it with:

    #!/usr/bin/env bash
    python setup.py install

Commit.


Now enchant our friend pig:

    git deploy enchant pig pylatinizer-dev


(Note: pylatinizer-dev is a random name I just made up... it doesn't
matter... this is the root directory name of the remote git
repo we are creating. Since we just want to push code and install, it *doesn't matter*
what we name it.)

Boom. You just upated pylatinizer.

Now hack for a bit, and commit some changes.

    git deploy

Your friend pig is running the latest and greatest.


## Disclaimer

This is an absurdly simple bash script. Also, the examples I gave are
contrived. But the idea is good. And this implementation is super
lightweight and good for hacking (on your projects that need
deployment... this bash script is not necessarily good for hacking).

(At this point, I actually searched for git-deploy on github and
realized there are *many* implementations. But they are all way more
complicated and/or require ruby or php or some other language. And you need config files.)

If you want a git-deploy for production use, you might want to look at
[a grown-up
git-deploy](https://github.com/git-deploy/git-deploy). Reference
disclaimer: I do not know the maintainers, and have not actually used
their tool. But a cursory inpection of docs and source reveals it to
look pretty good.