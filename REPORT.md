# Lab0：GitLab

张圣皓 25803050063

## 问答

### 多人协作经历

无。

### 为什么分成暂存和提交两步

一次修改可能混着几件事。暂存区让我先选出这次要交的部分，再用一次 commit 保存成一个有明确目的的版本。`git add` 保存的是当时的文件内容，之后继续改文件不会自动进入暂存区，所以提交前还要检查差异。

### git branch 和 git branch -a

`git branch` 列本地分支，`git branch -a` 还会列远程跟踪分支。远程跟踪分支是本地保存的记录，想更新它们还需要 `git fetch`。

## 两篇阅读材料

### Commit Message 规范

提交说明可以用“类型、范围、简短描述”组织，例如 `feat: add greeting`。标题写清改了什么，较复杂的改动再用正文说明原因；需要时注明不兼容变化或关联问题。统一格式能让提交历史更容易看，也方便生成变更日志。

### 语义化版本

版本号写成“主版本.次版本.修订版本”：不兼容的接口变化升主版本，向后兼容的新功能升次版本，兼容的修复升修订版本。已经发布的版本不应偷偷修改，而应发新版本；预发布后缀表示还不稳定。

Git 对我最有用的是能保留清楚的修改记录，出问题时知道哪里改过，也能恢复旧版本。分支让我先做实验，再把确认好的改动合回去；多人一起写时也容易分工和检查。

## 实验过程

【代码与历史已经恢复，等待个人模板仓库地址后完成远端对齐、实际操作截图和提交核验。】

## 参考

- [Commit message 和 Change log 编写指南](https://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)
- [语义化版本 2.0.0](https://semver.org/lang/zh-CN/)
- [git-add](https://git-scm.com/docs/git-add) 与 [git-branch](https://git-scm.com/docs/git-branch)
