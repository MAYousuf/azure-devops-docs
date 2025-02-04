---
title: Undo changes in your Git repo
titleSuffix: Azure Repos
description: Use git reset or revert to undo changes in Git repo 
ms.technology: devops-code-git 
ms.topic: tutorial
ms.date: 09/28/2021
monikerRange: '<= azure-devops'
---

# Undo changes

[!INCLUDE [temp](../includes/version-tfs-2015-cloud.md)]
[!INCLUDE [temp](../includes/version-vs-2015-vs-2019.md)]

When undoing changes in Git, first decide what type of changes you are looking to undo. These changes fall into three categories:

- Discard uncommitted changes to a file, bringing the file back to the version in the last commit.
- Reset your local branch to a previous commit.
- Revert changes pushed to a remote branch and shared with others.

If you just need to make small updates such as to fix a typo or small problem introduced in your last commit, consider [amending your previous commit](commits.md) or fixing the change
in a new commit instead of any of these other steps. 

In this tutorial you learn how to:

> [!div class="checklist"]
> * Discard uncommitted changes to a single file
> * Revert changes in shared commits
> * Reset a branch to a previous state


## Discard uncommitted changes to a single file

Restore file contents back to a known good version, removing unwanted changes.

> [!WARNING]
> These commands will overwrite your existing file changes. If you think you might want these changes later, consider [stashing](howto.yml#faq) them instead.

#### [Visual Studio](#tab/visual-studio/)

[!INCLUDE [temp](includes/note-new-git-tool.md)]  

Visual Studio 2015 &amp; 2017

1. Open up the **Changes** view in Team Explorer.
2. Under the **Changes** section, find the file that you want to restore to the previous version. If your change is staged, remove it from the **Staged Changes** section by right-clicking and selecting **Unstage**.
3. Right-click that file and select **Undo Changes**.

    ![Reset a single file with Git in Visual Studio](media/vs_reset_single_file.gif)

#### [Command Line](#tab/command-line/)
You can use the `checkout` command and give it the filename(s) to change. Use wildcards for undoing changes to multiple files.

> [!div class="tabbedCodeSnippets"]
```Git CLI
> git checkout approuter.js
```

You can revert the file to the version in a specific commit by providing the commit ID:

> [!div class="tabbedCodeSnippets"]
```Git CLI
> git checkout 38035acd2 approuter.js
```

This differs from the earlier use of the `checkout` command used to swap to a different [branch](./create-branch.md). 
Git will tell you if it is changing a file or swapping between branches in the output, and complain if it's not clear which one you are trying to do.

* * *
<a name="revert"></a>

## Revert changes in shared commits

Use `revert` to undo the changes made in your commits pushed to shared branches. The `revert` command creates a new commit that undoes the changes on a previous commit. No history is rewritten
in a `revert`, making it safe to use when working with others.

# [Visual Studio](#tab/visual-studio)

[!INCLUDE [temp](includes/note-new-git-tool.md)]

Open up the **Changes** view in Team Explorer. Select **Actions** and choose **View History** from the drop-down. In the history window that appears, right-click the commit to undo and
select **Revert** from the context menu.

![Revert changes from Visual Studio.](media/vs_revert_changes.png)

# [Command Line](#tab/command-line)

> [!div class="tabbedCodeSnippets"]
```Git CLI
> git revert 8437fbaf
> git commit
```

These commands will undo the changes made in commit 8437fbaf and create a new commit on the branch. The original commit at `commit_id` is still in the Git history.
`Revert` is flexible but it requires a branch history and commit identifiers to use. Review your [history](review-history.md) to find the commits you want to revert. 

---

## Reset a branch to a previous state

Use `reset` to bring a branch in your local repository back to the contents of a previous commit. The most common use of the `reset` command is 
to simply discard all changed files since the last commit and return the files to the state they were in at the most recent commit.

> [!WARNING]
> Don't use `reset` on branches shared with others. Use [revert](undo.md#revert) instead.

#### [Visual Studio](#tab/visual-studio/)
1. Open up the **Changes** view in Team Explorer. 
2. Select **Actions** and choose **View History** from the drop-down. 
3. In the history window that appears, right-click the commit to reset the repo to and select **Reset** from the context menu. 
4. Choose **Reset and delete changes...**.

    ![Reset a branch from Visual Studio](media/vs_reset_branch.png)

#### [Command Line](#tab/command-line/)

> [!div class="tabbedCodeSnippets"]
```Git CLI
> git reset --hard HEAD 
```

The `--hard` part of the command tells Git to reset the files to the state of the previous commit and discard any staged changes. 
The `HEAD` argument tells Git to reset the local repository to the most recent commit. If you want to reset the repo to a different commit, provide the ID instead of HEAD.

* * *
* 
A `reset` affects all files in the current branch on the repository, not just those in your current directory. `Reset` only discards changes that haven't 
been committed yet.




## Next steps

> [!div class="nextstepaction"]
> [Ignore files](ignore-files.md)