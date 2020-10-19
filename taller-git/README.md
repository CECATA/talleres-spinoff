# Git version control

## Git's data model

### Snapshots

```
<root> (tree)
|
+- foo (tree)
|  |
|  + bar.txt (blob, contents = "hello world")
|
+- baz.txt (blob, contents = "git is wonderful")
```

### Modeling history: relating snapshots

```
o <-- o <-- o <-- o
	    ^
	     \
	       -- o <-- o
```

```
o <-- o <-- o <-- o <---- o
	    ^            /
	     \          v
	       -- o <-- o
```

### Data model, as pseudocode

```
// a file is a bunch of bytes
type blob = array<byte>

// a directory containts named files and directories
type tree = map<string, tree | blob>

// a commit has parents, metadata, and the top-level tree
type commit = struct {
    parent: array<commit>
    author: string
    message: string
    snapshot: tree
}
```

### Objects and content-addressing

```
type object = blob | tree | commit
```

```
objects = map<string, object>

def store(object):
    _id = shal(object)
    objects[id] = object

def load(_id):
    return objects[id]
```

`git cat-file -p 698281bc680d1995`

```
20032 blob 444232badf2341 baz.txt
10003 tree c678asc7c74334 foo
```

`git cat-file -p 444232badf2341`


### References

```
references = map<string, string>

def update_reference(name, _id):
    references[name] = _id

def read_reference(name):
    return references[name]

def load_reference(name_or_id):
    if name_or_id if references:
	return load(references[name_or_id])
    else:
	return load(name_or_id)
```

### Repositories

A git *repository*** it is the data of `objects` and `references`.


## Git command-line interface

### Basics

- `git help <command>`: get help for a git command
- `git init`: creates a new git repo, with data stored in the .git directory
- `git status`: tells you what's going on with the data
- `git add <filename...>`: adds files to staging area
- `git commit`: creates a new commit
- `git log`: shows a flattened log of history
- `git log --all --graph --decorate`: visualizes history as a DAG
- `git diff <filename>`: show changes you made relative to the staging area
- `git diff <revision> <filename>`: shows differences in a file between snapshots
- `git checkout <revision>`: updates HEAD and current branch


### Branching and merging

- `git branch`: shows branches
- `git branch <name>`: creates a branch
- `git checkout -b <name>`: creates a branch and switches to it. Same as `git branch <name>; git checkout <name>
- `git merge <revision>`: merges into current branch
- `git mergetool`: use a fancy tool to help resolve merge conflicts
- `git rebase`: rebase set of patches onto a new base


## Remotes

- `git remote`: list remotes
- `git remote add <name> <url|path>: add a remote
- `git push <remote> <local branch>:<remote branch>`: send objects to remote, and update remote reference
- `git branch --set-upstream-to=<remote>/<remote branch>`: set up correspondence between local and remote branch
- `git fetch`: retrieve objects/references from a remote
- `git pull`: same as `git fetch; git merge`
- `git clone`: download repository from remote

## Undo
- `git commit --amend`: edit a commit's contents/message
- `git reset HEAD <file>`: unstage a file
- `git checkout -- <file>`: discard changes
