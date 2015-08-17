# Automatic CHANGELOG Git Hook

> A simple bash script to automatically generate a `CHANGELOG.md` file with every git commit.

## How to use it

Copy the file `post-commit` into the directory `.git/hooks/` of your git project and make it executable.

```bash
chmod +x post-commit
cp post-commit ${YOUR_GIT_PROJECT_PATH}/.git/hooks
```

### Side Note

This hook is only triggered locally, so if you use this with your team, everybody has to store the hook inside his or her local copy, since the `.git` directory is not pushed or tracked with the files from your repository. 

To learn more about the `.git` folder and what it really contains, I suggest you [the brief explanation of the various files and directories inside the `.git` directory](http://www.gitguys.com/topics/the-git-directory/). 

## Customization

### Generated file name 

You can change the name of the generated CHANGELOG file inside the `post-commit` file by modifying the variable `OUTPUT_FILE` to whatever you want. Some projects also use `HISTORY.md`, `NEWS.md` or `RELEASE.md`, but I suggest you stick to the convention.

### Output format

The default setting of the script generates markdown, which can be seen in the [CHANGELOG.md](CHANGELOG.md).

If you want to change the output format, you should read the [Git documentation for `pretty-formats`](http://git-scm.com/docs/pretty-formats) and change the `--format`argument in `post-commit` file according to your desired format.

You can test your output format by executing the following command inside your git repository folder:

```bash
git --no-pager log --no-merges --format="YOUR_FORMAT_GOES_HERE"
```

If you are happy with the result, just replace the command in the `post-commit` file with your command. 
Don't forget to pipe the output in you're CHANGELOG with ` > $OUTPUT_FILE` at the end of the line.

## How it works

The script uses [Git Hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) to execute the `git log` command after every commit and write the output inside the `CHANGELOG.md` file.

Since we want to add the changes in our `CHANGELOG.md` as well, we have to use [Git's `--amend` feature](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History) to change the previous commit and apply the changes from the script, too.

By changing the last commit, we are triggering the `post-commit` hook another time, since the `--ammend` commit is a commit as well. To prevent an infite execution of this hook, we have to check, whether our generated `CHANGELOG.md` have have changed at all. If it is executed the second time, the file should not be different. 

We can check this by using the [`git status` command with `--porcelain`](http://git-scm.com/docs/git-status) and parse the output for our `$OUTPUT_FILE`. If the process returns a code higher than zero, we had a match and know  that we have a changed `CHANGELOG.md`, which we have to append to our previous commit.

## Why I don't recommend this script

To understand the idea behind a change log, I highly suggest reading [Keep a CHANGELOG](http://keepachangelog.com/) and / or listening to the episode [127: Keep a CHANGELOG with Olivier Lacan](http://5by5.tv/changelog/127).

It says

> A change log is a file which contains a curated, chronologically ordered list of notable changes for each version of an open source project.

Even dough, this matches partly the generated change log by this script, since there are changes in chronologically order, the section about [What’s the point of a change log?](http://keepachangelog.com/#what-s-the-point-of-a-change-log-) does not fit at all.

> To make it easier for users and contributors to see precisely what notable changes have been made between each release (or version) of the project.

This script stupidly dumps all the commit logs into a single file, which doesn't help anybody in any way.

You should realy consider if this is the right way to keep a changelog for your project and what the benefits are by using this script, despite the fact that someone does not have to execute `git log` by itself in the command line.

### [Why can’t people just use a git log diff?](http://keepachangelog.com/#why-can-t-people-just-use-a-code-git-log-code-diff-)

> Because log diffs are full of noise — by nature. They could not make a suitable change log even in a hypothetical project run by perfect humans who never make typos, never forget to commit new files, never miss any part of a refactoring. The purpose of a commit is to document one atomic step in the process by which the code evolves from one state to another. The purpose of a change log is to document the noteworthy differences between these states.

> As is the difference between good comments and the code itself, so is the difference between a change log and the commit log: one describes the why, the other the how.