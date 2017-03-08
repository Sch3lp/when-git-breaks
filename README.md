## Knowledge gap

* [ ] knowing what to ignore or delete
* [ ] Files: .gitignore, …
* [ ] git merge conflicts (avoid most conflicts)
* [ ] how to avoid conflicts or resolve them.
* [ ] Revert back to older version (for everything & for only a single file)
* [ ] git remove (removed too much)

### Knowing what to ignore or delete
http://gitignore.io

### Merge conflicts
#### How to avoid
* Don't work on the same code
* Make small commits
* Integrate as fast as possible

#### Tools
* [KDiff3](http://kdiff3.sourceforge.net/)
* [SourceTree](https://www.sourcetreeapp.com/)
* [IntelliJ](https://www.jetbrains.com/help/idea/2016.3/using-git-integration.html), or any proper IDE actually

#### Whitespace and CR/LF conflicts
Be aware that inconsistent formatting settings on your team's workstations can cause merge conflicts that are _difficult_ to solve.

_Difficult_ because it can be hard to distinguish the important changes in between the new line changes or whitespaces.

Luckily most tools provide support to _ignore whitespace_.

### Reverting back
Sometimes you just want to undo your changes, because :poop: happens.

Depending on which stage your changes are in, there's `git revert` and `git reset` to help you out.

#### Your changes are in your staging dir?
Use `git reset` to undo them.

#### Your changes are in a commit?
##### But you haven't pushed them to a remote yet?
You can simply modify your files the way you want, and `git amend`.

##### And you have pushed them to a remote already
Then you can `git revert`, and create a new commit with the undo.

### Exercise

Step 1: fork this repository

Step 2: do as instructed

#### Changing non pushed commit
    git checkout exercise-start

Change the first sentence to _Woke up in a squad car, jailed away for vagrancy_.

Then run `git commit -am "misheard lyrics"` to both add and commit the changed file with a message.

We forgot to also change _jailed away_ to _busted down_.

Let's do that now and change the first sentence to _Woke up in a squad car, busted down for vagrancy_.

Now we want to modify the previous commit that we haven't pushed yet to incorporate this new change, so do

    git commit --amend

And keep the previous commit message as "misheard lyrics".

#### Fixing a merge conflict

    git merge exercise-merge-conflict

We want only the second sentence without comma's.

Your end result should look like this:

    Woke up in a squad car, busted down for vagrancy
    Outside my cell it sure as hell looked like rain


Now that our merge conflict has been resolved, let's add our changed file to our staging directory with `git add lyrics.md`.

#### Reset a failed merge

Let's assume that we botched the merge completely and we want to try the merge again.

    git reset --hard

If we wanted to retain our changes we could've just done `git reset`. `--hard` tells git we don't want any of our changes anymore and want to start clean again.

Perform the merge again with `git merge exercise-merge-conflict`.

This time do the same modification but leave out the `,` in the first sentence as well.

Your end result should now look like this:

    Woke up in a squad car busted down for vagrancy
    Outside my cell it sure as hell looked like rain


Commit with `git commit -am "merged without comma's"`.

#### Reverting a commit
If we do `git log` now, it should look a bit like this:

    commit 00fae8debff7de2f02e06151edfa0ba27973d709
    Merge: 31684e8 d1d8969
    Author: Tim Schraepen <sch3lp@gmail.com>
    Date:   Wed Mar 8 13:27:36 2017 +0100

        merged without comma's

    commit 31684e8abda479d4ae1847ec592b7d893025f4d1
    Author: Tim Schraepen <sch3lp@gmail.com>
    Date:   Wed Mar 8 11:40:28 2017 +0100

        misheard lyrics

    commit d1d8969fd87f4a1b6c1cde301afcd863916a03e8
    Author: Tim Schraepen <sch3lp@gmail.com>
    Date:   Wed Mar 8 11:39:09 2017 +0100

        away with comma's

    commit 6dacd22fec2168a1752a9f265c25ad0d74198cde
    Author: Tim Schraepen <sch3lp@gmail.com>
    Date:   Wed Mar 8 11:35:23 2017 +0100

        init

Let's create a new file in the root directory to contain artists or whatever: `touch artists.md`.

Then add this file and commit it. Your `git log` should have a new top entry:

    commit 93fe283cd8f5f1f6e1ac9be633d4c3b1552aa665
    Author: Tim Schraepen <sch3lp@gmail.com>
    Date:   Wed Mar 8 13:33:25 2017 +0100

        added artists

Our boss reads our commit history and was yelling that our repository isn't meant to host artist information, _we only do lyrics and we do it good god dammit!_.

We could delete our `artists.md` and commit that, but it would make more sense to add more semantics to this event and `git revert` the commit that added the `artists.md` file.

    git revert 93fe28

**Note**: Your commitish (the number-letter combination that identifies a commit) will look differently so don't just copy paste!

This will add a new commit that reverts the commit that introduced the artists file.

Your log should now look like this:

    commit c9e124348db9cc4229cd471fd24874f686cc3a79
    Author: Tim Schraepen <sch3lp@gmail.com>
    Date:   Wed Mar 8 13:36:45 2017 +0100

        Revert "added artists"
        We only do lyrics and we do it good god dammit.

        This reverts commit 93fe283cd8f5f1f6e1ac9be633d4c3b1552aa665.

Since we're rebels (FIGHT THE POWER!), let's add the `artists.md` file anyway and do a `git commit --amend`.

Note that you'll have to also add an artist or change the `artists.md` file in some way, otherwise git will notice the null operation of deleting an empty file and adding an empty file and tell you about it:

```
You asked to amend the most recent commit, but doing so would make
it empty. You can repeat your command with --allow-empty, or you can
remove the commit entirely with "git reset HEAD^".
```

#### Reverting a specific file to a previous version according to a commit
Let's add `##Micky Newbury` as the first line of both the `artists.md` and `lyrics.md` files and commit them.

Change the top line in `artists.md` to `## Micky Newbury - 33rd of August` and commit it as well.

Whoops! We were supposed to add that in the `lyrics.md` file, not in the `artists.md`.

Let's checkout the previous version of `artists.md`:

First get the commitish of the latest version

    git log artists.md

    commit b2f610b7ac232515106d8e7b7061db8b7babbada
    Author: Tim Schraepen <sch3lp@gmail.com>
    Date:   Wed Mar 8 14:43:49 2017 +0100

        added artist

    commit 1e41e4a8e282aae345bb297636d8fb1fc6d2090e
    Author: Tim Schraepen <sch3lp@gmail.com>
    Date:   Wed Mar 8 13:52:22 2017 +0100

        added artist info

My latest version is b2f610, I want to checkout the one before that:

    git checkout b2f610~ artists.md

The `~` represents _the one before_.

Now we can change the top line in `lyrics.md` to `## Micky Newbury - 33rd of August` and commit both files again.

### Note on renaming files to lowercase or uppercase
Here's a typical scenario:

You're just learning a new language and you're using another language's naming conventions.

e.g. You're working on a Java project, but you used .Net naming conventions for _packages_.

.Net convention says: Snakecase your package names: `Vagrancy`.

Java convention says: lowercase your package names: `vagrancy`.

#### Exercise
The `exercise-snake` branch contains a new directory `com/lyrics/Vagrancy` (mind the case). So let's check that out as our starting position.

    git checkout exercise-snake

Now rename `Vagrancy` to lowercase `vagrancy` and perform a `git status`.

_nothing to commit_? But, but, but, but I renamed the directory to lowercase!? What gives GIT?!

So how do we do this properly?

Let's reset back to our starting branch with `git reset --hard`.

So if `mv com/lyrics/Vagrancy com/lyrics/vagrancy` doesn't work, maybe if I prepend it with git it might work?

Try it! I'm getting `fatal: renaming 'com/lyrics/Vagrancy' failed: Invalid argument`.

So let's just try it with an intermediary step:

    git mv com/lyrics/Vagrancy com/lyrics/vagrancytmp
    git mv com/lyrics/vagrancytmp com/lyrics/vagrancy
    git status

Git recognized that we really wanted to rename it! Pure barry!

Another way to do it without `git mv` is to rename it to a temp folder, commit that, then rename it to the actual lowercase folder and `git commit --amend`.

When you `git show`, git also tells you it recognized the rename:

    rename from com/lyrics/Vagrancy/.gitkeep
    rename to com/lyrics/vagrancy/.gitkeep

That's it!