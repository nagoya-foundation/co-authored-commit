# co-authored-commit

GitHub provides a mechanism to create [commits with multiple authors](https://help.github.com/articles/creating-a-commit-with-multiple-authors/), which are visible on GitHub interface. However, git does not have a built-in command to execute this action easily. This project comes up with a [Git Alias](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases) in order to create Co-authored commits using the command line.

If you pass the email in `--co` flag both will be authors, otherwise only the person who commited will be seen by github. 

![commit with email](https://user-images.githubusercontent.com/7308241/41205762-046eacd2-6cd0-11e8-9be9-3ded468e912b.png)

![commit without email](https://user-images.githubusercontent.com/7308241/41205757-ead10450-6ccf-11e8-8c3c-28bf8966f042.png)

You can see the first commit [here](https://github.com/nagoya-foundation/r-functions-performance/commit/457e8c18aedbe2d8b54f0965da9d739d118d82e8) and the second [here](https://github.com/nagoya-foundation/co-authored-commit/commit/d11862fb4b7ff5f1b9f4fa1cc0f5aec807f853b3).

## Installation

Add alias located at the [config file](config) to the bottom of one of the following [git configuration file](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup):

* For global scope: `~/.gitconfig` or `~/.config/git/config`
* For system scope: `/etc/gitconfig`
* For project scope: `.git/config`

```
[alias]
    co-commit = "!co_authored_commit(){ usage='usage: git commit -m \"Commit message\" --co \"co_author_name\"\n'; while [ \"${#}\" -gt 0 ]; do case \"${1}\" in -m) shift; message="${1}"; shift;; --co) shift; co_author=\"Co-authored-by: \"${1}\"\n\"; co_authors=\"${co_authors}${co_author}\"; shift;; *) shift;; esac; done; if [ -z \"${co_authors}\" ]; then echo ${usage}; exit 1; fi; if [ -z \"${message}\" ]; then echo ${usage}; exit 1; fi; co_authored_message=\"${message}\n\n\n${co_authors}\"; git commit -m \"${co_authored_message}\"; }; co_authored_commit"
```

## Usage

After the installation step, you are able to perform the following command:

```
$ git co-commit -m "Commit message" --co "co-author <co-author-email>"
```
The `-m` flag is the commit message flag, the same as the `git commit` command.

The `--co` flag is passed to inform both co-author name and email. If one chooses to keep their email private, use `<no-reply>` instead.

It is possible to pass multiple `--co` flags.

## Limitations

For now, it is mandatory to pass the the git commit message flag.
