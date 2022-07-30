git init 初始化项目
git status 查看当前仓库的状态
git add 将文件添加到暂存区
git commit -m '注释' 提交并且添加注释将所有的修改提交到分支
git log 查看提交的日志
git log --pretty=oneline 简要的查看日志
git reset --hard HEAD^ 回退到上一个版本
git reset --hard commitId 到指定的版本(commitId可以不用写全)
git reflog 查看你的操作的所有命令(可以用来查看commitId)达到指定的版本
git checkout -- readme.txt(文件名) 让文件回到最近一次git add的状态，如果commit了的话就用版本回退，最后如果推到了远端那就没救了
rm test.txt 删除工作区的文件 git status的状态会发生改变
当你是误删除文件的时候，只要这个文件提交过，就可以通过 git chekcout -- 文件名的方式将最近一次版本库中的文件添加到工作区
git rm test.txt 删除文件 然后进行 git commit 文件就从版本库中删除了

git remote add origin git@github.com:michaelliao/learngit.git 将远程的仓库和本地的仓库进行绑定
git push -u origin master 我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令
git push origin master 
git remote -v 查看远程库信息
git remote rm 名称（origin）  根据名字删除比如删除origin
此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除
HEAD 指向的是当前分支
在Git当中，目前是只有一条时间线，在git里面，这个分支叫做主分支
git checkout -b dev 创建dev分支，并切换到dev分支上面
git branch dev 创建分支 dev
Git checkout dev 切换到分支dev
Git branch 查看当前分支
Git checkout 分支名  切换到指定的分支
git merge dev 将dev分支合并到当前分支
git merge命令用于合并指定分支到当前分支。合并后，再查看readme.txt的内容，就可以看到，和dev分支的最新提交是完全一样的。

注意到上面的Fast-forward信息，Git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。当然，也不是每次合并都能Fast-forward，我们后面会讲其他方式的合并。

git branch -d dev 删除分支dev

小结
Git鼓励大量使用分支：

查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>或者git switch <name>

创建+切换分支：git checkout -b <name>或者git switch -c <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>

当我们两个分支对同一个文件的同一行进行了修改之后，在进行分支合并的时候回出现冲突，使用git status 查看出现冲突的文件

Cat 冲突的文件可以查看具体的内容
git log --graph --pretty=oneline --abbrev-commit  以分支树的方式进行日志的查看
小结
	要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；
	关联一个远程库时必须给远程库指定一个名字，origin是默认习惯命名；
	关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
	此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；
	分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！
工作区： 文件夹就是一个工作区

git rm --cached 文件名 表示将已经添加到暂存区的文件删除，状态变成 git add之前的状态，也就是未被追踪的状态
	注意：这个删除只是将已追踪状态删除了，变成了未追踪状态，原来的文件还是存在，并没有被删除