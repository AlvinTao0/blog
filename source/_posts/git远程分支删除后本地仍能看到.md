---
title: git远程分支删除后,git branch -a 仍能看到
date: 2017-11-01 11:28:44
tags: git
---
有时候开发新功能新建一个分支，当这个功能上线后，我们通常会把这个分支删除掉，可是在本地git branch -a查看分支时，仍然能看到这些分支：
```
taojindeMacBook-Pro:myProject taojin$ git branch -a
  coupon
* master
  new-assets
  new-assets-test
  newcoupon
  receive-coupon
  remotes/origin/HEAD -> origin/master
  remotes/origin/bankLimit
  remotes/origin/changecard
  remotes/origin/coupon
  remotes/origin/dividend
  remotes/origin/fund
  remotes/origin/fund-new
  remotes/origin/master
  remotes/origin/new-assets
  remotes/origin/newapp
  remotes/origin/newcoupon
  remotes/origin/receive-coupon
  remotes/origin/remind
  remotes/origin/risk
  remotes/origin/sass
  remotes/origin/scrollLeft
  remotes/origin/smallke
  remotes/origin/theme
  ```
  我们可以用`git remote show origin`来查看分支的对应关系：
  ```
  taojindeMacBook-Pro:myProject taojin$ git remote show origin
* remote origin
  Fetch URL: git@git.n.xxx.com:myProject.git
  Push  URL: git@git.n.xxx.com:myProject.git
  HEAD branch: master
  Remote branches:
    coupon                         tracked
    master                         tracked
    new-assets                     tracked
    newcoupon                      tracked
    receive-coupon                 tracked
    refs/remotes/origin/bankLimit  stale (use 'git remote prune' to remove)
    refs/remotes/origin/changecard stale (use 'git remote prune' to remove)
    refs/remotes/origin/dividend   stale (use 'git remote prune' to remove)
    refs/remotes/origin/fund       stale (use 'git remote prune' to remove)
    refs/remotes/origin/fund-new   stale (use 'git remote prune' to remove)
    refs/remotes/origin/newapp     stale (use 'git remote prune' to remove)
    refs/remotes/origin/remind     stale (use 'git remote prune' to remove)
    refs/remotes/origin/risk       stale (use 'git remote prune' to remove)
    refs/remotes/origin/sass       stale (use 'git remote prune' to remove)
    refs/remotes/origin/scrollLeft stale (use 'git remote prune' to remove)
    refs/remotes/origin/smallke    stale (use 'git remote prune' to remove)
    refs/remotes/origin/theme      stale (use 'git remote prune' to remove)
  Local branches configured for 'git pull':
    coupon         merges with remote coupon
    master         merges with remote master
    new-assets     merges with remote new-assets
    newcoupon      merges with remote newcoupon
    receive-coupon merges with remote receive-coupon
  Local refs configured for 'git push':
    coupon         pushes to coupon         (up to date)
    master         pushes to master         (up to date)
    new-assets     pushes to new-assets     (local out of date)
    newcoupon      pushes to newcoupon      (up to date)
    receive-coupon pushes to receive-coupon (up to date)
  ```
  可以看到，有些远程已经删除掉的分支提示： stale (use 'git remote prune' to remove)
  接下来执行： `git remote prune origin` ,就可以把多余的分支删除掉；
  最后`git branch -a`查看结果：
  ```
  coupon
* master
  new-assets
  new-assets-test
  newcoupon
  receive-coupon
  remotes/origin/HEAD -> origin/master
  remotes/origin/coupon
  remotes/origin/master
  remotes/origin/new-assets
  remotes/origin/newcoupon
  remotes/origin/receive-coupon
  ```
