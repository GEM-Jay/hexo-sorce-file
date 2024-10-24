# hexo-sorce-file

This repository is hexo blog's file ,author can use this to rebuild his blog

**Git commands:**

**First, you should new a empty folder.**

1.**git init** : Create a git repository then you will get a file named ".git".

**Creat some files/folders in this folder.**

2.**git add** . :Add all files in this folder to the temporary repository.

3.**git add xxxx.xx** :Add the specified file in this folder to the temporary repository.

4.**git commit -m "###"** ：Commit your files from the temporary repository to the git repository. "###" represents your comments about this commit.

5.**git branch -M main** : Commit your files to the branch named "main".

6.**git remote add origin https://github.com/GEM-Jay/hexo-sorce-file.git** : Connect your remote repository, such as GitHub.

7.**git push origin main** : Push the local repository into your remote repository. Actually, after the "origin", the command should be written as: "\<local_repository\>:\<remote_repository\>", but since the local_repository and remote_repository are the same here, the remote_repository could be omitted.

8.**git branch** : Check all of the branches you have.

9.**git branch new_branch_name** : Create a new branch, but you won't switch to it.

10.**git checkout exist_branch_name** : Switch from the current branch to this eixst branch.

11.**git checkout -b new_branch_name** : : Create a new branch, and you'll switch to it.

12.**git merge exist_branch_name** : Merge the new branch into the current branch.

13.
