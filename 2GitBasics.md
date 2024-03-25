# Git 基础


## 在现有目录中初始化仓库

1. `git init` 该命令将创建一个名为 .git 的子目录

2. `git add` 来实现对指定文件的跟踪

3. `git commit` 提交， `git commit -m 'initial project version'`



## 克隆现有的仓库

`git clone https://github.com/libgit2/libgit2`


## 记录每次更新到仓库

![alt 文件的状态变化周期](./image/lifecycle.png)


## 检查当前文件状态
`git status`

干净状态：

    $ git status
    On branch master
    nothing to commit, working directory clean

创建一个新的 README 文件：

    $ echo 'My Project' > README
    $ git status
    On branch master
    Untracked files:
    (use "git add <file>..." to include in what will be committed)

        README

    nothing added to commit but untracked files present (use "git add" to track)


## 跟踪新文件

    $ git add README

再运行 git status，会看到 README 文件已被跟踪，并处于暂存状态：

    $ git status
    On branch master
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        new file:   README

只要在 Changes to be committed 这行下面的，就说明是已暂存状态

## 暂存已修改文件
现在我们来修改一个已被跟踪的文件。 如果你修改了一个名为 CONTRIBUTING.md 的已被跟踪的文件，然后运行 git status 命令，会看到下面内容：

    $ git status
    On branch master
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        new file:   README

    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md

要暂存这次修改的文件，需要运行 git add 命令。 这是个多功能命令：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。 将这个命令理解为“添加内容到下一次提交中”而不是“将一个文件添加到项目中”要更加合适。

    $ git add CONTRIBUTING.md
    $ git status
    On branch master
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        new file:   README
        modified:   CONTRIBUTING.md

## 状态简览

git status 命令的输出十分详细，但其用语有些繁琐。 如果你使用 git status -s 命令或 git status --short 命令，你将得到一种更为紧凑的格式输出。

    $ git status -s
    M README
    MM Rakefile
    A  lib/git.rb
    M  lib/simplegit.rb
    ?? LICENSE.txt

## 忽略文件
一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。

在这种情况下，我们可以创建一个名为 .gitignore 的文件，列出要忽略的文件模式

    # no .a files
    *.a

    # but do track lib.a, even though you're ignoring .a files above
    !lib.a

    # only ignore the TODO file in the current directory, not subdir/TODO
    /TODO

    # ignore all files in the build/ directory
    build/

    # ignore doc/notes.txt, but not doc/server/arch.txt
    doc/*.txt

    # ignore all .pdf files in the doc/ directory
    doc/**/*.pdf


## 查看已暂存和未暂存的修改
如果 `git status` 命令的输出对于你来说过于模糊，你想知道具体修改了什么地方，可以用 `git diff` 命令。

此命令比较的是工作目录中当前文件和暂存区域快照之间的差异， 也就是修改之后还没有暂存起来的变化内容。

若要查看已暂存的将要添加到下次提交里的内容，可以用 `git diff --cached` 命令。

## 提交更新
在此之前，请一定要确认还有什么修改过的或新建的文件还没有 git add 过，否则提交的时候不会记录这些还没暂存起来的变化。 

    $ git commit

你也可以在 commit 命令后添加 -m 选项，将提交信息与命令放在同一行

    $ git commit -m "Story 182: Fix benchmarks for speed"
    [master 463dc4f] Story 182: Fix benchmarks for speed
    2 files changed, 2 insertions(+)
    create mode 100644 README

提交后它会告诉你，当前是在哪个分支（master）提交的，本次提交的完整 SHA-1 校验和是什么（463dc4f），以及在本次提交中，有多少文件修订过，多少行添加和删改过。


## 跳过使用暂存区域
给 git commit 加上 -a 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 git add 步骤。


## 移除文件





























