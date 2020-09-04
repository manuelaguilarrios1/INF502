Question 1: Git branch gives you the branches there are and which branch you are currently in which is denoted by the *
Git checkout branch shows the branch you are currently in. 
Git log --decorate displays all commits, authors, dates, and tags of that branch.
```
>git branch
* master
  math
  
>git checkout math
Switched to branch 'math'

>git log --decorate
commit e3c629dd524712aedea96d7dbaad1c50d12b5b5e (HEAD -> math)
Author: Igor Steinmacher <igorsteinmacher@gmail.com>
Date:   Wed Aug 14 23:13:48 2019 -0700

    Adding some more knowledge to the function

commit 654b490a181dedf82dd6deda5f9848d6cca05918
Author: Igor Steinmacher <igorsteinmacher@gmail.com>
Date:   Wed Aug 14 23:12:14 2019 -0700

    Added a draft of A.py

commit 2dfb02c3f9383d6c3b2695c99e175d8b85f594a1
Author: Igor Steinmacher <igorsteinmacher@gmail.com>
Date:   Wed Aug 14 23:08:47 2019 -0700

     Creating all files (all empty)
```
Question 2: This lists all references, commits, authors, dates, tags, and branches
```
>git log --graph --all
* commit 18931d12a8be7cac049b73c6bc8136e9482f3371 (HEAD -> master)
| Author: Igor Steinmacher <igorsteinmacher@gmail.com>
| Date:   Wed Aug 14 23:15:28 2019 -0700
|
|     Making a small change here
|
| * commit e3c629dd524712aedea96d7dbaad1c50d12b5b5e (math)
|/  Author: Igor Steinmacher <igorsteinmacher@gmail.com>
|   Date:   Wed Aug 14 23:13:48 2019 -0700
|
|       Adding some more knowledge to the function
|
* commit 654b490a181dedf82dd6deda5f9848d6cca05918
| Author: Igor Steinmacher <igorsteinmacher@gmail.com>
| Date:   Wed Aug 14 23:12:14 2019 -0700
|
|     Added a draft of A.py
|
* commit 2dfb02c3f9383d6c3b2695c99e175d8b85f594a1
  Author: Igor Steinmacher <igorsteinmacher@gmail.com>
  Date:   Wed Aug 14 23:08:47 2019 -0700

       Creating all files (all empty)
```
Question 3: This lists the differences in the branches. What is same is in white, what is missing is in red denoted with a '-' in front of the missing line, and what is the missing in the other file is in green with a '+' in front of said line. 
```
>git diff master
diff --git a/A.py b/A.py
index dc1e9bd..0afa98c 100644
--- a/A.py
+++ b/A.py
@@ -1,3 +1,11 @@
 #this is just an example, do not freak out
 def calculate_this(operator, num1, num2):
-   print 'my knowledge is limited'
+   if operator == 'sum':
+      print num1 + num2
+      print 'That was easy buddy'
+   else:
+      if operator == 'subtraction':
+         print num1 - num2
+         print 'I could handle that...'
+      else:
+         print 'my knowledge is limited'
diff --git a/B.py b/B.py
index c63f94c..e69de29 100644
--- a/B.py
+++ b/B.py
@@ -1 +0,0 @@
-# Another file that will receive a line of code... at least.
```
Question 4: First we merge the master from the math, then we switch to the master and merge math to the master. I realized I made a mistake and that you should just start in the master to merge the math into it. 
```
>git merge master
Merge made by the 'recursive' strategy.
 B.py | 1 +
 1 file changed, 1 insertion(+)
>git checkout master
Switched to branch 'master'
>git merge math
Updating 18931d1..aea1f5a
Fast-forward
 A.py | 10 +++++++++-
 1 file changed, 9 insertions(+), 1 deletion(-)
```
Question 5: First I deleted the math branch, then I created a new math branch and switched into it
```
>git branch -d math
Deleted branch math (was aea1f5a).
>git checkout -b math
Switched to a new branch 'math'
```
Question 6: I used notepad to add those parts to the B.py file
```
>notepad B.py
```
Question 7: I used the following command to commit the change to the B.py file, then used git log to see the changes
```
git commit B.py -m "Adding question 6 to B.py in branch math"
[math 40220e6] Adding question 6 to B.py in branch math
 1 file changed, 2 insertions(+)
 >git log
commit 40220e698607a2851605538ed9c4014c84555668 (HEAD -> math)
Author: Manuel Aguilar <maa778@nau.edu>
Date:   Fri Sep 4 09:26:50 2020 -0700

    Adding question 6 to B.py in branch math

commit aea1f5a48bd7c9e7f14038b5347fef59f55760ce (master)
Merge: e3c629d 18931d1
Author: Manuel Aguilar <maa778@nau.edu>
Date:   Thu Sep 3 16:21:09 2020 -0700

    Merge branch 'master' into math

commit 18931d12a8be7cac049b73c6bc8136e9482f3371
Author: Igor Steinmacher <igorsteinmacher@gmail.com>
Date:   Wed Aug 14 23:15:28 2019 -0700

    Making a small change here

commit e3c629dd524712aedea96d7dbaad1c50d12b5b5e
Author: Igor Steinmacher <igorsteinmacher@gmail.com>
Date:   Wed Aug 14 23:13:48 2019 -0700

    Adding some more knowledge to the function

commit 654b490a181dedf82dd6deda5f9848d6cca05918
Author: Igor Steinmacher <igorsteinmacher@gmail.com>
Date:   Wed Aug 14 23:12:14 2019 -0700

    Added a draft of A.py

commit 2dfb02c3f9383d6c3b2695c99e175d8b85f594a1
Author: Igor Steinmacher <igorsteinmacher@gmail.com>
Date:   Wed Aug 14 23:08:47 2019 -0700

     Creating all files (all empty)

```
Question 8: Used git checkout branch_name to switch to master and then used notepad B.py to add the print "hello world!" line, and I used git commit and git log to commit my changes and see it.
```
>notepad B.py
>git commit B.py -m "Adding question 6 to B.py in branch math"
[math 40220e6] Adding question 6 to B.py in branch math
 1 file changed, 2 insertions(+)
>git log
commit 40220e698607a2851605538ed9c4014c84555668 (HEAD -> math)
Author: Manuel Aguilar <maa778@nau.edu>
Date:   Fri Sep 4 09:26:50 2020 -0700

    Adding question 6 to B.py in branch math

commit aea1f5a48bd7c9e7f14038b5347fef59f55760ce (master)
Merge: e3c629d 18931d1
Author: Manuel Aguilar <maa778@nau.edu>
Date:   Thu Sep 3 16:21:09 2020 -0700

    Merge branch 'master' into math

commit 18931d12a8be7cac049b73c6bc8136e9482f3371
Author: Igor Steinmacher <igorsteinmacher@gmail.com>
Date:   Wed Aug 14 23:15:28 2019 -0700

    Making a small change here

commit e3c629dd524712aedea96d7dbaad1c50d12b5b5e
Author: Igor Steinmacher <igorsteinmacher@gmail.com>
Date:   Wed Aug 14 23:13:48 2019 -0700

    Adding some more knowledge to the function

commit 654b490a181dedf82dd6deda5f9848d6cca05918
Author: Igor Steinmacher <igorsteinmacher@gmail.com>
Date:   Wed Aug 14 23:12:14 2019 -0700

    Added a draft of A.py

commit 2dfb02c3f9383d6c3b2695c99e175d8b85f594a1
Author: Igor Steinmacher <igorsteinmacher@gmail.com>
Date:   Wed Aug 14 23:08:47 2019 -0700

     Creating all files (all empty)
```
Question 9: You get a conflict message.
```
>git merge math
Auto-merging B.py
CONFLICT (content): Merge conflict in B.py
Automatic merge failed; fix conflicts and then commit the result.
```
Question 10: Used a simple git merge --abort to abort the command.
```
>git merge --abort
```
Question 11: To manually merge the two branches, I first see the differences in the files and then add the lines that are missing. After the file is changed, you commit the changes. 
```
>git diff math
diff --git a/B.py b/B.py
index 66ca0b7..2c7969a 100644
--- a/B.py
+++ b/B.py
@@ -1,3 +1,2 @@
 # Another file that will receive a line of code... at least.
-print 'I know math, look:'
-print 2+2
\ No newline at end of file
+print 'hello world!'
\ No newline at end of file

>notepad B.py
>git commit B.py -m "Added missing math branch lines"
[master 8035639] Added missing math branch lines
 1 file changed, 2 insertions(+)

```
Question 12: Use git diff, then edit the file to remove the unnecessary lines
```
>git diff math
diff --git a/B.py b/B.py
index 66ca0b7..ea734ee 100644
--- a/B.py
+++ b/B.py
@@ -1,3 +1,9 @@
 # Another file that will receive a line of code... at least.
+<<<<<<< HEAD
+print 'I know, look:'
+print 2+2
+print 'hello world!'
+=======
 print 'I know math, look:'
-print 2+2
\ No newline at end of file
+print 2+2
+>>>>>>> math
