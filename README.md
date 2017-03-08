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
#### Changing non pushed commit
    git checkout exercise-start

Change the first sentence to _As you lay tonight beside me baby, on the morning that you came_.

Then run `git commit -am "misheard lyrics"` to both add and commit the changed file with a message.

We forgot to also change _tonight_ to _to die_.

Let's do that now and change the first sentence to _As you lay to die beside me baby, on the morning that you came_.

Now we want to modify the previous commit that we haven't pushed yet to incorporate this new change, so do

    git commit --amend

And keep the previous commit message as "misheard lyrics".

#### Fixing a merge conflict

    git merge exercise-merge-conflict

We want only the second sentence without comma's.

Your end result should look like this:
```
As you lay to die beside me baby, on the morning that you came
would you wait for me the other one wait for me

You run with the devil
You run with the devil
```

Now that our merge conflict has been resolved, let's add our changed file to our staging directory with `git add .`.

Let's undo our merge so we can start from our own (`exercise-start`) version again.

    git reset


### Note on renaming files to lowercase or uppercase
Here's a typical scenario:

You're just learning a new language and you're using another language's naming conventions.

e.g. You're working on a Java project, but you used .Net naming conventions for _packages_.

.Net convention says: Snakecase your package names: `Fleetfoxes`.

Java convention says: lowercase your package names: `fleetfoxes`.

#### Exercise
The `exercise-snake` branch contains a new directory `com/lyrics/Fleetfoxes` (mind the case).

The `exercise-lower` branch contains a commit to rename the `com/lyrics/Fleetfoxes` directory to all lowercase: `com/lyrics/fleetfoxes`.

    git checkout exercise-snake
    git merge exercise-lower

What does your directory look like? Is it `Fleetfoxes` or `fleetfoxes`?