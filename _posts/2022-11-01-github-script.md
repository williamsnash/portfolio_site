---
layout: post
author: William Nash
title: Github Script
date: 2022-11-01T18:02:02.415Z
thumbnail-img: /assets/img/posts/hello.jpg
category: Bash, Scripting
summary: A bash script to add, commit and push to github
keywords: Bash, Scripts, Github
thumbnail: /assets/img/posts/code.jpg
permalink: /blog/github_script
---
## W﻿hy?

I﻿ created this script after a seriers of days where I was doing a lot of pushes. It got very tedious to type the git commands to add, commit and push. So I made a simple bash script



## A﻿bout

T﻿his script started off as a very simple script. Just 5 lines, one to get the branch using <code>BRANCH=$(git branch | grep "*" | cut -d' ' -f2)</code> To get the current branch name, <code>COMMAND=@$</code> to get anything typed on the command line and <code> git add .</code>, <code>git commit -m "$COMMAND"</code>, <code>git push -u origin $BRANCH</code>

This gave a simple and fast way to push to github, however, there was an issue. If this script was ran in a non-git directory it would try and run all the commands, this would fail and cause clutter. So I added a check to see if  the current directory was a git repo or not. 

```
#------------------------------------------------------------
# Checks if the current directory is a git repository
EXIT_CODE=0
git branch &>/dev/null || EXIT_CODE=$?
#------------------------------------------------------------
if [ $EXIT_CODE -eq 0 ]; then # If the current directory is a git repository
    BRANCH=$(git branch | grep "*" | cut -d' ' -f2) # Gets the current branch name by parsing git branch
    COMMAND=$@ # Allows the user to set commit message

    git add .
    git commit -m "$COMMAND"
    git push -u origin $BRANCH
else # If the current directory is not a git repository
    if [ $EXIT_CODE -eq 128 ]; then
        echo "Directory not a git repository"
    else
        echo "Unknown error"
    fi
fi
```

B﻿y checking the exit code the of <code>git branch</code> command, I was able to know if the current directory is or isn't a git repo