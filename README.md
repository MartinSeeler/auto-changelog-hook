utomatic CHANGELOG Git Hook

> A simple bash script to automatically generate a `CHANGELOG.md` file with every commit.

## How to use it

Copy the file `post-commit` into the directory `.git/hooks/` of your git project and make it executable.

```bash
chmod +x post-commit
cp post-commit ${YOUR_GIT_PROJECT_PATH}/.git/hooks
```

This hook is only triggered locally, so if you use this with your team, everybody has to store the hook inside his or her local copy, since the `.git` directory is not pushed or tracked with the files from your repository. 

To learn more about the `.git` folder and what it really contains, I suggest you [the brief explanation of the various files and directories inside the `.git` directory](http://www.gitguys.com/topics/the-git-directory/). 

## Customization

### Generated file name 

You can change the name of the generated Changelog file inside the `post-commit` file by changing the variable for `OUTPUT_FILE` to whatever you want. Some projects also use `HISTORY.txt`, `HISTORY.md`, `NEWS.md` or `HISTORY.md`.

### Output format

The default setting of the script generates markdown, which can be seen in the [CHANGELOG.md](CHANGELOG.md).

If you want to change the output format, you should read the [Git documentation for `pretty-formats`](http://git-scm.com/docs/pretty-formats) and change the `--format`argument in `post-commit` file according to your desired format.

You can test your output format by executing the following command inside your git repository folder:

```bash
git --no-pager log --no-merges --format="YOUR_FORMAT_GOES_HERE"
```

