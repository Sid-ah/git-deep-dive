# Git - Deep dive 

### Linus Torvalds said you can think of git as file system. what does it mean to us? 

It means that at the core of Git is a simple key-value data store. What this means is that you can insert any kind of content into a Git repository, for which Git will hand you back a unique key you can use later to retrieve that content.

### Let's see it in action! 

##### Step 1:
let's create a new project and this time will do directly from the command line. So please have your git bash open and create new directory and name it git-deep-dive.

`mkdir git-deep-dive`

##### Step 2:
Let's navigate inside the git-deep-dive folder and initialize it with git. As you might have guessed it will run `git init`

##### Step 3:
Now we will create a README.md file that will hold guidelines to follow to run and maintain our project for that we will run the following command: 
`touch README.md`

##### step 4: 
We have created the Readme.md file but it's empty so let's add something to it. I am going to open it with VS Code you can use the editor of your choice. For that I will run `code README.md` and I will add the following text to it: 

```
# Welcome to github deep dive 

This repo is subject to code conduct please read the rules if you want to contribute as this could results a ban from the community. We welcome contributors help to maintain and keep the content up to date as well as sharing the best practices.

```

A logical thing to perform at this stage is to commit your changes to git database. But before let's talk about how you should branch your project to ensure a safe deployment of your app: 

## Branching your repo

If you're working on a production App the convention is that you will have 3 main branches `master, hotfix, development or staging`. We will be talking about what the purpose of each of those branches.

<details>
  <summary><b>Examples of Branching</b></summary>
  <ul>
	<img src="https://nvie.com/img/hotfix-branches@2x.png" />
  </ul>
</details>

##### master branch
The master branch should always contain the most recent and stable version of your app.

##### Develop branch
This is the branch that devs should be pushing their features branch to. Dev branch should have  a CI/CD integration to run the tests and different code check, once it's successful and it passes all the tests then we can merge with the master branch.

#### Hotfix or Bug-fixes branch
Although this branch is used to push some minor updates, it's main purpose is to fix bug and revert back to previous stable version of our code base. We will see how we are going to revert back and troubleshoot when a problem arise.

Now that we have an idea of what we want to do as far as branching goes it's worth seeing how git stores data locally. Let's go back to the terminal an run some git pipe commands to see what's happening under the hood. 

## So, let's Dive in:

##### step 5: 

let's go back to the git bash and run some git pipe commands let's start with `git hash-object`. It will be good if we can give a string e.g: `git object-hash git-deep-dive`, but git isn't that smart yet. So a way around that we can run `echo "git-deep-dive" | git hash-object -w --stdin` 

so we have seeing that git is a map where the keys are SHA1 and the values are pieces of content.

Note: to view your hidden files you can run ls -a so let's run that. 

let's navigate to our .git directory and take a look at what's in the objects folder of that directory, you should see something like this: 
![alt text](https://res.cloudinary.com/dcqrxsgq8/image/upload/v1545176986/git-deep-dive/git-1.png)

let's look at `fb` the name of the folder that's in a circle and yes it's the first two character of the SHA1 `fbef70402e67eed3ceca1640ab0a9a33d28a8c81` and the name of our file is the remaining of that SHA1. git smart enough to use this schema to organize content and into multiple directories. So the file stored `fbef70402e67eed3ceca1640ab0a9a33d28a8c81` has the string git-deep-dive but git is smart enough to compress the character a get rid of white spaces so we can't just open it with notepad. But we can run a plumbing command to see the content `git cat-file fbef70402e67eed3ceca1640ab0a9a33d28a8c81 -p` 

`-p` stands for prettier printing and the result should return the string `git-deep-dive`. Now if we were to remove `-p` git will return the type of data and in that case `git cat-file fbef70402e67eed3ceca1640ab0a9a33d28a8c81` it will return a blob. 


##### step 6 (commit):

let's run ls which should return `README.md` and if we were to run cat README.md we should see the text we pasted into it earlier 

```
# Welcome to github deep dive 

This repo is subject to code conduct please read the rules if you want to contribute as this could results a ban from the community. We welcome contributors help to maintain and keep the content up to date as well as sharing the best practices.

```
let's our `README.md` to our staging by running the `git add README.md` command. Now we can run the `git status` and it should return green file and it should say `Change to be committed: new file README.md`. Let's commit our README file `git commit -m 'Initialize my project with a readme'`.

Let's go and open up our objects folder inside .git once there we will find the first two number of our commit as our folder name for me it looks something like this: 

![alt text](https://res.cloudinary.com/dcqrxsgq8/image/upload/v1545189580/git-deep-dive/git-2.png)

By now we are used to run `git cat-file -p` to see the content of the commit, so let's actually do that: 
`git cat-file -p 2a70470`

##### Important side note:

Please make sure you check for your generate SHA1 for the step above.


And long and behold there is our first commit 

```
$ git cat-file -p 2a70470
tree 76e0c483d083b6ac42a126800d6200385a24fe8c
author sid-ah <sidah_10@hotmail.com> 1545182970 -0500
committer sid-ah <sidah_10@hotmail.com> 1545182970 -0500

initialize project with a README.md
```

### So what's a commit? 

A commit is a simple short piece of text that's generate by git and stores it content in a blob.

##### step 7 (Branching):

Let's go back to our project, we have created in branches yet - but as we saw earlier initializing a project with `git init` creates automatically a branch called `master`. Let's see where git stores our local branches, so let's take a look at `.git/`. And this time around let's open up `refs/` folder for a change, once there let's open the sub directory called heads. SURPRISE! Our `master` branch was hiding there this whole time :).  And we were to print the content of `master` we will get our commit
```
SIMERZOU@MININT-5FJR9QC MINGW64 ~/Desktop/dev/git-deep-dive (master)
$ cat .git/refs/heads/master
2a7047016e39300305032a30bf77a4ff89b2fd5f
```

### So what's a branch? 

A branch is a point of reference that's direct us to one or more commits. 

Once that's done let's add a file name music.txt `touch music.txt` and that's done let's add some titles to it you could open with your favorite text editor I am using code `code music.txt` copy and paste the content below: 
```
1-Tasha Blanka - Get down 22
2-Bach 
3-Gaada diwan
```
let's add and commit. we know now how to do it `git add music.txt && git commit -m 'Add music.txt'`.

##### step 8 (Merging):

- Let's create a feature branch and call it feature/sidi-ringtones `git branch feature/sidi-ringtones`. let's open the `music.txt` file with our favorite text editor and change `Bach` to `MJ`. Let's just say that Sidi isn't a big fun of classical music. Now, we can add and commit our changes `git add music.txt && git commit -m 'Modify music to include MJ and remove Bach'`.
- Switch back to master branch by moving the HEAD `git checkout master` and then let's merge the changes `git merge feature/sidi-ringtones`


### So what's a merge?

A merge is simply combining lines of (code, files. txt ...etc) together. 

##### step 8 (Tags):

- Annotated  tags:
	They are called annotated tag because they come with a message `git tag -a mytag -m "I love soccer!"` you can try that command on your git project.


Now if we run `git tag` we will get back `mytag` and if we were to `cat-file` the name of my tag like so `git cat-file -p mytag` We will get back our message `I love soccer!`. 

### So what's a tag?

It's a label attach to an object that points to a commit. 


##### step 9 (Remote):


In this step you have a free choice of using what you want to use as your remote repo either (github, bitbucket, or Azure DevOps). Once you choose please create a new repo and  please make sure you don't intialize it with a readme. Once that's done let's Add the remote to our local repo and to do that you can run the command `git remote add origin <remote-url>`. So if we were to open config file inside `.git` folder we should few things:

- remote="origin"
- url=our-remote-repo-url
- fetch=our-remote-repos-url

With this file config in place git will know which repo or repos we want to sync with. 


### So what's a remote: 

Remote is a way for git to know where to pull and push code from.

##### step 10 (Push):

This is the last step before we are done. To push our changes to our remote repo we can use `git push origin -u master` to push our master branch. `-u` stands for upstream and it's only needed the first time we push to the git remote repo every other time we will be just using `git push origin <name-of-the-branch>`

### So what's a push: 

Push is simply a way to sync our remote repo with what's on our local repo. 



### Congrats! You have made it!


Although we are done, I encourage to take a look at the resources below:

https://git-scm.com/

https://medium.com/@patrickporto/4-branching-workflows-for-git-30d0aaee7bf

https://medium.com/@muneebsajjad/git-flow-explained-quick-and-simple-7a753313572f



