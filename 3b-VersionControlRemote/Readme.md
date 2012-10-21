Version Control Collaboratively

----

[Back To Local Version Control](http://github.com/swcarpentry/2012-10-ucb/tree/master/3a-VersionControlLocal/) - [Forward To Software Engineering](http://github.com/swcarpentry/2012-10-ucb/tree/master/4-SoftwareEngineering/) 

----

**Presented By Katy Huff**
**Based on material by Katy Huff with edits by Joshua Ryan Smith**

## github.com?

GitHub is a site where many people store their open (and closed) source
code repositories. It provides tools for browsing, collaborating on and
documenting code. Your home institution may have a repository hosting system
of it's own. To find out, ask your system administrator.
GitHub, much like other forge hosting services 
( [launchpad](https://launchpad.net),
[bitbucket](https://bitbucket.org), [googlecode](http://code.google.com), 
[sourceforge](http://sourceforge.net) etc.)
provides :

-   landing page support 
-   wiki support
-   network graphs and time histories of commits
-   code browser with syntax highlighting
-   issue (ticket) tracking
-   user downloads
-   varying permissions for various groups of users
-   commit triggered mailing lists
-   other service hooks (twitter, etc.)

**NOTE** Public repos have public licences **by default**. If you don't want to 
share (in the most liberal sense) your stuff with the world, use bitbucket for 
private repos, pay github money for private repos, or host your own.

Software Carpentry hosts a lot of our teaching material on github. You've 
already cloned it yesterday. If you weren't in class yesterday, try this from 
your terminal 

    $ git clone https://github.com/swcarpentry/2012-10-ucb.git
    $ cd 2012-10-ucb


### Digression: SSH

ssh is a protocol for securely sending data across an insecure network. An 
important feature of ssh is the ability to perform public key cryptography. We 
are about to take a little digression, but it is important because ssh is built 
in to git at a very low level.

Public key cryptography consists of a public key, a private key, and an 
algorithm called a cypher. Information can be combined with the public key using 
hte cypher in such a way that it appears like nonsense to anyone not holding the 
private key. The only way to recover the originial information is to use the 
cypher and the private key; using the cypher and the public key only generates 
more nonsense-looking data. In this way, a person can use a public key, encrypt 
data using the cypher + public key, and send the encrypted data over the network 
without fear that someone will intercept the information.

In addition to secure communication, public key cryptography gives 
authentication: you can know that a message sent by someone hasn't been altered. 
A person uses their private key and the cypher on some data to create a hash, 
then sends the data and the resulting hash to someone holding the public key. 
The person on the other end can take the public key + hash and verify the data 
wasn't changed in transit.

In this way people can be certain who made commits to a git repository. 
Furthermore, a git server can be certain that the person who is pushing changes 
to a particular repository actually has commit access to that repo.

## github pasword 

Setting up github at first requires a github user name and password. 
Please take a moment to [create a free one](https://github.com/signup/free)
(if you want to start paying, you can add that to your account some 
other day). 

## github ssh keys

It will help you to set up automatic authorization, so that github can handshake
with your computer (in this case, your virtual machine).
There are [some setup instructions](http://help.github.com/set-up-git-redirect)  
on the website, but I'll do this along with you at the front of the room as 
well. 

    $ cd ~/.ssh

It will likely say "no such file or directory."

    $ ssh-keygen -t rsa -C "your_email@youremail.com"
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/swc/.ssh/id_rsa):  <press enter>

The path that it provides will be to this home directory. This is okay. **Press 
enter.** You may enter a passphrase. You'll see something like this :

    Created directory '/home/swc/.ssh'.
    Enter passphrase (empty for no passphrase): 
    Enter same passphrase again: 
    Your identification has been saved in /home/swc/.ssh/id_rsa.
    Your public key has been saved in /home/swc/.ssh/id_rsa.pub.
    The key fingerprint is:
    09:06:c6:0f:24:b7:84:ef:22:74:de:95:f0:99:64:5d your_email@youremail.com
    The key's randomart image is:
    +--[ RSA 2048]----+
    |  .+*   . .E     |
    |  .=o+ o .       |
    |   ..oB +        |
    | . ....B .       |
    |. o.. . S        |
    |. ....           |
    | . .             |
    |                 |
    |                 |
    +-----------------+

## git config : Configuring your git environment

Once you've set up your rsa keys, you need to tell github who you are.
Crack open a terminal.

    $ git config --global user.name "Firstname Lastname"
    $ git config --global user.email "your_email@youremail.com"

Unless your name is Firstname Lastname, please don't copy the above
lines verbatim. Make the appropriate substitutions.

If you did this properly, you'll have a file in your home **(\~)**
directory that's called **.gitconfig** . It's contents should look like
:

    [user]
          name = Katy Huff
          email = katyhuff@gmail.com

This configuration step allows github to properly credit the authorship
of changes you make in your repository. For projects with numerous
authors, this is essential.

Another configuration step for some will be to set their favorite text
editor as git's text editor of choice. This is optional, since vi is
usually the default, but can be done with the following command (if you
like **nano** for example):

    $ git config --global core.editor nano

## git remote : Steps for Forking a Repository

A key step to interacting with an online repository that you have forked
is adding the original as a remote repository. By adding the remote
repository, you inform git of a new option for fetching updates and
pushing commits.

The **git remote** command allows you to add, name, rename, list, and
delete repositories such as the original one **origin** from your
fork, others that may be **parallel** to your fork, and so on.

### Exercise : Fork Our GitHub Repository

While you probably already have a copy of the 2012-10-ucb repository,
GitHub doesn't know about it yet. You'll need to tell github you want to
have an official fork of this repository.

Step 1 : Go to our 
[repository](https://github.com/swcarpentry/2012-10-ucb) from your browser, and
click on the Fork button. Choose to fork it to your username rather than
any organizations.

Step 2 : Add your new repository as a remote in the previous repository

    $ git remote add username https://github.com/username/2012-10-ucb.git
    $ git remote -v
    origin  https://github.com/swcarpentry/2012-10-ucb.git (fetch)
    origin  https://github.com/swcarpentry/2012-10-ucb.git (push)
    username        https://github.com/username/2012-10-ucb.git (fetch)
    username        https://github.com/username/2012-10-ucb.git (push)

All repositories that are clones begin with a remote called origin.

Step 3 : Make sure that it worked by updating your local repository from your 
fork.

    $ git pull username master
    Already up-to-date.

## git fetch : Fetching the contents of a remote

Now that you have alerted your repository to the presence of others, it
is able to pull in updates from those repositories. In this case, if you
want your master branch to track updates in the original 2012-10-ucb
repository, you simply **git fetch** that repository into the master
branch of your current repository.

    $ git fetch origin

The fetch command alone merely pulls down information recent changes
from the original master (origin) repository. By itself, the fetch
command does not change your local working copy. To update your local
working copy to include recent changes in the original (origin)
repository, it is necessary to also merge.

## git merge : Merging the contents of a remote

To incorporate origin changes from the original master repository (in
this case swcarpentry/2012-10-ucb) into your local working copy, you
must both fetch and merge. The process of merging may result in
conflicts, so pay attention. This is where version control is both at
its most powerful and its most complicated.

### Exercise : Fetch and Merge the Contents of Our GitHub Repository

Step 1 : Fetch the recent remote repository history

    $ git fetch origin

Step 2 : Make certain you are in the master branch and merge the
upstreeam master branch into your master branch

    $ git checkout master
    $ git merge origin\master

Step 3 : Check out what happened by browsing the directory.

## git pull : Pull = Fetch + Merge

The command **git pull** is the same as executing **git fetch** followed
by **git merge**. Though it is not recommened for cases in which there
are many branches to consider, the pull command is shorter and simpler
than fetching and merging as it automates the branch matching.
Specificially, to perform the same task as we did in the previous
exercise, the pull command would be :

    $ git pull origin
    Already up-to-date.

When there have been remote changes, the pull will apply those changes
to your local branch, unless there are conflicts with your local
changes.

## git push : Sending Your Commits to Remote Repositories

The **git push** command pushes commits in a local working copy to a
remote repository. The syntax is git push [remote] [local branch].
Before pushing, a developer should always pull (or fetch + merge), so
that there is an opportunity to resolve conflicts before pushing to the
remote.

We'll talk about conflicts later, but first, since we have no conflicts
and are up to date, we can make a minor change and send our changes to
your fork, the "origin."

    $ git push username master

If you have permission to push to the origin repository, sending
commits to that remote is exactly analagous.

    $ git push origin master

In the case of the 2012-10-ucb code, new developer accounts will not allow
this push to succeed. You're welcome to try it though.

## git merge : Conflicts

This is the trickiest part of version control, so let's take it very
carefully.

In the 2012-10-ucb code, you'll find a file called Readme.md. This is a
standard documentation file that appears rendered on the landing page
for the repository in github. To see the rendered version, visit your
fork on github, (https://github.com/username/2012-10-ucb/).

For illustration, let's imagine that, suddenly, each of the developers
on the 2012-10-ucb code would like to welcome visitors in a language other
than English. Since we're all from so many different places and speak
so many languages, there will certainly be disagreements about what to
say instead of "Welcome."

I, for example, am from Texas, so I'll push (to the origin repository) my own 
version of Welcome on line 2 of Readme.md.

You may speak another language, however, and may want to replace the 
english word Welcome with an equivalent word that you prefer (willkommen, 
bienvenido, benvenuti, etc.).

You'll want to start a new branch for development. It's a good
convention to think of your master branch as the "production branch,"
typically by keeping that branch clean of your local edits until they
are ready for release. Developers typically use the master branch of
their local fork to track other developers changes in the remote
repository until their own local development branch changes are ready
for production.

### Exercise : Experience a Conflict

Step 1 : Make a new branch, edit the readme file in that branch, and
commit your changes.

    $ git branch development
    $ git checkout development
    Switched to branch 'development'
    $ kate Readme.md &
    <edit the readme file and exit kate>
    $ git commit -am "Changed the welcome message to ... "

Step 2 : Mirror the remote origin repository in your master branch by
pulling down my changes

    $ git checkout master
    Switched to branch 'master'
    $ git fetch origin
    $ git merge origin/master
    Updating 43844ea..3b36a87
    Fast-forward
     README.rst |   2 +-
     1 files changed, 1 insertions(+), 1 deletions(-)

Step 3 : You want to push it to the internet eventually, so you pull
updates from the origin repository, but will experience a conflict.

    $ git merge development
    Auto-merging Readme.md
    CONFLICT (content): Merge conflict in Readme.md
    Automatic merge failed; fix conflicts and then commit the result.

## git resolve : Resolving Conflicts

Now what?

Git has paused the merge. You can see this with the **git status**
command.

    # On branch master
    # Unmerged paths:
    #   (use "git add/rm <file>..." as appropriate to mark resolution)
    #
    #       unmerged:      Readme.md
    #
    no changes added to commit (use "git add" and/or "git commit -a")

The only thing that has changed is the Readme.md file. Opening it,
you'll see something like this at the beginning of the file.

    =====================
    <<<<<<< HEAD
    Howdy
    =======
    Willkommen
    >>>>>>> development
    =====================

The intent is for you to edit the file, knowing now that I wanted the
Welcome to say Howdy. If you want it to say Willkommen, you should
delete the other lines. However, if you want to be inclusive, you may
want to change it to read Howdy and Willkommen. Decisions such as this
one must be made by a human, and why conflict resolution is not handled
more automatically by the version control system.

    =====================
    Howdy and Willkommen
    =====================

This results in a status To alert git that you have made appropriate
alterations,

    $ git add Readme.md
    $ git commit
    Merge branch 'development'

    Conflicts:
      Readme.md
    #
    # It looks like you may be committing a MERGE.
    # If this is not correct, please remove the file
    # .git/MERGE_HEAD
    # and try again.
    #
    $ git push origin master
    Counting objects: 10, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (6/6), done.
    Writing objects: 100% (6/6), 762 bytes, done.
    Total 6 (delta 2), reused 0 (delta 0)
    To git@github.com:username/2012-10-ucb.git

## gitolite

[Gitolite](https://github.com/sitaramc/gitolite) is a way for you to host your own multi-user git repositories. I'm not going to go into details here, but all you need is a machine with some drive space and network access. You can install [minimal ubuntu](https://help.ubuntu.com/community/Installation/MinimalCD), then sudo apt-get install gitolite will pull in everything you need. At that point, your collaborators will only need to send you their public ssh keys for you to configure pull and push access to the repos.
