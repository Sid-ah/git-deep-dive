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

2. We are going to give our repo a name and initialize it with a readme ![alt text]

We will be creating a new github repo to do so click on [github](https://github.com/) 

*Note:* I am assuming that you already have created a github account and you're logged in and that you have git installed on your command prompt

1. We are going to first create a repo (for a sake of this demo I am keeping it simple, so I went with an open project - you have the ability to restrict it to your organization only by choosing private)
![alt text](https://res.cloudinary.com/dcqrxsgq8/image/upload/v1543944779/github-example/github1.png)
2. We are going to give our repo a name and initialize it with a readme ![alt text](https://res.cloudinary.com/dcqrxsgq8/image/upload/v1543944801/github-example/github2.png)
3. In this step we are going to invite members of our team to join and collaborate![alt text](https://res.cloudinary.com/dcqrxsgq8/image/upload/v1543944820/github-example/github-settings.png) ![alt text](https://res.cloudinary.com/dcqrxsgq8/image/upload/v1543944843/github-example/github-s-1.png) ![alt text](https://res.cloudinary.com/dcqrxsgq8/image/upload/v1543944872/github-example/github-s-2.png) ![alt text](https://res.cloudinary.com/dcqrxsgq8/image/upload/v1543944872/github-example/github-s-2.png)  
Before diving in and start using git commands let's see how we can protect people from pushing to master branch (side note: Most of the time master host production code)

4. We are going to create a rule that would prevent members of the organization from pushing code directly to the master branch  
 ![alt text](https://res.cloudinary.com/dcqrxsgq8/image/upload/v1543944909/github-example/github-s-4.png)  ![alt text](https://res.cloudinary.com/dcqrxsgq8/image/upload/v1543944927/github-example/github-s-5.png) ![alt text](https://res.cloudinary.com/dcqrxsgq8/image/upload/v1543944942/github-example/github-s-6.png)
 
 ### Congrats we finished setting up our project! 
 
 ## Now let's start using git
  
  First let's open the terminal and navigate to the directory where you want to keep you project for me it's going to be `~/Desktop/dev`  
  ```shell
	cd ~/Desktop/dev
  ```
  
  we are going to clone the repo in our local machine and for the we would need the link to our github repo ![alt text](https://res.cloudinary.com/dcqrxsgq8/image/upload/v1543949510/github-example/git-clone.png)
Now that we have the link copied let's go back to the terminal and run the following command
  ```shell
git clone <github-repo-url>
```
Once you are done cloning the repo go inside our project directory

```
ls
// => github-example
cd github-example
```
Since it is a good practice not to push your code to master, we will be creating what we call a feature branch (to keep our development code separated from the production one). To do so we are going to use `git branch` to see what our current head (for us it should return master since we created a new project)
```
git branch 
// => master
  ```
Create a branch using the following naming convention in the terminal.

  ```shell
  $ git checkout -b "<feature-branch>"
  // => The convention is to name your branch after the feature you're currently working on 
  $ git branch
  // => should return <fature-branch>
  ```

Let's open the readme file with your favorite text editor I will be using vim from the command line, but feel free to use editor of your choice


  ```shell
  $ vim README.md 
  ```
press on a letter `i` in the keyboard to insert (add/edit or delete) a line and you can add something of your choice. Once you're done press escape and enter `wq` (for write and quit) 
  
When you work in production env you may make changes to different files and it's hard to keep track that's when git comes in handy. Let's how we it's done:

 ```shell
  $ git status
 
// git status => returns all the files that where changed, deleted or added 
// And the message should say Changes not staged for commit: README.md
  ```
  
  In our case it's going to return `You've modify README.md file please commit your changes`. So now we need to add it to git control:

 ```shell
  $ git add README.md //(or the name of the file that was modified)
  ```
  Now let's check the status again: 
   ```shell
  $ git status
 // =>  Changes to be committed: README.md
  ```
  At this point we just need to commit and push our branch to our github repo: 

   ```shell
  $ git commit -m "Add github links to readme.md file" // -m stands for message 
 // =>  Changes to be committed: README.md 
  ```
  Commit messages should start with a verb, use present tense, and describe the work you have done. 
  
 ## Final step
 
 Our final step is to push our current branch to github and open a pull request tagging the project owner to review your changes:
 
 ```shell
  $ git push origin feature-branch 
  
  // => when it's done successfully you should see 
  // Writing objects: 100% (8/8), 1.03 KiB | 350.00 KiB/s, done.
  // Total 8 (delta 6), reused 0 (delta 0)
  // remote: Resolving deltas: 100% (6/6), completed with 6 local objects.
  // To https://github.com/bon-heur/github-example.git
  // 2308295.80308294..120c778d1  branch -> feature-branch
  ```
  
  - Let's go back to our github repo and open our first pull request: 
  
  step 1: 
  ![alt text](https://res.cloudinary.com/dcqrxsgq8/image/upload/v1543960080/github-example/pull1.png)
  
  step 2:
  ![alt text](https://res.cloudinary.com/dcqrxsgq8/image/upload/v1543960080/github-example/pull2.png)
 
  step 3:
   ![alt text](https://res.cloudinary.com/dcqrxsgq8/image/upload/v1543960080/github-example/pull3.png) 
  
  
  Congrats ! Now you have first git workflow. Below are some good links for best practices with git. 
  
