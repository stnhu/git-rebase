# 优化提交记录

在合并分支的操作中，要被合并的分支的 snapshot 如果比当前分支 snapshot 提前，合并后的历史记录就会呈一条直线；反之分支合并的操作就让提交记录产生支线。

在前面的分支合并操作中，可以使用 `git log --graph` 来查看提交历史。

在此基础上，我们如果想让提交历史呈一条直线，需要 experimental 分支的 snapshot 在 master 分支的基础上往前一步，这就是 rebase 操作。

```bash
$ git reset --hard HEAD~1  # 让 master 分支的 snapshot 回到合并 experimental 分支前

# 切换到 experimental 分支，进心 rebase 操作
$ git checkout experimental
$ git rebase master

# 遇到冲突，需要手动编辑文件
$ vim a.md

# 解决冲突后，结束 rebase 操作
$ git add a.md
$ git rebase --continue

# rebase 操作后，experimental 的 snapshot 就比 master 的 snapshot 提前一步
# 此时执行合并操作，提交记录就是一条直线

$ checkout master 
$ git merge experimental
```

> This operation works by going to the common ancestor of the two branches (the one you’re on and the one you’re rebasing onto), getting the diff introduced by each commit of the branch you’re on, saving those diffs to temporary files, resetting the current branch to the same commit as the branch you are rebasing onto, and finally applying each change in turn.
