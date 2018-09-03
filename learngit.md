# 安装Git
1. 看系统有没有安装Git，在终端工具中输入**git**
1. 在Mac OS X上安装Git：
   - 安装homebrew，然后通过homebrew安装Git，
     - 使用命令行 **brew install git** 
     - 查看git的安装目录 **which git**
   - 从AppStore安装Xcode，Xcode集成了Git，不过默认没有安装，你需要运行Xcode，选择菜单“Xcode”->“Preferences”，在弹出窗口中找到“Downloads”，选择“Command Line Tools”，点“Install”就可以完成安装了。

---

# 创建版本库
1. 版本库，也就是一个目录，里面的所有文件都被Git管理，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
1. 创建版本库的命令操作
   - **mkdir learngit** 创建一个名为learngit的空目录
   - **cd learngit**
   - **pwd** 该命令用于显示当前目录
   - **git init** 通过该命令把这个目录变成Git可以管理的仓库
   - **ls -ah** .git目录默认隐藏，用该命令可以看到.git目录

---

# 创建一个文件，把文件添加到仓库并提交
1. 编写一个文件，该文件一定要放到learngit目录下
1. **git add readme.txt**，告诉git，把文件添加到仓库，add 后面可以跟具体的文件名，也可以跟 . 或者-A 添加该目录下所有文件。
1. **git commit -m**，告诉git，把文件提交到仓库,commit的作用是把暂存区中的文件，全部提交到版本库中。-m是必选命令，后面跟注释。
1. 配置用户名和用户邮箱
   - git config --global user.name "your_name"
   - git config --global user.email "your_email@gmail.com"

---

# 恢复文件
1. 不断修改并提交测试文件，使用 **git log** 命令查看每次修改的记录(commit操作的历史记录)， **git log --pretty=oneline** 该命令可以使每条记录在一行显示，显示如下：

 commit 5e696427f9b3a1342e06795d378f016a9c34561b (HEAD -> master)
Author: Wusiqi-w <Wusiqi_wm@163.com>
Date:   Fri Aug 31 15:50:54 2018 +0800

    append GPL

commit a7418f25779638bc3d14edd23e7b7eec898080a1
Author: Wusiqi-w <Wusiqi_wm@163.com>
Date:   Fri Aug 31 15:49:10 2018 +0800

    add distributed

commit e4996d7e65f4722c08891da8c22f5a82689bf4ce
Author: Wusiqi-w <Wusiqi_wm@163.com>
Date:   Fri Aug 31 15:40:18 2018 +0800

    wrote a readme file

   - 结束 **git log**或类似命令，在英文键盘下，点击q，即可结束该命令。
2. 在Git中,用HEAD(相当于指针)表示当前版本，**git reset --hard HEAD^** ，该条命令会使文件从当前版本回退到上一个版本，上上一个版本就是 **HEAD^^**，往上一百个则可以写成 **HEAD~100** ,
1.  **cat readme.txt** 查看当前文件的内容，
1. 如果回退一个版本后想要再回到之前的版本，可以通过 **commit id** 来找回，每一个commit的操作，都会生成一个commit id，命令语句为 **git reset --hard 5e6964**
1. 若找不到之前新版本的commit id， **git reflog** 查看命令历史。
1. 版本回退总结：
   - HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令 **git reset --hard commit_id**。
   - 穿梭前，用**git log**可以查看提交历史，以便确定要回退到哪个版本。

---

# git的工作区和暂存区以及管理撤销删除修改
1. 工作区就是在电脑里能看到的目录，如learngit文件夹就是一个工作区。
2. 工作区中的隐藏目录.git就是Git的版本库，在版本库中，做重要的是称为stage（或index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的指针HEAD。
3. git add实际上就是把文件修改添加到暂存区；git commit实际上就是把暂存区的所有内容提交到当前分支。
4. **git status**, 该命令用来查看版本库的状态：
   - Untracked files: 未跟踪文件，一般是新建的文件
   - Changes not staged for commit: 已修改，未添加，未提交的文件。
   - Changes to be committed:修改已添加，未提交，文件已经添加到暂存区中，使commit命令提交
   - nothing to commit, working tree clean：工作区目前没有要添加或提交的文件。
5. **git diff HEAD -- readme.txt**该命令后加文件名可以查看工作区文件和版本库里面最新版本的区别。
6. **git checkout -- readme.txt** 该命令的意思就是把readme.txt文件在工作区的修改全部撤销，分为两种情况：
   - 一种是该文件修改后还没有添加到暂存区，撤销修改后，就回到和版本库一样的状态。
   - 一种是该文件已经添加到暂存区，又作了修改，撤销修改就回到添加到暂存区后的状态。
7. 如果把错误的文件添加到了暂存区，想要撤销修改：
   - **git reset HEAD <file>** 先使用该命令，把暂存区的修改回退到工作区。
   - **git checkout -- readme.txt** 再使用该命令丢弃工作区的修改。
8. 若要删除一个文件，可以手动删除，或者使用命令 **rm <file>**进行删除。删除文件后，此时工作区和版本库状态就不一致了，git status命令会立刻告诉你哪些文件被删除了：
   - 若确实要从版本库中删除该文件，使用 **git rm <file>**删掉该文件，并且使用 **git commit -m **提交
   - 若是删错了，使用 **git checkout -- <file>**就可以把误删的文件恢复到最新版本
   - **git checkout**其实就是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以还原。

---

# 远程仓库
1. 生成ssh密钥：生成ssh密钥后，会在当前用户的根目录下创建.ssh目录。因此，可以通过以下两种方式检查是否生成过ssh 密钥。
   - **cd ~/.ssh** 是否可切换到.ssh目录，切换到对应目录后，可以使用 **ls -l ~/**检查。
   - 也可重新生成ssh密钥：**ssh-keygen -t rsa -C "Wusiqi_wm@163.com"**，生成成功后，可以使用 **clip < ~/.ssh/id_rsa.pub**该命令查看生成的密钥，复制后将其添加到Github上。
2. 添加远程库：在github上创建一个新的仓库，在本地仓库下运行以下命令 **git remote add origin git@github.com:Wusiqi-w/learngit.git**，将本地仓库和github远程仓库关联。远程库的名字就是origin，这是Git默认的叫法，也可以改成别的。
    - **git push -u origin master**把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程，由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
    - **git push origin master** 通过该命令把本地master分支的最新修改推送至GitHub。
    -  **git remote**查看远程库的信息。**git remote -v**，查看关联的远程库更详细的信息。
    - **git pull origin master**，从远程仓库下载到本地仓库
3. 从远程库克隆。在本地文件夹下运行以下命令： **git clone git@github.com:Wusiqi-w/gitskills.git**，地址可以从github页面复制。Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

---

# 分支管理
1. 创建并切换到dev分支： **git checkout -b dev**
   -  **git branch dev**,创建dev分支
   -  **git checkout dev**, 切换到dev分支。
   -  **git branch** 查看所有分支。当前分支前会标*号。
2. 合并分支：
   - **git checkout master**先切换到master分支
   - **git merge dev** 把dev分支的工作成果合并到master分支上。
   - **git branch -d dev** 合并完成后，删除dev分支。
3. 解决冲突：当两个分支对同一个文件都进行了修改，合并分支时会提醒解决冲突。
   - 手动打开存在冲突的文件，修改后，保存再进行提交，**git log --graph --pretty=oneline --abbrev-commit** 可以看到分支的合并情况。
   - 解决冲突后， **git branch -d **删除不需要的分支。
4. 分支管理：
   - 通常合并分支时，会使用Fast forward模式。
   - 要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。 **git merge --no-ff -m "merge with no-ff" dev**
   - --no-ff参数，表示禁用Fast forward，因为普通模式合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。
5. bug分支:
   - 在分支工作时，若需要解决bug，而当前分支的工作还不能提交，可以储藏当前工作 **git stash**，然后切换到需要修复bug的分支上创建临时分支。
   - 解决完bug之后，需要回到之前分支继续工作。 **git stash list**，使用该命令查看储藏的工作现场。
   - stash@{0}: WIP on dev: 59389fd add merge
   - 恢复工作现场有两种方式。
     - 一是用**git stash apply**恢复，但是恢复后，stash内容并不删除，你需要用**git stash drop**来删除；
     - 另一种方式是用**git stash pop**，恢复的同时把stash内容也删了：
   - 可以多次stash，恢复的时候，先用**git stash list**查看，然后恢复指定的stash，用命令: **git stash apply stash@{0}**
6. 删除一个没有合并过的分支：
   - git branch -d  删除失败，会提示该分支还没有被合并，如果删除，将丢失修改，
   - 强行删除，需要使用大写的-D参数： **git branch -D **
7. 删除Github上的分支 **git push origin :dev**
8. 多人协作：
   - 本地新建的分支如果不推送到远程，对其他人就是不可见的。
   - 若本地仓库没有dev分支，远程仓库有dev的分支，则直接抓取。
     - **git checkout -b dev origin/dev**
   - 若本地仓库没有dev的分支，远程仓库也没有dev的分支，则首先在本地仓库建立dev的分支，然后推送本地dev到远程仓库： 
     - **git checkout -b dev**
     - **git push origin dev**
   - 若本地仓库和远程仓库均有dev分支：
     - 尝试推送： **git push origin dev**
     - 若失败，说明远程仓库的最新提交和你试图推送的提交有冲突：需要git pull：把最新的提交从origin / dev抓下来。
     - 若git pull失败，提示：no tracking  information,则说明本地分支和远程分支的连接关系没有创建，用命令： **git branch --set-upstream-to dev origin/dev**,再**git pull**。
     - git pull成功，手动解决合并冲突。
     - 最后 **git push origin dev**
9. rebase操作可以把本地未push的分叉提交历史整理成直线，rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。 **git rebase**

---

# 标签管理
1. 创建标签
   - **git tag v1.0**，默认对最新的commit打一个标签。
   - 若需要对之前的commit打标签，先找到历史提交的commit id, **git log --pretty=oneline --abbrev-commit**,然后 **git tag v0.9 <commit id>**
   - **git tag**可以查看所有标签。注意，标签不是按时间顺序列出，而是按字母排序的
   - 可以用**git show <tagname>**查看标签信息。还可以创建带有说明的标签： **git tag -a v0.1 -m "version 0.1 released" e4996d7**，用-a指定标签名，-m指定说明文字
2. 操作标签
   - 删除本地标签： **git tag -d v0.1**
   - 推送标签到远程： **git push origin v1.0**,或推送全部尚未推送到远程的本地标签： **git push origin --tags**
   - 删除远程标签，先从本地删除：**git tag -d v0.9**,再从远程删除： **git push origin :refs/tag/v0.9**

---

# Github
1. 如何参与一个开源项目：
   - 访问项目主页，点Fork就会在自己的账号克隆一个该开源项目的仓库，然后从自己的账号下clone到本地。在本地修改后，推送到自己的仓库。
   - 如果你希望该开源项目的官方库能接受你的修改，你就可以在GitHub上发起一个pull request。
2. 使用码云：
   - 注册登录后添加ssh密钥，在码云创建一个新的项目。
   - **git remote add origin **使用该命令把本地库和码云的远程库关联。
   - 若在使用**git remote add origin **时报错，说明本地库已经关联了一个名叫origin的远程库，可以使用  **git remote -v **查看远程库信息。
   - 删除已有的origin远程库： **git remote rm origin**
   - 使用不同的名字可以同时关联码云和GitHub远程库。 **git remote add github **和 **git remote add gitee **
   - 推送时也需要改变名字： **git push github master**, **git push gitee master**

---

# 自定义Git
1. 让Git显示颜色，会让命令输出看起来更醒目： **git config --global color.ui true**
2. 忽略特殊文件：
   - 在Git工作区下创建一个特殊的.gitignore文件，把要忽略的文件名填进去，Git就会自动忽略这些文件。创建文件： **touch .gitignore**, 编辑文件： **vim .gitignore**,
   - 按 WIN+I 进入插入模式，这个模式下才能编辑该文件,输入符合格式内容后，按 ESC 键退出插入模式，保存文件退出 VIM 的两种模式：
     - 快捷键：
       - 按 Shift + zz ——保存退出
       - 按 Shift + zq ——不保存退出（q 表示放弃）
     - 命令行
       - :q ——不保存退出
       - :q! ——不保存强制退出
       - :wq ——保存退出（w 表示写入，无论是否修改，时间戳更改）
       - :x  ——保存退出（若内容未改，时间戳不变）
   - 忽略文件的原则是：
       - 忽略操作系统自动生成的文件，比如缩略图等；
       - 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
       - 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。
   - 然后把.gitignore文件提交到Git。
   - 添加一个被.gitignore文件忽略的文件到git，使用命令： **git add -f .DS_Store**
   -  **git check-ignore** 可以检查.gitignore文件中哪个规则写错了
3. 配置别名：
   - **git config --global alias.st status**,该命令就是告诉Git，以后st就表示status。
   - 当然还有别的命令可以简写，很多人都用co表示checkout，ci表示commit，br表示branch
     - **git config --global alias.co checkout**
     - **git config --global alias.ci commit**
     - **git config --global alias.br branch**
   - 命令git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区。既然是一个unstage操作，就可以配置一个unstage别名： **git config --global alias.unstage 'reset HEAD'**
   - 配置一个git last，让其显示最后一次提交信息 **git config --global alias.last 'log -1'** 这样，用git last就能显示最近一次的提交
   - 配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。
   - 每个仓库的配置文件都放在.git/config文件中。别名就在alias后面，要删除别名，直接把对应的行删掉即可。
   - 当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中。配置别名也可以直接修改这个文件，如果改错了，可以删掉文件重新通过命令配置