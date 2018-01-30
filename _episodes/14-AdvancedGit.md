---
title: Pull requests and some idiosyncrasies of LONI
teaching: 30
exercises: 0
questions:
- "How can I use git to receive new material from you, for this class?"
- "`git pull` copies changes from a remote repository to a local repository."
- "If I can't use git on LONI, how can I transfer files to the cluster?"
---
## How can I use Git to receive new class materials from you, and to share scripts with you?

First, we will make a fork of the class repository. A fork is a copy of all the class materials, saved to your GitHub account. So you can make changes that are independent of any changes I have made, save them, and share them without, say, deleting everything I had ready for class.

![Forking a repository](../fig/githubfork.png)

Now, we will clone the repository in the way that we practiced last time.

What I want you to do now is add one line to the reference.md. Describe one command we've used so far. Add, commit and push your changes. 

We will now make a pull request.

![Make a pull request](../fig/GitMenu.png)

A pull request doesn't insert your code into mine. It asks me to insert your code into mine (hence, the request). I will now inspect this contribution, and "merge" it, or add it into my code.

Now, the rest of you will "pull" these changes to your copy of the repository. To "pull" changes refers to accepting changes into your codebase. To do this, we need to tell git how to find my copy of the repository:

```
git remote add upstream https://github.com/Paleantology/SELUGandT
```

Now, we may pull any changes from that repository.

```
git pull upstream master
```

You now have all the changes I just merged.

Do any of you have conflicts? 

You might! We all added text on the same line! Git will let you know if a change will overwrite your work. We will now resolve this conflict, add and commit.

## If I can't use git on LONI, how can I transfer files to the cluster?

We, regrettably and in a decision I will whine about more, cannot use GitHub via the cluster. Instead, what we will do is move files to the cluster via the command secure copy. 

First, move out of the class repository by using the cd command.

Next, we will copy the repository to the cluster.

```
scp -r SELUGAndT amwright@qb2.loni.edu:~/
```





