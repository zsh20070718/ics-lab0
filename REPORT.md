# Lab0：Git 与分支合并实验报告（待补齐后提交）

实验日期：2026-09-14。模板：[ICS-26Fall-FDU/GitLab](https://github.com/ICS-26Fall-FDU/GitLab)。

代码、分支提交、真实冲突和解决均已在本地完成，编译运行通过。本报告还缺个人经历与课程要求的截图；个人 GitHub 仓库尚未创建或指定，尚未推送、尚未在 eLearning 提交。以下日志是真实执行记录，不将文字日志冒充截图。

## 一、文档问题

### 多人协同开发经历

**待本人补充真实情况：是否有过多人开发经历？如果有，项目是什么、怎样分工、如何合并代码与沟通？如果没有，明确写没有，并描述计划如何使用分支与代码审查协作。**

### Git 为什么采用“暂存—提交”两步

工作目录里的变化往往并非同一个目的：修复一个错误时可能同时调整注释，或者还留有未完成的实验代码。暂存区允许先选出属于一个逻辑单元的改动，再形成一次提交；必要时还可以按文件或补丁片段选择。这样每个提交更容易解释、审查、回退和定位问题。

`git add` 把选定版本写入索引；`git commit` 用索引内容创建快照，并连接到父提交，而不是无条件保存此刻工作目录里的所有变化。在暂存后继续编辑文件，新的编辑不会自动进入刚刚暂存的版本。因此提交前同时看 `git diff` 和 `git diff --cached` 有助于核对提交范围。

### `git branch` 与 `git branch -a`

不带其它参数时，`git branch` 列出本地分支，并标出当前分支。`git branch -a` 除本地分支外还显示远程跟踪分支，如 `remotes/origin/main`。后者也是本地保存的远程状态记录；它不会因为“显示所有”就自动联网更新。需要更新记录时先执行 `git fetch`。仅显示远程跟踪分支可用 `git branch -r`。

参考：[Git 官方 git-branch 文档](https://git-scm.com/docs/git-branch)。

## 二、两篇阅读材料与理解

### Commit Message 规范

阅读：[Commit message 和 Change log 编写指南](https://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)。

文章介绍了按类型、范围和简短主题组织提交标题，并用正文说明动机、用尾部说明破坏性变化或关联问题的方法。统一格式可以帮助读者快速辨别功能、修复和文档修改，也方便整理变更日志。实践时不应只追求格式整齐：一次提交应尽量表达一个清楚的目的，描述实际改动，而非反复写“update”。本次使用 `feat:`、`merge:` 等可读前缀说明各阶段意图；具体前缀集合应服从项目约定。

### 语义化版本

阅读：[语义化版本 2.0.0](https://semver.org/lang/zh-CN/)。

语义化版本以 `主版本.次版本.修订版本` 表达公开接口的兼容性变化：不兼容变化提升主版本，向后兼容的新功能提升次版本，兼容的缺陷修复提升修订版本。已发布版本的内容不应悄悄改变，需要修改就发布新版本。预发布标识可以提示它尚未达到正式版本的稳定性。

Git 提交记录回答“代码如何变到现在”，发布版本回答“用户依赖的接口发生了什么变化”。两者配合能让开发者准确找到发布对应的代码，判断升级风险并追踪问题。学习 Git 的价值不只是把文件上传到网站，更在于让试验、协作和恢复有清晰且可检查的历史。

## 三、实验步骤与真实结果

环境：Windows 上现有 Git 2.53.0 和 GCC；所有变化限于本任务内的模板克隆，不修改全局 Git 配置。新提交使用本地实验作者 `Homework Agent`，未声称是模板作者的提交。

1. 克隆模板，模板基线为 `f67e080`。
2. 修改 `main.c` 的问候语，完成 TODO；提交 `a74535b`。
3. 从此处新建 `feature`，把同一行问候语改为描述分支隔离开发；提交 `48a54b6`。
4. 切回 `main`，把同一行改为描述稳定集成分支；提交 `766ef04`。
5. 在 `main` 执行 `git merge feature`，命令实际返回冲突，`git status --short` 显示 `UU main.c`。
6. 检查冲突标记，保留两条问候语作为两次 `printf`，删除冲突标记；暂存并创建合并提交 `9a85ac3`。
7. 用 `git rev-list --parents -n 1 HEAD` 验证合并提交有两个父提交；`git ls-files -u` 为空，说明没有未解决的索引冲突。
8. 用 `gcc -std=c11 -Wall -Wextra -Werror main.c` 编译到任务临时目录，编译退出码为 0；运行也返回 0，输出包含两个分支的语句。

两分支修改同一祖先版本中的同一行，而且内容不同，Git 无法自动判断应该保留哪一边，因而产生实际冲突。解决时可以保留一方，也可以像本次一样重新组织为同时保留双方有用信息；不能只是机械删除标记而不检查程序含义。

### 冲突记录

```text
$ git merge feature
Auto-merging main.c
CONFLICT (content): Merge conflict in main.c
Automatic merge failed; fix conflicts and then commit the result.

$ git status --short
UU main.c
```

完整记录：[evidence/git-session.txt](evidence/git-session.txt)。原始冲突文件：[evidence/conflict-main.c.txt](evidence/conflict-main.c.txt)。机器核验：[evidence/verification.json](evidence/verification.json)。

### 最终提交关系

```text
*   9a85ac3 merge: preserve both greetings and resolve the conflict
|\
| * 48a54b6 feat: describe isolated development on feature
* | 766ef04 feat: describe stable integration on main
|/
* a74535b feat: complete the introductory Git lab greeting
* f67e080 template baseline
```

### 程序输出

```text
Main branch: keep a stable integration point.
Feature branch: practice isolated development.
```

### 截图待补

课程要求提供“遇到并解决冲突”的截图。当前只有原始日志、冲突文件和可核验提交历史，没有桌面截图。可在自己打开的终端展示 `evidence/git-session.txt` 中的冲突段，以及 `git log --graph --oneline --all`、`git ls-files -u` 和程序运行结果，再截图加入此处。不要重新制造冲突或改写已经验证的提交历史。

## 四、提交前剩余事项

- 填写真实协作经历，加入要求的截图。
- 按课程要求从模板建立本人仓库，并确认目标 GitHub URL；不要把实验推送到教师的模板仓库。
- 将代码、报告、证据提交到本人仓库的 main 分支。eLearning 当前普通作业列表尚无 Lab0 提交入口；入口发布后提交本人仓库链接。
- 课程文档标注截止为 2026-09-30 23:59。
