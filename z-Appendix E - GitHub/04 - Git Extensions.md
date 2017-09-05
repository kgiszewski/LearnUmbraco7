# Git Extensions

Git Extensions is a free tool that helps users (new and old) operate their local Git repos.  You can download it [here](https://code.google.com/p/gitextensions/).  Installation may seem daunting, but please read each of the installation panels and when in doubt, just take the defaults.  By installing Git Extensions, you are also installing Git itself which is required if you want to actually *use* Git.

## Why use Git Extensions?
Git Extensions visualizes you commit history and provides a GUI to perform many of the command line interface (CLI) functions.  Many developers use Git Extensions in a hybrid way meaning they use the GUI for some things and using CLI commands for other things.

Git Extensions excels at graphically showing which files have changed and which lines have changed within them.  It has a great UI to stage your files as your choosing what you want to commit.

![git-extensions.png](assets/git-extensions.png)

## Commit Something
When you open Git Extensions and click `Commit` along the top toolbar, you will see the staging/commit panel.  In order to commit a change, you need to stage one or more files.  You don't have to stage all of the files and may want to split up your changes into multiple commits.  You can even abandon changes by *resetting* a file before you commit it.  When you reset a file you discard all pending changes and nothing will get committed.  You can even reset a file that has been changed line by line.  Conversely you can even stage a file line by line.  For instance if you change your web.config in two spots but only want to commit one part right now, you can click the file in the top right, highlight the lines to commit in the top right, then press `s` to stage *just those lines*!

>Resetting and staging by line requires that a change actually is pending on those lines.

Finally once you have staged your changes, enter a commit and it is now committed locally.  When ready to notify GitHub, perform a push.

[<Back 03 - gitignore](03%20-%20gitignore.md)

[Next> 05 - Contribute to a Project](05%20-%20Contribute%20to%20a%20Project.md)
