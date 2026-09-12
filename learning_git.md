# Git学习

## 纯git部分

### 基础命令

1. pwd : print working directory   - 打印当前所在的绝对路径

2. cd : change directory  - 切换目录
    - cd + 地址 , 进入到此地址所在的目录
    - cd ..  - 回到上一级目录
    - cd ~  - 回到家目录
    - cd -  - 返回上一次所在目录

3. mkdir : make directory
    `mkdir /D/user/jotang`则是在D盘中的user目录中创建了名为jotang的仓库。

4. rm : remove

### 仓库创建及初始化

1. 建立Git仓库
    - `mkdir /D/gerenyong/jotang_`

2. 改变目录
    - `cd /D/gerenyong/jotang_`
    - 执行完后`main`切换为`cd /D/gerenyong/jotang_`

3. 初始化仓库
    - `git init`
    - 执行显示`Initialized empty Git repository in D:/gerenyong/jotang_/.git/`

### 文件操作

1. 在文件中添加文件`README.txt`,第一次随意输入内容为`read read read read read waht`

2. 添加与提交

- `git add README.txt`   将文件添加到工作区
   
- `git commit -m "a README.txt"`    将文件提交到工作区
    > 显示：
    `[master (root-commit) 8a97e2a] a README.txt`
    `1 file changed, 1 insertion(+)`
    `create mode 100644 README.txt`
- 内容显示表明一个文件被改变，增添了一行内容。

3. 状态查看

- 在添加文件之后提交文件之前，利用`git status`进行了当前状态查看。
    >显示：
    `On branch master`
    `No commits yet`
    `Changes to be committed:`
    `(use "git rm --cached <file>..." to unstage)`
    `new file:   README.txt`
- 可以看到当前文件尚未提交,以及文件名称。

> 后进行一次修改，将内容改为了
"Do not go gentle into that good night,
Old age should burn and rave at close of day;
Rage, rage against the dying of the light."
之后在此基础上操作。

4. 查看修改记录

- Git具有保存修改内容的功能，即倘若只是对文件进行了修改，并**没有进行add与commit**，那么使用`git diff <file>`就能查看修改内容。

- 例如在三行诗后加上作者名字，改为：
    "Do not go gentle into that good night,
    Old age should burn and rave at close of day;
    Rage, rage against the dying of the light.
    --Dylan Thomas"

- 先用`git status`查看当前状态：

    `$ git status`
    `On branch master`
    `Changes not staged for commit:`
    `(use "git add <file>..." to update what will be committed)`
    `(use "git restore <file>..." to discard changes in working directory)`
            `modified:   README.txt`

    `no changes added to commit (use "git add" and/or "git commit -a")`

    - 内容显示改变并未添加，并未处于暂存区，并提示下一步操作。

- 使用`git diff`查看修改了什么内容。
    `$ git diff`
    `diff --git a/README.txt b/README.txt`
    `index 64d8ef9..64c9d72 100644`
    `--- a/README.txt`
    `+++ b/README.txt`
    `@@ -1,3 +1,5 @@`
    `Do not go gentle into that good night,`
    `Old age should burn and rave at close of day;`
    `-Rage, rage against the dying of the light.`
    `\ No newline at end of file`
    `+Rage, rage against the dying of the light.`
    `+`
    `+--Dylan Thomas`
    `\ No newline at end of file`

- 内容清晰表明：增添了作者的名字在"rage"这一行后面。

- 在清晰了修改内容后，将文件进行add和commit，完成修改。

### 版本回退

1. 版本控制系统有个命令可以告诉我们历史记录，在Git中，我们用
`git log 命令查看：`
`$ git log`
`commit 6ab30ecf59bc1f97c9be99c2426ca24a3d144f8f (HEAD -> master)`
`Author: rainball <rainbaii797@gmail>`
`Date:   Wed Sep 2 20:02:02 2026 +0800`

`    add the name of the poem`

`commit 9cd2fdbbeb1b6751d39b712a114be8acf53f6478`
`Author: rainball <rainbaii797@gmail>`
`Date:   Wed Sep 2 19:18:23 2026 +0800`

`    replace with a poem`

`commit 8a97e2ad6a949a6ddd4b380286b581df9d055cae`
`Author: rainball <rainbaii797@gmail>`
`Date:   Wed Sep 2 15:10:45 2026 +0800`

`    a README.txt`

- 从上向下，依次是**由最近的到最早的**更改，同时显示在commit时写的修改内容message.


2. 进行版本回退

- 现在准备进行版本回退，即未添加诗作作者名字的版本。
- 在进行操作时，Git必须知道当前版本是哪个版本。在Git中：
    - 用`HEAD`表示当前版本，即最新提交的版本。
    - 用`HEAD^`表示上一个版本。
    - 用`HEAD^^`表示上上个版本。
    - 再往上的比如100个^，写成`HEAD~100`。

- 要进行版本回退，可以使用`git reset`命令：
    `$ git reset --hard HEAD^`
    `HEAD is now at 9cd2fdb replace with a poem`

    - 其中的`--hard`参数会回退到上个版本的已提交状态;而`--soft` 会回退到上个版本的未提交状态;`--mixed`会回退到上个版本已添加但未提交的状态。
- 操作完成后，再在txt文件所在文件夹中查看文件内容，作者名字已经不见了，版本回退成功。

3. 回到未来版本

- 现在的`HEAD`指向的已经是没有诗人名字的版本了，若想要回到有名字的版本，只能是在**能够找到那一次的`commit id`**的情况下，再次使用`git reset`,就可以回到那个版本：
    `$ git reset --hard 6ab30`
    `HEAD is now at 6ab30ec add the name of the poem`

- 同时，在Git中，有一个命令`git reflog`,可以记录过程中的每一次命令，于是，若是想要回到未来版本，id的寻找不成问题：
    `$ git reflog`
    `6ab30ec (HEAD -> master) HEAD@{0}: reset: moving to 6ab30`
    `9cd2fdb HEAD@{1}: reset: moving to HEAD^`
    `6ab30ec (HEAD -> master) HEAD@{2}: commit: add the name of the poem`
    `9cd2fdb HEAD@{3}: commit: replace with a poem`
    `8a97e2a HEAD@{4}: commit (initial): a README.txt`
    - 可见，依旧是依次**由最近的到最早的**更改。

### 工作区与暂存区

- 工作区(Working Directory):就是在电脑里能看到的目录，比如`jotang_`文件夹就是一个工作区。
- 版本库(Repository):工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。
    - Git的版本库里存了很多东西，其中最重要的就是称为stage的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向master 的一个指针叫`HEAD`。

- 在添加文件时：
    - 第一步用add添加文件，实际上就是把文件修改添加到暂存区；
    - 第二步用commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。(因为在创建Git版本库时，自动创建了唯一一个master分支，所以在此分支上提交更改。)

### 撤销修改

### 添加远程库

- 将本地仓库的内容推送到GitHub仓库:
    - `$ git remote add origin git@github.com:rainb-all/jotang-recruit-2026.git`将本地库关联至远程库，远程库名为origin.
    - 再用`git push`进行推送。
    `$ git push -u origin master`
    完成后，由于原远程库只有main分支，但关联的是master分支，所以在远程库中创建了master分支。
