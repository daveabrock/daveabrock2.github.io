---
date: "2019-09-30"
title: "How to rename a Git branch"
tags: [git, quick-tips]
---

As a developer for more than a decade, sometimes I wonder how it's taken me this long to do a thing. This morning, it was renaming a Git branch. Whatever the case, I finally found the need to do it this morning and am happy to share how easy it is.

# Rename locally

If you aren't already at the branch in question, check it out:

```bash
git checkout <my-branch-i-want-to-rename>
```

Now that your branch is checked out, rename it by using the `git branch` command:

```bash
git branch -m <new-name-of-my-branch>
```

# Push to remote, if needed

If you have pushed the old branch, you'll need to clean it up in the remote branch as well. To do this, delete the old branch and then push your newly-named branch to your repository:

```bash
git push origin --delete <my-branch-i-want-to-rename>
git push origin -u <new-name-of-my-branch
```

Mission accomplished! Take the rest of the day off—you deserve it.
