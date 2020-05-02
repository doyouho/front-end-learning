## 学习资源：
    [Git 下载](https://git-scm.com/downloads)
    [Github Desktop下载](https://desktop.github.com/)
    [文档教程：廖雪峰官网Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)
    [文档教程：傻瓜都会用的Github Desktop](https://www.jianshu.com/p/06a960d991aa)
    [文档教程：官方](https://help.github.com/en/desktop/getting-started-with-github-desktop)
	提示：安装和学习途中遇到任何问题，请上网搜索答案。
	
## 学习路径： 
    邮箱创建Github账号，若是可以下载安装一个Github Desktop 和 Git，学习下面Github Desktop基本操作，初级到这里就基本够用了。
	学习方式：以前面的文档教程为主体来理解概念，以本文档的总结为思路或笔记来学习。
	（网上有说Github Desktop难下载的，而我遇到的问题是无法登陆，网上搜到的原因有弹框拦截，更多的说是英文输入法，最后我是换了系统的英文输入法登陆进去的）
	（除了这个GUI界面，还有Git本身也有图形用户界面？还有人用TortoiseGit的，我安装不成功，所以弃了。最近重装电脑了，本想重装Git，但是下载不了，就将就用以前装的了。）
	更多的学习：
		（1）Github上的项目界面是要熟悉的，课下做功课。
		（2）学习Git的命令行操作。
	
## Github Desktop 基本操作（配合上面教程链接，下面只是总结）：
1.创建/克隆一个本地仓库
2.提交改变  commit to 分支
3.推送到远程仓库 push origin 推送本地的修改
4.更新本地仓库 一键fetch
5.版本回退 打开history你会发现有很多commit后的历史记录,右键revet this commit
6.创建分支 首页的current branch （会保留原有的master分支内容）
（会看到default branch是master，这是所有git仓库的默认主分支，都叫master，origin是你github的分支，关联的是服务器端。）
7.合并分支  菜单栏branch- Merge into current branch【Ctrl+Shift+M】
   永远都是把其他分支merge到当前,所以当前的分支先选好，再选需要合并到这里的分支。

## Git的命令学习 -- 基于廖雪峰老师的文档总结	
git  ——   两周由Linus用C书写的分布式版本控制系统（诞生：2005年；2008年github诞生，jQuery，PHP，Ruby等开源项目移居地）
	所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，而图片、视频，word这些二进制文件没法跟踪文件的变化。

1.首次安装之后，配置姓名和邮箱
git config --global user.name ""
git config --global user.email ""

【下面代码中的文件名都含后缀名】
2.进入安装的目录，创建一个git库。如我的第一个库：E:/git/Git/learngit  
		创建文件夹，      用 mkdir learngit
3.再初始化这个库，用 git init
		再建一些文件，   用 vi 文件名 （:wq退出 ）
		直接看文件内容，用 cat 文件名
4.把文件添加到库，用 git add 文件名
5.把文件提交到库，用 git commit -m "更新描述"

### 帮助查看的命令:
git status—— 查看库状态；  
git diff 文件名 ——查看文件具体修改了什么
git diff HEAD -- readme.txt——提交后查看工作区和版本库里面最新版本的区别
pwd 命令用于显示当前目录(即路径)
ls -ah 显示隐藏文件

git reflog —— 查看命令历史
git log —— 查看最近到最远的提交日志
git log --pretty=oneline  （同上），却只看到commit id和自己写的更新说明。（按q）
$ git log --graph --pretty=oneline --abbrev-commit
git reset --hard HEAD^
git reset --hard commit_id
git reset HEAD <file> ——可以把暂存区的修改撤销掉（unstage），重新放回工作区：
$ git reset HEAD readme.txt
          （git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。）

git checkout -- 文件名 ——可以丢弃工作区的修改。表示把文件在工作区的修改全部撤销。
$ git checkout -- readme.txt 两种情况：①还没有被放到暂存区（没add），则撤销修改就回到和版本库一模一样的状态；②已经添加到暂存区后（有add）且作了修改，则撤销修改就回到添加到暂存区后的状态。【就是没add的回到版本库状态，add了的回到暂存区状态】

#### 【修改场景】
场景1：git checkout -- file。（未add）文件内容已乱，想直接丢弃工作区的修改（用版本库里的版本替换工作区的版本）。
			同样可用于误删操作，只是注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！
场景2：git reset HEAD <file>，(已经add，想改)，文件内容已乱，添到了暂存区时想丢弃修改。执行后就回到了场景1，第二步按场景1操作。
场景3：git reset --hard commit_id，（已经commit，但未push到远程库）已经提交了不合适的修改到版本库时，想要撤销本次提交。

#### 【确定要删】
$ rm test.txt  工作区删文件
$ git rm test.txt   缓存区删文件，但已被提交commit到版本库的文件，永远不用担心误删。
$ git commit -m "remove test.txt"  缓存区同步，也就相当于版本库中没有该文件。


#### 【关于远程库】
（1）远程仓库的搭建材料：.ssh目录下的公钥  和 远程仓库的注册账号（如github和码云）
	公钥：用户主目录(C盘里的用户，user)下.ssh目录里的id_rsa.pub文件的内容。若无，则打开Git Bash，创建SSH Key：
$ ssh-keygen -t rsa -C "你的邮箱" （安装时一路回车，使用默认值就行，注意这个邮箱和你的登陆账号应该是一致的）
若是创建多个密钥，需要自定义名称，例如 $ ssh-keygen -t rsa -C "对应的邮箱" -f ~/.ssh/自定义名称

	Github账号配置：用户设置里，创建一个SSL Key，填上title或名称，填上公钥，添加即可。

（2）搭建后，需要关联本地库。
1.在网站上面创建一个新的库，并且这个名字和本地的是一样的。

2.要关联一个远程库，使用命令 (一般git remote add +origin【默认值】+网址.git)
git remote add github https://github.com/doyouho/learngit.git  （github的上传）
git remote add github git@github.com:doyouho/learngit.git
git remote add gitee https://gitee.com/kathy_candy/learngit.git  (码云的上传)
git remote add gitee git@gitee.com:kathy_candy/learngit.git （或者这种）

3.关联后，推送内容（本地到网络的过程）
git push -u 库名 master  ——第一次推送master分支的所有内容；（-u 表示第一次推送，以后推送就不用加了）
git remote -v   ——查看远程库信息
git remote rm origin  ——删除已有的远程库 （此处的origin为默认库的名称，应该按实际库名来操作）
【注意，关于库名是什么，上面第2步add的后一个单词就是设名字的；或者用git remote -v查看的每行头一个单词。】

（3）从远程库克隆：克隆给出的地址不止一个，建议能不用http方式的就不用，通过ssh支持的原生git协议速度最快。
首先必须知道仓库的地址，然后【在Git安装目录下】使用git clone命令克隆。
命令cd .. 返回上一层
git clone git@github.com:doyouho/gitskills.git  【组成：git clone 克隆地址.git】



#### 【关于标签】
理解：标签是版本库的一个快照。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。它是指向某个commit的指针,与分支不同，分支可以移动，标签不能移动。标签创建时，只存储在本地，不会自动推送到远程。（特点，自己命名，方便查找）

创建标签原理---找到分支 ,然后tag 标签。
git branch  ——查看当前库所有分支（当前分支前面会标一个*号）
git checkout/switch master  ——切换到master分支
			【找 commit_id，用git log --pretty=oneline】
git tag 标签 commit_id  —— 打一个新标签，默认为HEAD（即最新commit的），也可以指定一个commit_id,commit_id是个可选值。
git tag -a 标签 -m "说明文字"  commit_id ——可以指定标签信息（用-a指定标签名，-m指定说明文字）。
git tag  ——查看所有标签。
git show 标签 ——查看标签信息。

git pull 远程更新拉到本地
git push 库名 标签 ——推送一个本地标签到远程。
git push 库名 --tags ——推送全部未推送过的本地标签到远程。
git tag -d 标签 ——删除一个本地标签；
git push origin :refs/tags/标签 ——删除一个远程标签。【注意】删除远程标签需要先从本地删除，然后从远程删除。



#### 【偷懒系列---创设关键字，加快命令输入】

方法一：用命令一条条设置 （加上--global是对当前用户起作用，否则只针对当前的仓库起作用。）
git config --global alias.缩词 原词
	git config --global alias.st status   ——用st表示status。
	git config --global alias.unstage 'reset HEAD'   ——用unstage表示'reset HEAD'
	git config --global alias.last 'log -1' ——用last表示 'log -1'，让其显示最后一次提交信息。（我们常见的是git log）

	还有人丧心病狂地把lg配置成了：（这个方便查看提交的记录）
	git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit" 

方法二：修改配置文件
cat .git/config  ——查看配置，设置的缩写都在[alias]栏目中。别名就在[alias]后面，要删除别名，直接把对应的行删掉即可。
cat .gitconfig  ——用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中
	    【重点】可以直接修改这个文件，如果改错了，可以删掉文件重新通过命令配置。

#### 【分支管理】
（1）创建与合并分支
	git branch 新分支 ——创建分支（若不加分支名，即查看所有分支）
	git switch -c 新分支 ——创建并切换到新的分支  或用git checkout -b 新分支
	git switch/checkout 已有分支 ——直接切换到已有的分支
	git merge 分支2  ——分支合并 （合并分支2到当前分支）
	git branch -d 分支 ——删除分支

（2）解决冲突
	当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。
	要查看远程库的信息，用git remote：
		$ git remote
		origin
	或者，用git remote -v显示更详细的信息：
		$ git remote -v
		origin  git@github.com:michaelliao/learngit.git (fetch)
		origin  git@github.com:michaelliao/learngit.git (push)
	上面显示了可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址。
	
（3）分支管理策略（略）
（4）Bug分支（略）
（5）Feature分支（略）
（6）多人协作（略）
（7）Rebase
	基于场景：后push的童鞋不得不先pull，在本地合并，然后才能push成功。
	rebase操作可以把本地未push的分叉提交历史整理成直线；
	rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。


#### 【运作原理的理解】
工作区有一个隐藏目录.git，这个是Git的版本库。Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，
还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
把文件往Git版本库里添加的时候，是分两步执行的：
第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。
Git是如何跟踪修改的？每次修改，如果不用git add到暂存区，那就不会加入到commit中。
关于删除，当上传到远程库的时候，是没有后悔药吃的，你的傻逼行为会被看得一清二楚，嘻嘻。

#### 【忽略特殊文件 未整理好】
忽略文件的原则是：
	忽略操作系统自动生成的文件，比如缩略图等；
	忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
	忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。
不需要从头写.gitignore文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。
[.gitignore文件的配置文件](https://github.com/github/gitignore)
```
# My configurations:
db.ini
deploy_key_rsa

# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini

# 此为注释 – 将被 Git 忽略

*.cs       # 忽略所有 .cs 结尾的文件
!ABC.cs    # 但 ABC.cs 除外
/BLL       # 仅仅忽略项目根目录下的 BLL 文件，不包括 subdir/BLL
build/     # 忽略 build/ 目录下的所有文件
doc/*.txt  # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt

```
规则很简单，不做过多解释，但是有时候在项目开发过程中，突然心血来潮想把某些目录或文件加入忽略规则，按照上述方法定义后发现并未生效;
————原因是.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交：

git rm -r --cached .
git add .
git commit -m 'update .gitignore'

最后一步就是把.gitignore也提交到Git，就完成了！当然检验.gitignore的标准是git status命令是不是说working directory clean。
使用Windows的童鞋注意了，如果你在资源管理器里新建一个.gitignore文件，它会非常弱智地提示你必须输入文件名，但是在文本编辑器里“保存”或者“另存为”就可以把文件保存为.gitignore了。

如果你确实想添加该文件，可以用-f强制添加到Git：
$ git add -f App.class
或者你发现，可能是.gitignore写得有问题，需要找出来到底哪个规则写错了，可以用git check-ignore命令检查：

$ git check-ignore -v App.class
.gitignore:3:*.class	App.class
Git会告诉我们，.gitignore的第3行规则忽略了该文件，于是我们就可以知道应该修订哪个规则。

小结
忽略某些文件时，需要编写.gitignore；
.gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！
