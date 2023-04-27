# Git Flow

<!-- MarkdownTOC -->

- [引言](#%E5%BC%95%E8%A8%80)
    - [编写目的](#%E7%BC%96%E5%86%99%E7%9B%AE%E7%9A%84)
    - [背景](#%E8%83%8C%E6%99%AF)
    - [总则](#%E6%80%BB%E5%88%99)
    - [提交的准则](#%E6%8F%90%E4%BA%A4%E7%9A%84%E5%87%86%E5%88%99)
- [分支策略](#%E5%88%86%E6%94%AF%E7%AD%96%E7%95%A5)
    - [常设分支](#%E5%B8%B8%E8%AE%BE%E5%88%86%E6%94%AF)
    - [临时分支](#%E4%B8%B4%E6%97%B6%E5%88%86%E6%94%AF)
- [版本控制](#%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6)
- [Pull / Merge Request](#pull--merge-request)
- [Code Review](#code-review)
- [Commit](#commit)
    - [使用了哪些开源项目及作用](#%E4%BD%BF%E7%94%A8%E4%BA%86%E5%93%AA%E4%BA%9B%E5%BC%80%E6%BA%90%E9%A1%B9%E7%9B%AE%E5%8F%8A%E4%BD%9C%E7%94%A8)
    - [操作流程](#%E6%93%8D%E4%BD%9C%E6%B5%81%E7%A8%8B)
    - [commit 书写规范](#commit-%E4%B9%A6%E5%86%99%E8%A7%84%E8%8C%83)
- [附录](#%E9%99%84%E5%BD%95)
    - [附录 A：Git Command](#%E9%99%84%E5%BD%95-a%EF%BC%9Agit-command)

<!-- /MarkdownTOC -->

## 引言

最近在看一本书，未来简史。里面有一段描述人跟动物的差别：人可以很自然的融入不同的环境中去适应生活并参与劳动，但动物不行。

> 俗话说，没有规矩不成方圆。

现在，我想向大家隆重介绍，Git Workflow。

### 编写目的

通过规范化的流程，使得产品、开发与测试等各个部门更高效的协同工作。

通过规范化的流程使得产品高效稳定运行。

### 背景

在多组员，多项目等环境进行协同工作时，如果没有统一规范、统一流程，则会导致额外的工作量，甚至会做无用功。所以要减少版本冲突，减轻不必要的工作，就需要规范化的工作流程。

### 总则

统一使用 Git 作为版本控制的主要工具。

统一使用 Gitflow 流程管理控制版本。

### 提交的准则

除了源码相关的东西之外，其他build产生的东西（如：node_modules文件夹，.idea文件夹等），均不能提交进入源码仓库，添加到 .gitignore 文件中忽略掉。

撰写规范的提交说明。一份好的提交说明可以帮助协作者更轻松更有效地配合工作。

要严格按照我们指定的流程切换到指定分支，开发相应的功能。

## 分支策略

- **代码库的常设分支需要设置分支保护，临时分支最后都删除。**
- **临时分支合并到常用分支时，必须发起 Pull / Merge Requset ，并指定一个人 code review。**
- **远程临时分支由创建者起者追踪和维护， reviewer 负责删除。**
- 所有的开发和迭代尽量都在临时分支上。
- 开发记录、功能集成、测试历史由 develop 分支管理，正式发布记录由 master 分支管理。
- 发布和部署时，必须新建发布分支。`（发布分支基于 develop 分支）`
- 发布和部署完成后，必须将发布分支合并回 develop / master 分支，然后删除发布分支。
- 合并到 master 分支的代码必须打上 Tag。`（Tag：版本号、描述、日期）`

### 常设分支

**master: 主干分支**

master 分支上存放的应该是随时可供在生产环境中部署的代码（Production Ready state）。当开发活动告一段落，产生了一份新的可供部署的代码时，master 分支上的代码会被更新。同时，每一次更新，需要添加对应的版本号标签（TAG）。

**develop: 开发分支**

develop 分支是保存当前最新开发成果的分支。通常这个分支上的代码也是可进行每日夜间发布的代码（Nightly build）。因此这个分支有时也可以被称作“integration branch”。

**production: 生产分支**

production 分支是用来存放生产线上代码，以留备用。

### 临时分支

**功能分支**

功能分支，开发新功能都从 develop 分支出来，从 develop 分支上面分出来的。开发完成后，要再并入 develop。它的命名，可以采用 feature/* 的形式。

**发布分支**

发布分支，准备要 release 的版本，只修 bugs。从 develop 分支上面分出来，发布结束以后，必须合并进 develop 和 master 分支。它的命名，可以采用 release/* 的形式。

**修补分支**

修补分支，是指等不及 release 版本就必须马上修 master 赶上线的情况。它是从 master 分支上面分出来的。修补结束以后， 再合并进 master 和 develop 分支。它的命名，可以采用 hotfix/* 的形式。

## 版本控制

版本格式：主版本号（x）.次版本号(y).修订号(z)，版本号递增规则如下：

- 主版本号：当你做了不兼容的 API 修改。（通常指技术架构发生变化）
- 次版本号：当你做了向下兼容的功能性新增。（通常指需求迭代）
- 修订号：当你做了向下兼容的问题修正。（通常指线上bug，紧急需求）

先行版本号及版本编译信息可以加到“主版本号.次版本号.修订号”的后面，作为延伸。

参考 [Semantic Versioning 2.0.0](https://semver.org/)

## Pull / Merge Request

代码合并到 master / develop 分支：

1. 将需要合并到 master / develop 的分支 push 到 gitlab。
2. 进入工程 -> merge request -> create new merge request
3. 选择源分支、目标分支，确定。
4. review 负责人进入 merge request，确认没有问题之后选择 Auto Merge（或者手动在本地合并之后再 push 到 gitlab），并关闭这个 merge request，完成。
5. 如果发现问题那么在有问题的行下注释，并提醒 request 的发起人及时修改。
6. 删除本地临时分支，本地 master / develop 更新到最新状态。

## Code Review

- 提交 Pull / Merge Request 时， Commit 和 Message 要足够清晰详细。
切记，如果一次提交的内容包含很多 Commit，请不要使用自动生成的描述。
请用简短且足够说明问题的语言（理想是控制在3句话之内）来描述：

> 你改动了什么，解决了什么问题，需要代码审查的人留意那些影响比较大的改动。特别需要留意，如果对基础、公共的组件进行了改动，一定要另起一行特别说明。

- 审核人员邀请原则：项目参与人员 & 团队同事 & 团队 Leader。**（对项目足够了解，对项目足够了解，对项目足够了解，重要的事情说三遍）**；
- 评论中至少出现一个 lgtm 且保证代码评审通过之后 Pull / Merge Request 才可以被合并；`（注：lgtm 即 looks good to me 的缩写）`

> lgtm 需要配置插件实现功能。

## Commit

### 使用了哪些开源项目及作用

- Commitizen
> 一个撰写合格 Commit message 的工具。
- validate-commit-msg
> 用于检查 Node 项目的 Commit message 是否符合格式。
- standard-version
> 用于依据 Commit message 生成 changelog 以及版本发布。

### 操作流程

1.拉取最新代码。
2.在**本地分支**开发，开发完成之后将该分支 push 到远程仓库。
3.远程仓库上审核代码。通过之后，将分支合并到仓库主干，并清理分支。

> note: commit 内容也作为审核的一部分, 要求精准，简练，完整。对于改动较大，功能复杂的代码修改，尽量使用中文。

### commit 书写规范

标准参考 [Contributing to AngularJS](https://github.com/angular/angular.js/blob/master/CONTRIBUTING.md)

    <type>(<scope>): <subject>
    <BLANK LINE>
    <body>
    <BLANK LINE>
    <footer>

**type 代表某次提交的类型，比如是修复一个 bug 还是增加一个新的 feature。所有的 type 类型如下：**

- feat： 新增feature
- fix: 修复bug
- docs: 仅仅修改了文档，比如README, CHANGELOG, CONTRIBUTE等等
- style: 仅仅修改了空格、格式缩进、都好等等，不改变代码逻辑
- refactor: 代码重构，没有加新功能或者修复bug
- perf: 优化相关，比如提升性能、体验
- test: 测试用例，包括单元测试、集成测试等
- chore: 改变构建流程、或者增加依赖库、工具等
- revert: 回滚到上一个版本

**scope 表示改动部分的受影响范围:**

- user 用户
- pay 支付
- product 产品
- article 文章
- core 核心
- router 路由
- api 接口
- doc 文档
- upgrade 升级计划

> note: 如果无法确认影响范围，填写修改的文件。e.g. examples.js

**subject 主题，包含对变更的简明描述：**

- 以动词开头，使用第一人称现在时："change"（改变 动作） 不是 "changed"（改变 过去式） 也不是 "changes"（改变 三单形式）
- 不要大写第一个字母
- 最后没有点（。）

**常用表述语：**

- add 添加
- change 改变
- modify 修改
- update 更新
- remove 移动
- delete 删除

**body 主体**

- 使用第一人称现在时，比如使用："change"（改变 动作） 不是 "changed"（改变 过去式） 也不是 "changes"（改变三单形式） 。
- 正文应该包括变化的动机，并与之前的行为进行对比。

**footer 页脚**

- 如果当前 commit 针对某个issue，那么可以在 Footer 部分关闭这个 issue 。
- 如果当前代码与上一个版本不兼容，则 Footer 部分以 BREAKING CHANGE 开头，后面是对变动的描述、以及变动理由和迁移方法。

页脚应包含对问题的结束引用（如果有）。请参照以下格式：

- close 关闭（动作）
- closes 关闭（三单形式）
- closed 关闭（过去式）
- fix 修理（动作）
- fixes 修理（三单形式）
- fixed 修理（过去式）
- resolve 解决（动作）
- resolves 解决（三单形式）
- resolved 解决（过去式）

格式要求：

    # 标题行：50个字符以内，描述主要变更内容
    #
    # 主体内容：更详细的说明文本，建议72个字符以内。 需要描述的信息包括:
    #
    # * 为什么这个变更是必须的? 它可能是用来修复一个bug，增加一个feature，提升性能、可靠性、稳定性等等
    # * 他如何解决这个问题? 具体描述解决问题的步骤
    # * 是否存在副作用、风险?
    #
    # 尾部：如果需要的化可以添加一个链接到issue地址或者其它文档，或者关闭某个issue。

示例：

    feat(ALL): Add Projet

    前端 H5 组件库

Git command:

    git add -A

    git cz # 凡是用到git commit命令，一律改为使用 git cz

## 附录

### 附录 A：Git Command

**git checkout**

    # 放弃在 file.txt 的修改，恢复成未修改时的样子
    git checkout file.txt
    # 当前目录所有修改的文件，恢复成未修改时的样子
    git checkout .
    # 创建新的分支，并切换过去
    git checkout -b branchname

**git tag**

    # 创建一个带版本有描述的 tag
    git tag -a v0.1.0 -m 'commit'
    # 覆盖该版本已有 v0.1.0
    git tag -f v0.1.0
    # 推送服务器
    git push origin --tags
    # 服务器已有 v0.1.0，强制推到服务器
    git push origin -f v0.1.0
    # 服务器获取刚刚的 v0.1.0
    git fetch --tag
    # 删除本地版本
    git tag -d v0.1.0
    # 删除服务器上的tag
    git push origin :v0.1.0

**git merge**

    # 不使用 fast-forward 方式合并，保留分支的 commit 历史
    git merge --no-ff develop
    # 使用 squash 方式合并，把多次分支commit历史压缩为一次
    git merge --squash develop

**git reset**

    # 回退指定的 commit
    git reset 0c5602affd27d2224d151284bd1c6e033fd9023f
    # 回退上次修改
    git reset --hard

**git flow**

    # git flow 初始化
    git flow init
    # 创建一个新的 feature 分支
    git flow feature start name
    # 将 feature 分支推送到远程仓库
    git flow feature publish name
    # 当特性开发完毕，需要将此分支合并到 develop 分支
    git flow feature finish name
