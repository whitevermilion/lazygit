<!-- TOC GFM -->

* [吐槽时间：](#吐槽时间)
* [功能特性](#功能特性)
  - [暂存单行代码](#暂存单行代码)
  - [交互式变基](#交互式变基)
  - [拣选提交](#拣选提交)
  - [二分查找](#二分查找)
  - [清空工作区](#清空工作区)
  - [修改旧提交](#修改旧提交)
  - [过滤](#过滤)
  - [调用自定义命令](#调用自定义命令)
  - [工作树](#工作树)
  - [变基魔法（自定义补丁）](#变基魔法自定义补丁)
  - [从标记的基础提交进行变基](#从标记的基础提交进行变基)
  - [撤销](#撤销)
  - [提交图谱](#提交图谱)
  - [比较两个提交](#比较两个提交)
* [使用方法](#使用方法)
  - [快捷键绑定](#快捷键绑定)
  - [退出时切换目录](#退出时切换目录)
  - [撤销/重做](#撤销重做)
* [配置说明](#配置说明)
  - [自定义分页器](#自定义分页器)
  - [自定义命令](#自定义命令)
  - [Git flow 支持](#git-flow-支持)

<!-- /TOC -->

## 吐槽时间：

害

这话你可能听过无数遍了：git **功能强大**。但是，当所有操作都特么这么难用的时候，这种强大有什么用？

**交互式变基** 竟然需要你在编辑器里编辑一个该死的 TODO 文件？**你特么在逗我吗？**

想要**暂存文件的一部分**，你需要用一个命令行程序一步步检查每个代码块，如果某个代码块无法再分割但又包含你不想暂存的代码，你就得**手动编辑一个晦涩难懂的补丁文件**？**你特么在逗我吗？！**

有时候切换分支时，git 会要求你先储藏修改，结果等你切换完分支并取出储藏后才发现，**明明没有任何冲突，直接切换分支完全没问题**？**你特么一定是在逗我！**

如果你像我一样只是个普通人，厌倦了整天听别人说 git 多么强大，而它在你的日常生活中却只是个让你**蛋疼不已**的"强大"工具，那么 lazygit 可能就是为你准备的。

## 功能特性

### 暂存单行代码

在选中的行上按空格键进行暂存，或按 `v` 开始选择多行。你也可以按 `a` 选择当前整个代码块。

![stage_lines](../assets/demo/stage_lines-compressed.gif)

### 交互式变基

按 `i` 开始交互式变基。然后可以对任何 TODO 提交进行压缩（`s`）、修复（`f`）、删除（`d`）、编辑（`e`）、上移（`ctrl+k`）或下移（`ctrl+j`）操作，最后按 `m` 打开变基选项菜单并选择 `continue` 继续变基。

你也可以一次性执行这些操作（例如在提交上按 `s` 进行压缩），而无需显式启动变基。

此演示还使用了 shift+向下键 来选择要移动和修复的一系列提交。

![interactive_rebase](../assets/demo/interactive_rebase-compressed.gif)

### 拣选提交

在提交上按 `shift+c` 进行复制，然后按 `shift+v` 进行粘贴（拣选）。

![cherry_pick](../assets/demo/cherry_pick-compressed.gif)

### 二分查找

在提交视图中按 `b` 将提交标记为良好/有问题的，以开始 git 二分查找。

![bisect](../assets/demo/bisect-compressed.gif)

### 清空工作区

当你真的想要摆脱 `git status` 显示的所有内容时（是的，包括脏子模块），可以按 `shift+d` 打开重置选项菜单，然后选择"清空"选项。

![清空工作区](../assets/demo/nuke_working_tree-compressed.gif)

### 修改旧提交

在任何提交上按 `shift+a` 将使用当前暂存的更改来修改该提交（在后台运行交互式变基）。

![修改旧提交](../assets/demo/amend_old_commit-compressed.gif)

### 过滤

你可以使用 `/` 过滤视图。这里我们过滤分支视图，然后按 `enter` 查看其提交。

![filter](../assets/demo/filter-compressed.gif)

### 调用自定义命令

Lazygit 有一个非常灵活的[自定义命令系统](docs/Custom_Command_Keybindings.md)。在这个例子中，定义了一个模拟内置分支检出操作的自定义命令。

![custom_command](../assets/demo/custom_command-compressed.gif)

### 工作树

你可以创建工作树，以便同时处理多个分支，而无需在切换分支时进行储藏或创建 WIP 提交。在分支视图中按 `w` 从选定的分支创建工作树并切换到该工作树。

![从分支创建工作树](../assets/demo/worktree_create_from_branches-compressed.gif)

### 变基魔法（自定义补丁）

你可以从旧提交构建自定义补丁，然后从提交中移除补丁、拆分出新提交、将补丁反向应用到索引等等。

在这个例子中，我们有一个想要从旧提交中移除的冗余注释。我们在提交上按 `<enter>` 查看其文件，然后在文件上按 `<enter>` 聚焦到补丁，接着按 `<space>` 将注释行添加到我们的自定义补丁，然后按 `ctrl+p` 查看自定义补丁选项；选择从当前提交中移除补丁。

在 [Rebase magic Youtube 教程](https://youtu.be/4XaToVut_hs) 中了解更多。

![custom_patch](../assets/demo/custom_patch-compressed.gif)

### 从标记的基础提交进行变基

假设你正在一个功能分支上，该分支本身是从开发分支分出来的，而你决定更希望从主分支分出。你需要一种方法仅变基功能分支的提交。在这个演示中，我们检查开发分支上的最后一个提交，然后按 `shift+b` 将该提交标记为我们的基础提交，然后在主分支上按 `r` 变基到它，仅带入我们功能分支的提交。然后我们使用 `shift+p` 推送更改。

![变基到](../assets/demo/rebase_onto-compressed.gif)

### 撤销

你可以按 `z` 撤销上一个操作，按 `ctrl+z` 重做。这里我们删除了几个提交，然后撤销了这些操作。
撤销使用 reflog，这是特定于提交和分支的，因此我们无法撤销对工作树或储藏的更改。

[更多信息](/docs/Undoing.md)

![undo](../assets/demo/undo-compressed.gif)

### 提交图谱

在放大的窗口中查看提交图谱时（使用 `+` 和 `_` 循环切换屏幕模式），会显示提交图谱。颜色对应于提交作者，当你向下导航图谱时，所选提交的父提交会高亮显示。

![commit_graph](../assets/demo/commit_graph-compressed.gif)

### 比较两个提交

如果在提交（或分支/引用）上按 `shift+w`，将打开一个菜单，允许你标记该提交，以便将你选择的任何其他提交与之进行比较。选择第二个提交后，你将在主视图中看到差异，如果按 `<enter>`，你将看到差异的文件。你可以再次按 `shift+w` 查看差异菜单，以查看诸如反转差异方向或退出差异模式等选项。你也可以通过按 `<escape>` 退出差异模式。

![diff_commits](../assets/demo/diff_commits-compressed.gif)

## 使用方法

在 Git 仓库内的终端中调用 `lazygit` 命令。

```sh
$ lazygit
```

如果你需要，也可以通过 `echo "alias lg='lazygit'" >> ~/.zshrc` 命令（或你正在使用的其他 rc 文件）为此命令添加别名。

### 快捷键绑定

你可以在此处查看快捷键绑定列表[这里](/docs/keybindings)。

### 退出时切换目录

如果你在 lazygit 中切换了代码库，并希望退出 lazygit 时 shell 能自动切换到该代码库目录，请将以下内容添加到你的 `~/.zshrc`（或其他 rc 文件）中：

```bash
lg()
{
    export LAZYGIT_NEW_DIR_FILE=~/.lazygit/newdir

    lazygit "$@"

    if [ -f $LAZYGIT_NEW_DIR_FILE ]; then
            cd "$(cat $LAZYGIT_NEW_DIR_FILE)"
            rm -f $LAZYGIT_NEW_DIR_FILE > /dev/null
    fi
}
```

然后执行 `source ~/.zshrc`，从现在开始，当你调用 `lg` 并退出时，你将自动切换到在 lazygit 中所处的目录。如需覆盖此行为，你可以使用 `shift+Q` 而不是 `q` 来退出。

### 撤销/重做

请参阅[文档](/docs/Undoing.md)

## 配置说明

请查阅[配置文档](docs/Config.md)了解详细配置信息。

### 自定义分页器

参阅[相关文档](docs/Custom_Pagers.md)

### 自定义命令

如果 lazygit 缺少某个你需要的功能，你很可能会通过自定义命令来实现它！

请参阅[相关文档](docs/Custom_Command_Keybindings.md)

### Git flow 支持

如果你已安装 [Gitflow](https://github.com/nvie/gitflow)，Lazygit 支持其工作流。要了解 Gitflow 模型的工作原理，请查阅 Vincent Driessen 解释该模型的原始[文章](https://nvie.com/posts/a-successful-git-branching-model/)。若要在 Lazygit 中查看 Gitflow 选项，请在分支视图中按下 `i` 键。
