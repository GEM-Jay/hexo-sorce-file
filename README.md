# hexo-sorce-file

This repository is hexo blog's file ,author can use this to rebuild his blog

**Git commands:**

**First, you should new a empty folder.**

1.**git init** : Create a git repository then you will get a file named ".git".

**Creat some files/folders in this folder.**

2.**git add .** . :Add all files in this folder to the temporary repository.However, this command does not log deletions, so the author believes that this command should be used sparingly.

3.**git add --all** : Also add all files in this folder to the temporary repository.but this command will log deletions.

4.**git add xxxx.xx** :Add the specified file in this folder to the temporary repository.

5.**git commit -m "###"** ：Commit your files from the temporary repository to the git repository. "###" represents your comments about this commit.

6.**git branch -M main** : Commit your files to the branch named "main".

7.**git remote add remote_repository_name_youwant URL** : Connect your remote repository, such as GitHub. Usually the name is **"origin"** and you can get the URL form github.

8.**git push remote_repository_name_youset main** : Push the local repository into your remote repository. Actually, after the remote_repository_name_youset like "origin", the command should be written as: "\<local_repository\>:\<remote_repository\>", but since the local_repository and remote_repository are the same here, the remote_repository could be omitted.

9.**git branch** : Check all of the local branches you have.

10.**git branch -a** : Please check all of the branches you have, including both local and remote branches.

11.**git branch new_branch_name** : Create a new branch, but you won't switch to it.

12.**git checkout exist_branch_name** : Switch from the current branch to this eixst branch.

13.**git checkout -b new_branch_name** : : Create a new branch, and you'll switch to it. This command equal to **git branch new_branch_name + git checkout new_branch_name** .

14.**git merge exist_branch_name** : Merge the new branch into the current branch.

15.**git branch -D exist_local_branch_name** : Delete the local branch.

16.**git branch remote_repository_name_youset --delete exist_remote_branch_name** :  Delete the remote branch.

17.**git status** : Show the current local branch name. | Show the files in the local working directory that are not added to the staging area. | Show the changes in the temporary repository.|Show the difference between local repository and remote repository.

18.**git ls-tree -r --name-only HEAD** ：This command lists the file tree structure for a specific commit (or the current branch).
    **"-r"** means recursively lists all files, including those in subdirectories.
    **"--name-only"** means shows only the filenames (without additional information).
    **"HEAD"** meansrefers to the latest commit on the current branch. You can replace it with another branch name or commit hash if you want to see a different commit.
