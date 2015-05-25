#Command Line#

There are hundreds of Git command resources on the internet and you should probably Google them to learn more.  However below are several common commands you should be aware of.  These commands should be run from your repo directory as they are contextual:

```
git clone https://github.com/kgiszewski/LearnUmbraco7.git

//current directory is c:/dev
//this will clone the project listed to c:/dev/LearnUmbraco7
```

```
git fetch --all

//this grabs branches from the GitHub not yet on your machine
```

```
git add .

//this stages all changes, however you should probably use Git Extensions for this
```

```
git commit -m "my message"

//this commits any files staged with the message entered
```

```
git pull origin master

//this pulls changes from GitHub for branch master
```

```
git push origin master

//this pushes local changes to GitHub
```

```
git cherry-pick deb725b91364345e8df941e8ba3ea6656be2ec77

//this merges a single commit to the current branch
```

```
git checkout develop

//this switches to the develop branch
```

```
git checkout -b mynewbranch

//this creates a new branch based on the current branch and switches to it
```

```
git merge master

//this merges master into the current branch (assuming we're not on master)
```