# 只是本地使用


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
### 移除已暂存的文件
要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。

可以用 `git rm` 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。

如果只是简单地从工作目录中手工删除文件，运行 git status 时就会在 “Changes not staged for commit” 部分（也就是 未暂存清单）看到：

    $ rm PROJECTS.md
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes not staged for commit:
    (use "git add/rm <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

            deleted:    PROJECTS.md

    no changes added to commit (use "git add" and/or "git commit -a")

然后再运行 git rm 记录此次移除文件的操作：

    $ git rm PROJECTS.md
    rm 'PROJECTS.md'
    $ git status
    On branch master
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        deleted:    PROJECTS.md

如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f

### 移除Git仓库中的文件
我们想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中。 换句话说，你想让文件保留在磁盘，但是并不想让 Git 继续跟踪。

    $ git rm --cached README


## 移动文件
要在 Git 中对文件改名，可以这么做：

    $ git mv file_from file_to

其实，运行 git mv 就相当于运行了下面三条命令：

    $ mv README.md README
    $ git rm README.md
    $ git add README

如此分开操作，Git 也会意识到这是一次改名，所以不管何种方式结果都一样。两者唯一的区别是，mv 是一条命令而另一种方式需要三条命令，直接用 git mv 轻便得多。


## 查看提交历史

    git log


## 撤消操作

有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 此时，可以运行带有 --amend 选项的提交命令尝试重新提交：

    $ git commit --amend

这个命令会将暂存区中的文件提交。 如果自上次提交以来你还未做任何修改（例如，在上次提交后马上执行了此命令），那么快照会保持不变，而你所修改的只是提交信息。

    $ git commit -m 'initial commit'
    $ git add forgotten_file
    $ git commit --amend

最终你只会有一个提交 - 第二次提交将代替第一次提交的结果。


## 取消暂存的文件

例如，你已经修改了两个文件并且想要将它们作为两次独立的修改提交，但是却意外地输入了 git add * 暂存了它们两个。 如何只取消暂存两个中的一个呢？ git status 命令提示了你：

    $ git add *
    $ git status
    On branch master
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        renamed:    README.md -> README
        modified:   CONTRIBUTING.md

提示使用 git reset HEAD <file>... 来取消暂存。 所以，我们可以这样来取消暂存 CONTRIBUTING.md 文件：

    $ git reset HEAD CONTRIBUTING.md
    Unstaged changes after reset:
    M	CONTRIBUTING.md
    $ git status
    On branch master
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        renamed:    README.md -> README

    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md

这个命令有点儿奇怪，但是起作用了。 CONTRIBUTING.md 文件已经是修改未暂存的状态了。到目前为止这个神奇的调用就是你需要对 git reset 命令了解的全部。我们将会在 重置揭密 中了解 reset 的更多细节以及如何掌握它做一些真正有趣的事。


## 撤消对文件的修改

如果你并不想保留对 CONTRIBUTING.md 文件的修改怎么办？ 你该如何方便地撤消修改 - 将它还原成上次提交时的样子（或者刚克隆完的样子，或者刚把它放入工作目录时的样子）？ 幸运的是，git status 也告诉了你应该如何做。 在最后一个例子中，未暂存区域是这样：

    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md

它非常清楚地告诉了你如何撤消之前所做的修改。 让我们来按照提示执行：

    $ git checkout -- CONTRIBUTING.md
    $ git status
    On branch master
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        renamed:    README.md -> README


# 远程仓库的使用

如果想查看你已经配置的远程仓库服务器，可以运行 git remote 命令。它会列出你指定的每一个远程服务器的简写。如果你已经克隆了自己的仓库，那么至少应该能看到 origin - 这是 Git 给你克隆的仓库服务器的默认名字。

    $ git remote
    origin

你也可以指定选项 -v，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL。

    $ git remote -v
    origin	https://github.com/schacon/ticgit (fetch)
    origin	https://github.com/schacon/ticgit (push)

如果你的远程仓库不止一个，该命令会将它们全部列出。 例如，与几个协作者合作的，拥有多个远程仓库的仓库看起来像下面这样：

    $ git remote -v
    bakkdoor  https://github.com/bakkdoor/grit (fetch)
    bakkdoor  https://github.com/bakkdoor/grit (push)
    cho45     https://github.com/cho45/grit (fetch)
    cho45     https://github.com/cho45/grit (push)
    defunkt   https://github.com/defunkt/grit (fetch)
    defunkt   https://github.com/defunkt/grit (push)
    koke      git://github.com/koke/grit.git (fetch)
    koke      git://github.com/koke/grit.git (push)
    origin    git@github.com:mojombo/grit.git (fetch)
    origin    git@github.com:mojombo/grit.git (push)

这样我们可以轻松拉取其中任何一个用户的贡献。 此外，我们大概还会有某些远程仓库的推送权限。


## 添加远程仓库

运行 git remote add <shortname> <url> 添加一个新的远程 Git 仓库，同时指定一个你可以轻松引用的简写：

    $ git remote
    origin
    $ git remote add pb https://github.com/paulboone/ticgit
    $ git remote -v
    origin	https://github.com/schacon/ticgit (fetch)
    origin	https://github.com/schacon/ticgit (push)
    pb	https://github.com/paulboone/ticgit (fetch)
    pb	https://github.com/paulboone/ticgit (push)

现在你可以在命令行中使用字符串 pb 来代替整个 URL。

    $ git fetch pb
    remote: Counting objects: 43, done.
    remote: Compressing objects: 100% (36/36), done.
    remote: Total 43 (delta 10), reused 31 (delta 5)
    Unpacking objects: 100% (43/43), done.
    From https://github.com/paulboone/ticgit
    * [new branch]      master     -> pb/master
    * [new branch]      ticgit     -> pb/ticgit

例如，如果你想拉取 Paul 的仓库中有但你没有的信息，可以运行 git fetch pb

现在 Paul 的 master 分支可以在本地通过 **pb/master** 访问到 - 你可以将它合并到自己的某个分支中，或者如果你想要查看它的话，可以检出一个指向该点的本地分支。


## 从远程仓库中抓取与拉取
从远程仓库中获得数据，可以执行：

    $ git fetch [remote-name]

这个命令会访问远程仓库，从中拉取所有你还没有的数据。 执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。

如果你使用 clone 命令克隆了一个仓库，命令会自动将其添加为远程仓库并默认以 “origin” 为简写。 所以，git fetch origin 会抓取克隆（或上一次抓取）后新推送的所有工作。 必须注意 git fetch 命令会将数据拉取到你的本地仓库 - 它并不会自动合并或修改你当前的工作。 当准备好时你必须手动将其合并入你的工作。

如果你有一个分支设置为跟踪一个远程分支（阅读下一节与 Git 分支 了解更多信息），可以使用 git pull 命令来自动的抓取然后合并远程分支到当前分支。 这对你来说可能是一个更简单或更舒服的工作流程；默认情况下，git clone 命令会自动设置本地 master 分支跟踪克隆的远程仓库的 master 分支（或不管是什么名字的默认分支）。运行 git pull 通常会从最初克隆的服务器上抓取数据并自动尝试合并到当前所在的分支。


## 推送到远程仓库

    git push [remote-name] [branch-name]
    $ git push origin master

当你和其他人在同一时间克隆，他们先推送到上游然后你再推送到上游，你的推送就会毫无疑问地被拒绝。 你必须先将他们的工作拉取下来并将其合并进你的工作后才能推送。


## 查看远程仓库
如果想要查看某一个远程仓库的更多信息

    git remote show [remote-name]

    $ git remote show origin
    * remote origin
    Fetch URL: https://github.com/schacon/ticgit
    Push  URL: https://github.com/schacon/ticgit
    HEAD branch: master
    Remote branches:
        master                               tracked
        dev-branch                           tracked
    Local branch configured for 'git pull':
        master merges with remote master
    Local ref configured for 'git push':
        master pushes to master (up to date)

它同样会列出远程仓库的 URL 与跟踪分支的信息。它告诉你正处于 master 分支，并且如果运行 git pull，就会抓取所有的远程引用，然后将远程 master 分支合并到本地 master 分支。 它也会列出拉取到的所有远程引用。

## 远程仓库的移除与重命名

如果想要重命名引用的名字可以运行 git remote rename 去修改一个远程仓库的简写名。

    $ git remote rename pb paul
    $ git remote
    origin
    paul

值得注意的是这同样也会修改你的远程分支名字。 那些过去引用 pb/master 的现在会引用 paul/master。

移除一个远程仓库 - 你已经从服务器上搬走了或不再想使用某一个特定的镜像了，又或者某一个贡献者不再贡献了 - 可以使用 git remote rm ：

    $ git remote rm paul
    $ git remote
    origin

## 打标签

在 Git 中列出已有的标签是非常简单直观的。 只需要输入 git tag。

    $ git tag
    v0.1
    v1.3

Git 使用两种主要类型的标签：轻量标签（lightweight）与附注标签（annotated）。

创建一个附注标签：

    $ git tag -a v1.4 -m 'my version 1.4'
    $ git tag
    v0.1
    v1.3
    v1.4

创建轻量标签，不需要使用 -a、-s 或 -m 选项，只需要提供标签名字：

    $ git tag v1.4-lw
    $ git tag
    v0.1
    v1.3
    v1.4
    v1.4-lw
    v1.5

你也可以对过去的提交打标签。

默认情况下，git push 命令并不会传送标签到远程仓库服务器上。在创建完标签后你必须显式地推送标签到共享服务器上。`git push origin [tagname]`

    $ git push origin v1.5 推送一个
    or
    $ git push origin --tags 一次性推送很多

检出标签是要再留意。




















