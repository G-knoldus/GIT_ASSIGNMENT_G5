# Git Hub

## hooks Command
[![G5|Git](https://miro.medium.com/max/640/1*75jvBleoQfAZJc3sgTSPQA.jpeg)](https://github.com/G-knoldus/GIT_ASSIGNMENT_G5)

Git hooks are scripts that run automatically every time a particular event occurs in a Git repository. They let you customize Git’s internal behavior and trigger customizable actions at key points in the development life cycle.

- Performance is Excellent
- Secure open source bucket
- Fexiblity is Good

## How it works

All Git hooks are ordinary scripts that Git executes when certain events occur in the repository. This makes them very easy to install and configure.
Hooks can reside in either local or server-side repositories, and they are only executed in response to actions in that repository. We’ll take a concrete look at categories of hooks later in this article. The configuration discussed in the rest of this section applies to both local and server-side hooks.

## Installation of git

- To see if you already have git installed on the System
```sh
git version
```

- Make sure everything is up to date on the system
```sh
sudo apt-get update
```

- To install git use the following command in the terminal
```sh
sudo apt-get install git-all
```

## Commands of Installation of hooks

- Hooks reside in the .git/hooks directory of every Git repository. Git automatically populates this directory with example scripts when you initialize a repository. 
```sh
applypatch-msg.sample       pre-push.sample
commit-msg.sample           pre-rebase.sample
post-update.sample          prepare-commit-msg.sample
pre-applypatch.sample       update.sample
pre-commit.sample
```

## Local Hooks

Local hooks affect only the repository in which they reside.Each developer can alter their own local hooks, so you can’t use them as a way to enforce a commit policy.
 
## Type of hooks
 
- pre-commit
- prepare-commit-msg
- commit-msg

# pre-commit
The pre-commit script is executed every time you run git commit before Git asks the developer for a commit message or generates a commit object. 

```sh
#!/bin/sh

# Check if this is the initial commit
if git rev-parse --verify HEAD >/dev/null 2>&1
then
    echo "pre-commit: About to create a new commit..."
    against=HEAD
else
    echo "pre-commit: About to create the first commit..."
    against=4b825dc642cb6eb9a060e54bf8d69288fbee4904
fi

# Use git diff-index to check for whitespace errors
echo "pre-commit: Testing for whitespace errors..."
if ! git diff-index --check --cached $against
then
    echo "pre-commit: Aborting commit due to whitespace errors"
    exit 1
else
    echo "pre-commit: No whitespace errors :)"
    exit 0
fi
```

# prepare-commit-msg
The prepare-commit-msg hook is called after the pre-commit hook to populate the text editor with a commit message. This is a good place to alter the automatically generated commit messages for squashed or merged commits.

```sh
#!/usr/bin/env python

import sys, os, re
from subprocess import check_output

# Collect the parameters
commit_msg_filepath = sys.argv[1]
if len(sys.argv) > 2:
    commit_type = sys.argv[2]
else:
    commit_type = ''
if len(sys.argv) > 3:
    commit_hash = sys.argv[3]
else:
    commit_hash = ''

print "prepare-commit-msg: File: %s\nType: %s\nHash: %s" % (commit_msg_filepath, commit_type, commit_hash)

# Figure out which branch we're on
branch = check_output(['git', 'symbolic-ref', '--short', 'HEAD']).strip()
print "prepare-commit-msg: On branch '%s'" % branch

# Populate the commit message with the issue #, if there is one
if branch.startswith('issue-'):
    print "prepare-commit-msg: Oh hey, it's an issue branch."
    result = re.match('issue-(.*)', branch)
    issue_number = result.group(1)

    with open(commit_msg_filepath, 'r+') as f:
        content = f.read()
        f.seek(0, 0)
        f.write("ISSUE-%s %s" % (issue_number, content))
```

# commit-msg
The commit-msg hook is much like the prepare-commit-msg hook, but it’s called after the user enters a commit message. 

```sh
#!/usr/bin/env python

import sys, os, re
from subprocess import check_output

# Collect the parameters
commit_msg_filepath = sys.argv[1]

# Figure out which branch we're on
branch = check_output(['git', 'symbolic-ref', '--short', 'HEAD']).strip()
print "commit-msg: On branch '%s'" % branch

# Check the commit message if we're on an issue branch
if branch.startswith('issue-'):
    print "commit-msg: Oh hey, it's an issue branch."
    result = re.match('issue-(.*)', branch)
    issue_number = result.group(1)
    required_message = "ISSUE-%s" % issue_number

    with open(commit_msg_filepath, 'r') as f:
        content = f.read()
        if not content.startswith(required_message):
            print "commit-msg: ERROR! The commit message must start with '%s'" % required_message
            sys.exit(1)
```
