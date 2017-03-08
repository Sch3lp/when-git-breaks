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

#### Exercise
    git checkout exercise-start
    git merge exercise-merge-conflict

Your end result should look like this:

### Reverting back
