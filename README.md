# Git and GitHub: A Practical Workflow Guide

This guide explains practical Git and GitHub habits for managing an individual project. It assumes Git is installed and you have a GitHub account.

## Contents

- [1. Clone a repository](#1-clone-a-repository)
- [2. Make a change on a branch](#2-make-a-change-on-a-branch)
- [3. Commit and push](#3-commit-and-push)
- [4. Open and merge a pull request](#4-open-and-merge-a-pull-request)
- [5. Resolve a merge conflict](#5-resolve-a-merge-conflict)
- [6. Inspect project history](#6-inspect-project-history)

## 1. Clone a repository

Create a repository on GitHub, then copy its HTTPS or SSH URL. Clone it and move into the new directory:

```bash
git clone https://github.com/USERNAME/REPOSITORY.git
cd REPOSITORY
```

Check that Git connected the local copy to the expected remote and see the current branch:

```bash
git remote -v
git status
```

`origin` is the conventional name for the remote repository. The default branch is commonly named `main`.

## 2. Make a change on a branch

Start a descriptive feature branch from the current `main` branch. Branches keep unfinished work separate from the stable version of the project.

```bash
git switch main
git pull origin main
git switch -c feature/update-guide
```

Edit the project files, then inspect what changed before staging:

```bash
git status
git diff
```

Use one branch for a focused task. Names such as `feature/add-installation-steps` make the branch's purpose easy to recognize.

## 3. Commit and push

Stage only the files for this task. Review the staged patch, then create a concise commit that describes the change:

```bash
git add README.md
git diff --cached
git commit -m "Add installation steps to guide"
```

Push the branch to GitHub. The `-u` option sets its upstream, so later pushes can use `git push` by itself.

```bash
git push -u origin feature/update-guide
```

Keep commits small and focused. A commit should record one understandable step, rather than combining unrelated edits.

## 4. Open and merge a pull request

On GitHub, open a pull request from your feature branch into `main`. Give it a specific title and explain what changed and how you checked it. Link the relevant Issue in the description with a closing keyword, for example `Closes #1`.

Before merging, inspect the pull request's **Files changed** tab. Check that the diff contains only the intended edits and that the guide is accurate and readable. For an individual project, you review your own pull request; a second-person approval is not required unless repository settings say otherwise.

After the review, merge the pull request on GitHub and delete the merged branch when prompted. Update the local `main` branch before starting another task:

```bash
git switch main
git pull origin main
```

## 5. Resolve a merge conflict

A merge conflict occurs when Git cannot automatically combine overlapping edits. The following sequence creates that situation with two branches editing the same line in `README.md`:

1. From an up-to-date `main`, create `feature/guide-wording-a`. Change one sentence in `README.md`, commit it, and push the branch.
2. Return to the original `main` commit and create the second branch from there, not from branch A:

	```bash
	git switch main
	git switch -c feature/guide-wording-b
	```

	Change the same sentence differently, commit the change, and push this branch too.
3. Open and merge the pull request for `feature/guide-wording-a` into `main`.
4. Update the second branch with the new `main`:

	```bash
	git switch feature/guide-wording-b
	git fetch origin
	git merge origin/main
	```

5. Git reports a conflict and marks the affected file. Open it and find the conflict markers:

	```text
	<<<<<<< HEAD
	text from the current feature branch
	=======
	text merged from main
	>>>>>>> origin/main
	```

6. Edit the section to keep the intended final wording. Remove all conflict markers, save the file, and inspect the result. Then stage and commit the resolution:

	```bash
	git diff
	git add README.md
	git commit -m "Resolve guide wording conflict"
	git push
	```

7. Recheck the second pull request's diff and merge it. Record what conflicted, why you chose the final wording, and how you verified that the markers were removed.

Do not blindly choose one side: read both versions and produce a final result that preserves the information the project needs.

## 6. Inspect project history

Use `git log` to review the commit sequence. These options show a compact graph, branches, and commit messages:

```bash
git log --oneline --graph --decorate --all
```

Use `git show` to inspect a particular commit, or `git diff main...feature/update-guide` to compare a feature branch with its base. On GitHub, Issues and a Project Board help track tasks, while **Pull requests** records review and merge activity.

## Project Files

- [`PLAN.md`](PLAN.md) describes the guide's audience and structure.
- [`version_control_practice.py`](version_control_practice.py) is a small companion program that prints a basic Git workflow.

## Screencast

Add the link to the 3-5 minute repository walkthrough here after recording it.
