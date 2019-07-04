---
layout: post
title: "Git Subtrees Workflow"
published: false
---

# Create two repositories

Create two repositories, one named `master` and the other `consumer`. on a hosted Git server (eg. GitHub/GitLab) and note the clone URLs.

Create the `master` repository locally and push it to it's clone URL.

```sh
export MASTER_REPO_CLONE_URL="ssh://git@gitlab.gds-gov.tech:30022/zephinzer/stmaster1.git";
mkdir repo-master;
cd repo-master;
git init;
git remote add origin ${MASTER_REPO_CLONE_URL};
echo 'this is the master repo' > ./README.md;
git add .;
git commit -m 'initial commit';
git push -u origin master;
cd ..;
```

Create the `consumer` repository locally and push it.

```sh
export CONSUMER_REPO_CLONE_URL="ssh://git@gitlab.gds-gov.tech:30022/zephinzer/stconsumer1.git";
mkdir repo-consumer;
cd repo-consumer;
git init;
git remote add origin ${CONSUMER_REPO_CLONE_URL};
echo 'this is the consumer repo' > ./README.md;
git add .;
git commit -m 'initial commit';
git push -u origin master;
cd ..;
```

# Add re-usable content to `master`

We'll assume that content we wish to re-use from `master` lies in a directory named `reuse-me`.

Create the `reuse-me` directory and add a file to it.

```sh
cd repo-master;
mkdir reuse-me;
echo 're-use me' > reuse-me/a-file.txt;
git add .;
git commit -m 'added reusable content';
git push -u origin master;
cd ..;
```

# Create a sub-tree in `master`

We'll now use the `subtree` sub-command to spin off a new branch named `reusable` from the `reuse-me` directory and push the branch to the remote source control.

```sh
cd repo-master;
git subtree split --prefix=reuse-me --branch reusable;
git checkout reusable;
git push origin reusable;
git checkout master;
cd ..
```

The `split` sub-command creates a new branch (named `reusable`) containing only the contents of the `reuse-me` directory (indicated by `--prefix`). To prove this to yourself, navigate into the `repo-master` directory and run:

```sh
git checkout reusable;
```

You should see only the `a-file.txt`.

Let's go back to the `master` branch where the magic will happen:

```sh
git checkout master;
```

Before continuing, navigate out to the directory containing both directories `repo-master` and `repo-consumer`.

# Clone the sub-tree to `consumer`

Now that `repo-master` contains the re-usable content in a branch that's pushed to the remote source control, we can demonstrate pulling it into the `repo-consumer` repository. To do this, we will first need to add the remote of `repo-master` into `repo-consumer` and merge the work done in the `resuable` branch into a directory within `repo-consumer` named `from-master`. This looks like:

```sh
export MASTER_REPO_CLONE_URL="ssh://git@gitlab.gds-gov.tech:30022/zephinzer/stmaster.git";
cd repo-consumer;
git remote add remote-master ${MASTER_REPO_CLONE_URL};
git fetch remote-master;
git subtree add --squash --prefix=from-master remote-master reusable;
git push origin master;
cd ..;
```

After the commands have completed running, you should be able to observe a `from-master` directory in `repo-consumer` containing the same contents as the `reusable` branch from `repo-master`.

# Update a file from `repo-master`, pull updates into `repo-consumer`

Assuming we'd like to work only with `repo-master`, we'd like these changes to be propagated to `repo-consumer`.

We proceed by updating a file in the `repo-master`:

```sh
cd repo-master;
echo 'i was updated' >> reuse-me/a-file.txt;
git add .;
git commit -m 'updated master';
git subtree split --prefix=reuse-me --branch reusable;
git checkout reusable;
git push origin reusable;
git checkout master;
cd ..;
```

Next, we pull these changes in to `repo-consumer`:

```sh
cd repo-consumer;
git subtree pull --squash --prefix=from-master remote-master reusable;
cd ..;
```

# Update a file in `repo-consumer`, merge updates into `repo-master`

Assuming we'd like to work only with `repo-consumer`, we'd like these changes to be propagated to `repo-master`.

We proceed by updating a file in the `repo-consumer`:

```sh
cd repo-consumer
echo 'updated again' >> from-master/a-file.txt;
git add .;
git commit -m 'updated consumer';
git subtree push --prefix=from-master remote-master reusable;
cd ..;
```

Next, we pull these changes into `repo-master`:

```sh
cd repo-master;
git checkout reusable;
git pull origin reusable;
git checkout master;
git pull -s subtree origin reusable
```
