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

let's go back to the git bash and run some git pipe commands let's start with `git hash-object`. It will be good if we can give a string e.g: `git object-hash git-deep-dive`, but git isn't that smart yet. So a way around that we can run `echo "git-deep-dive" | git hash-object -w` 



  
