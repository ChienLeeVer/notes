# Git

标签（空格分隔）： git

---
### 基本操作

git log: 查看提交历史

git reflog: 查看命令历史

git init : 初始化git仓库

git add . : 提交工作区内容到暂存区

git commit : 在当前父节点的基础上，将暂存区全部内容提交到分支上形成新的节点

git branch newbranchName: 在同一节点上创建一个分支。

git branch -d oldName:删除某一个分支  -D表示即使当前工作区已经被提交到暂存区和分支还没合并到main分支上，也要强制删除这个分支以及它的内容包括暂存区

git checkout branchName : 将跳转到该分支上进行其它操作命令

git switch branchName:跳转到某分支上

git rm file.txt: 从版本库中同步删除某个文件

git tag tagName [hash值]: 在当前分支上给其定义一个标签，用于代替hash值，如果不在分支/想给某个历史分支打上标签，通过git log获取历史然后找到hash值作为第二个参数

### 合并分支
git merge B : 当前所在分支为A,把B分支与A分支合并形成C。

git rebase B : 复刻当前分支A ，然后放到B下，成为其子代。（实质仍然是并行执行，只改变了提交序列）

git rebase A B : 将B合并到A，成为A的子代

git cherry-pick B C ... G ：当前所在分支为A，能够按照顺序，一次性将B，C...顺序依次成为A的子代

git rebase -i HEAD~num : 当不知道哈希值时，此方法能够通过UI复刻包括当前分支在内以及之前的提交记录总共num个进行复刻。

### 移动HEAD
HEAD : 总是指向当前分支上最近一次提交记录。使用如下：git checkout 哈希值，即可让HEAD->分支名->其所在的唯一标识哈希值, 这个哈希值可以通过git log查看

git checkout A分支^ / 某个~num : 由于查看哈希值并不方便，因此^表示查看A分支的上一次提交记录即父节点，而-num(num为正整数)，表示查看上几次的提交记录， ~2表示上上次提交记录，**此时HEAD指向其上上次提交记录**.

git branch -f A B ： 能够让A强制移动到B。即使分支2为父节点，因此可以配合HEAD使用，如git branch -f 分支1 HEAD~3 。

git checkout HEAD^num : num表示第几个父节点，因为可能存在这种情况，C1提交了一次，形成C2,然后又回到C1, 再次提交形成C3,现在在C2提交一次形成C4, 那么git checkout HEAD^表示回到HEAD的上一层的第一个父节点，而git checkout HEAD^2 表示回到上一层第二个父节点。当然^和～可以连写，如git checkout HEAD~^2~1

### 撤销
git reset HEAD~ ：撤销HEAD~所指向的提交记录即commit到分支的记录。仅适合本地分支撤销,通常配合git log查看提交历史,git reflog查看命令历史

git checkout -- filename.txt ：假设文件修改后被提交到暂存区即git add filename.txt，此命令可以撤销修改，使得filename.txt的提交无效，git checkout -- 其实是用版本库里的版本替换工作区的版本


git revert HEAD : 支持远程分支撤销，提交一次。如提交了A-B-C ,此时使用此命令，A-B-C-C1,此时C1的内容为B，此时git push 就可以与其他人分享撤销



------
## 远程仓库
git remote add origin(可以自定义名字，自定义名一般用于一个工作区关联多个远程仓库) xxx

git remote rm xxx 删除远程仓库关联

git clone url ： 将远程仓库复刻到本地仓库，并形成新的远程分支o/main和本地分支main，o/main表示上一次与远程仓库通信时的状态，并且远程仓库默认名为origin,o/main分支只有远程仓库更新才会更新。
### 拉取
git fetch ： 从远程仓库拉取数据并更新o/main分支，但是本地main不会更新，即只是单纯下载远程仓库的分支，不会更改本地main状态,可以配合合并分支命令使用

git fetch origin o/分支1 ：获取远程仓库上的分支1，然后更新本地仓库的同名 o/分支1上

git pull : git  fetch太麻烦了，还需要更新main，git pull 能够实现拉取数据，然后让o/main指向拉去到的最新分支，接着让main合并最新分支并形成一个main指向的新分支

git pull --rebase : git pull之后如果远程仓库又被其他人提交了新分支，那么自己是无法推送的，因此可以键入这个命令，先拉取远程仓库代码然后与本地合并

### 推送
git push : 将本地仓库推送到远程仓库，此时远程仓库main更新，本地的o/main也更新 ,一般在push之前，需要git pull --rebase。
如果分支还没与远程分支关联，可以使用git push -u origin main表示将当前分支推送到远程仓库的main分支上并关联

git push origin 分支1 ： 将当前分支推送到origin上的分支1 ，前提这个分支1能够跟踪o/分支1

git push origin 本地仓库分支:远程仓库分支 : 能够将没有跟踪的o/main的分支推送到远程仓库的分支上

>git push 默认 push main，这是因为在git clone的时候，会在本地创建main和o/main,main被指定跟踪o/main,而o/main只想远程仓库的main，如果想想让其他分支也跟踪o/main，实现在其他分支上直接push到远程仓库的main上可以，键入命令：```git     checkout -b xxxx o/main;```如果已经存在分支，则键入```git branch -u o/main xxxx```

### 配置公共SSH公钥
>(1)在终端中输入：**ssh-keygen -t rsa**
(2)找到公钥：**cd ~/.ssh**
(3)复制公钥：**cat id_rsa.pub**
(4)在github中的Settings找到ssh and gpb keys 选择new keys ,将公钥添加即可

### 修复bug

如果当前工作区在开发某个功能，开发到一半，接到一个bug修复任务又想保存现在的工作现场怎么办？
在当前功能开发分支dev下：git stash 表示将当前工作区存起来
然后切换回main分支并新建一个bug分支，然后修复，修复完成后就可以删除本地bug分支了。
然后切换回dev分支，此时输入git stash pop 表示恢复现场并删除临时存储的状态，此时dev分支是继承自旧的main分支，需要更新一下，因为此时本地main分支是自己修改的，所以是最新的，使用 git cherry-pick hash值，hash值，意思是将复制一个特定的提交到当前分支。遇到时再仔细研究吧。




### 购买一个linux服务器后自己搭建一个git仓库
<https://www.liaoxuefeng.com/wiki/896043488029600/899998870925664>
