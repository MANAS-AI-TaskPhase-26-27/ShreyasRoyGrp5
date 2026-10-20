**Git** is a free, open-source distributed version control system designed to track changes in source code and files over time.

### Git vs. GitHub

- **Git** is the local **command-line tool** that manages project versions.
    
- **GitHub** (or GitLab, Bitbucket) is a **cloud-hosting service** where developers push their Git repositories to back up work and collaborate online.

\<> -> req. argument
\[] -> optional argument
working directory -> where editing of data/ code takes place.
staging area -> loading area for what is in the next commit
repository -> permanent history and storage area

$ git commit: pushes changes into repository
$ git branch \<branchName>: branches different pointers (to help develop features separately and later merge it back in)
$ git checkout \<branchName>: selects specific pointer
$ git merge \[branchName]: Merges branches together

![[Screenshot 2026-09-03 191310.png]]
Half Assed Merged
![[Pasted image 20260903192343.png]]
Complete Merge(merged main into bugFix)

$ git rebase \<branchName>: Takes a set of commits, copies them and plops them elsewhere. Make it look like 2 features were developed in sequence, when in reality they where developed parallelly. 
> Make sure to have the proper checkout commit/ pointer when rebasing.

Proper pointer means the branched node
![[Pasted image 20260903232014.png]]
Half assed Rebased bugFix to main (stacked bugFix commit on top of main) ($ git rebase main)
![[Pasted image 20260903232135.png]]
Complete Rebased of main with bugFix ($ git rebase bugFix)

## Moving Around In Git:

 - Head: Symbolic name for currently checked out commit. Head follows the current active pointer. Detaching the head from a pointer can be done using: $ git checkout \<nodeHash>
NOTE: Nodes are referred to by their hash (encrypted name)
$ git log: Used to retrieve hashes (encrypted names)
 - Relative Refs/Commits: Two simple relative commits are noted here -
	 - Moving upwards one commit at a time with `^`
		 - $ git checkout main^
		 - "main^" => "first parent of main"
		![[Pasted image 20260903235310.png]]($ git checkout main^)
		Here, c1 is the  parent(first generation ancestor) of main pointer
		The "HEAD" can also act as a \<branchName>, allowing us to go back in time by using 
		$ git checkout HEAD^
	- Moving upwards a number of times with `~<num>`
		- $ git checkout HEAD~4 : this will move pointer 4 times backward in time
		- git branch -f main HEAD~3
		- git branch -f bugFix HEAD^
	- Moving upwards using git reset: (By rewriting)
		- git reset HEAD~1
	- Moving upwards using git revert: (By reversing changes and sharing reversed changes)
		- git revert HEAD^
- `git cherry-pick <Commit1> <Commit2> ...` 
	- copies a series of commits below current location(HEAD) 
- Git Interactive(-i) Rebase
	- Can do many things, but here we just focus on 2 main abilities
	- Reorder of commits
		- Choose to keep all commits or drop specific ones. Each commit is by default set to be included.
		- `git rebase -i HEAD~num` allows us to reorder previous `num` nodes. 
		- Within the interactive menu opened by executing this command, we can choose o include/exclude these nodes and can free reorder them by drag and drop mechanism.
		- After reordering, the `HEAD` initially moves upward `num` times and new copied nodes are created by the order specified. After node creation, the main branch points to last node created and HEAD returns to main branch.
- The Staging Area
	- For commits to stay tidy, and to ensure all changed files are not automatically included as commits, we are given the liberty to pick exactly what changes come along with each commit.
	- `git status` checks where thins stand at the current moment.
	- Files are "staged" with `git add filename`
	- After a file is staged `git commit` seals it into a snap shot
	- To ignore certain files, or to never want to commit certain files, `.gitignore` file can be made and those files can be added to that file.

## Undoing in Git:
 - To "un"stage a file that we weren't supposed to, we use `git restore`:
	 - `git restore -- staged <file>` "restores" file from staging area to working directory.
	 - `git restore <file>` removes file from working directory. 
