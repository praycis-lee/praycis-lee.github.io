---
title: git使用
date: 2017-11-21 10:25PM
categories: 'git'
---

#git
--

<h4>git初始化版本库</h4>

1. 选择一个合适的地方，创建一个空目录
2. 执行<code>git init</code>命令把这个目录变成Git可以管理的仓库
	<pre>
		$ git init
		Initialized empty Git repository in /Users/michael/learngit/.git/
	</pre>
	它会告诉你这是一个空的GIT仓库，同时在目录下生成一个<code>.git</code>目录，这个目录就是用来跟中管理版本库的，不可以随意更改里面的内容。这个目录是默认隐藏的。
3. 然后我们就可以在这个空目录里写东西了。这个目录就是我们的工作区，也就是git仓库了。然后我们在这个目录创建一个文件：
    <pre>
    	<code>$ touch a.txt</code>
    </pre>
    
<h4>添加文件到暂存区</h4>
    
4. 用命令<code>$ git add</code>告诉git， 把文件添加到暂存区
	<pre>$ git add a.txt</pre>
	
<h4>从暂存区提交到git仓库</h4>	
5.  用命令<code>$ git commit</code>告诉git,把文件从暂存区提交到仓库
	<pre>
		$ git commit -m "wrote a readme file"
		[master (root-commit) cb926e7] wrote a readme file
 		 1 file changed, 2 insertions(+)
 		 create mode 100644 readme.txt
	</pre>
	<code>-m</code>后面输入的是本次提交的说明。上面的意思是有一个文件被改变，并且插入了两行内容，<code>commit</code>可以提交很多文件。所以我们通常多次<code>add</code>很多文件，然后一次性的<code>commit</code>。至此，一个文件就成功的提交到git仓库啦！！


<h4>版本回退</h4>

1. 现在我们尝试修改一下这个a.txt
	
	<pre>
		hello, I changed the txt file
	</pre>
	然后尝试提交：
	<pre>
		$ git add a.txt
		$ git commit -m "modify"
		[master 3628164] modify
 		 1 file changed, 1 insertion(+), 1 deletion(-)
	</pre>
	每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为commit。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个commit恢复，然后继续工作，而不是把几个月的工作成果全部丢失。	
	现在，我们一共有两个版本被提交到GIT仓库中：
	
	版本1: wrote a readme file
	
	版本2： modify
	
	在git中， 我们用<code>git log</code>命令查看提交记录
	
	<pre>
	commit 009fc1a25691f4b00ae36aacdb7365d6cc6a7668
	Author: leexiaoyong <xiaoyong.l@moneplus.cn>
	Date:   Wed Nov 22 18:21:09 2017 +0800

    	modify

	commit eb799bf0f1eb6c00e1f9fc485a34286c11a05309
	Author: leexiaoyong <xiaoyong.l@moneplus.cn>
	Date:   Wed Nov 22 17:50:59 2017 +0800

    	wrote a readme file
	</pre>
	
	<code>git log</code>命令显示从最近到最远的提交日志，我们可以看到两次提交。如果这么看不太方便。我们可以加上<code>--pretty=oneline</code>参数：
	<pre>
		009fc1a25691f4b00ae36aacdb7365d6cc6a7668 modify
		eb799bf0f1eb6c00e1f9fc485a34286c11a05309 wrote a readme file
	</pre>
	
2. 在git中，用<code>HEAD</code>表示当前版本，上一个版本就用<code>HEAD^</code>,或者<code>HEAD~1</code>
	现在我们要把当前版本回退到上一个版本，就可以使用<code>git reset</code>命令：
	<pre>
		$ git reset --hard HEAD~1
		HEAD is now at eb799bf wrote a readme file
	</pre>
	
	此时，回退已经完成，回退之前的版本已经看不到了。
	如果要回退到一个指定的版本，我们可以使用：
	<pre>
		$ git reset --hard <版本号>
	</pre>
	Git的版本回退速度非常快，因为Git在内部有个指向当前版本的<code>HEAD</code>指针，当你回退版本的时候，Git仅仅是把HEAD指向了指定的版本号而已。
3. 如果回退到了某个版本，想恢复到新版本，但是回退的时候commit id 已经找不到了。不用担心，Git提供了一个命令<code>git reflog</code>来记录你的每一次命令：
	<pre>
		eb799bf HEAD@{0}: reset: moving to HEAD~1
		009fc1a HEAD@{1}: reset: moving to 009fc1a
		eb799bf HEAD@{2}: reset: moving to HEAD~1
		009fc1a HEAD@{3}: reset: moving to 009fc1a
		009fc1a HEAD@{4}: reset: moving to 009fc1a
		eb799bf HEAD@{5}: reset: moving to HEAD~1
		009fc1a HEAD@{6}: reset: moving to 			009fc1a25691f4b00ae36aacdb7365d6cc6a7668
	</pre>
	然后我们找到对应commit的id，然后选择前几位，执行
	<pre>
		$ git reset --hard <'commitid的前几位'>
	</pre>
	就可以恢复到新版本。
	
<h4>工作区和暂存区</h4>

###### 工作区（Working Directory）

* 就是电脑里一开始创建的空目录

###### 版本库（Repository）
* 工作区有一个隐藏目录工作区有一个隐藏目录<code>.git</code>

* Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。	

* 前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

	1.	第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；

	2. 第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

	3. 因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。

	4. 你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

<h4>撤销修改</h4>

1. 如果当前你修改的内容还没有添加到暂存区，也就是在<code>git add</code>之前，我们可以使用
	<pre>
		$ git checkout -- <'filename'>
	</pre>
	来撤销在工作区所做的修改。
2. 如果当前修改的内容已经执行了<code>git add</code>了，但是还没有执行<code>git commit</code>，那么我们可以使用<pre>git reset HEAD <'filename'></pre>把暂存区的修改撤销掉，重新放回工作区，然后再执行<pre>git checkout -- <'filename'></pre>就可以了.

<h4>删除文件</h4>

* 如果在工作区删除了一个文件，这个时候，git知道你删除了文件，因此，工作区和版本库的内容就不一致了。如果确实要从版本库中删除该文件，那就使用
	<pre>
		$ git rm <'filename'>
	</pre>
命令来删除版本库中的文件，并且提交。如果把工作区的文件误删了，则使用<pre>
	$ git checkout -- <'filename'>
</pre>
来还原就OK了。
<code>git checkout</code>其实使用版本库里的版本替换工作区的版本，无论工作区修改还是删除，都可以一键还原。

<h4>远程仓库</h4>

* Git是分布式版本控制系统，同一个Git仓库，可以分布到不同的机器上。	怎么分布呢？最早，肯定只有一台机器有一个原始版本库，此后，别的机	器可以“克隆”这个原始版本库，而且每台机器的版本库其实都是一样的，	并没有主次之分。
* 我们可以通过github网站来创建远程仓库。

	由于本Git仓库和github仓库之间的传输是通过ssh加密的,所以需要一些设置：
	1. 创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再	看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有	了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git 	Bash），创建SSH Key：
	<pre>
		$ ssh-keygen -t rsa -C "youremail@example.com"
	</pre>
	你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即	可，由于这个Key也不是用于军事目的，所以也无需设置密码。
	如果一切顺利的话，可以在用户主目录里找到<code>.ssh</code>目	录，里面有<code>id\_rsa</code>	和<code>id\_rsa.pub</code>两个	文件，这两个就是SSH Key的秘钥对，<code>id\_rsa</code>是私	钥，不能泄露出去，<code>id\_rsa.pub</code>是公钥，可以放心地告	诉任何人。
	2. 登陆GitHub，打开“Account settings”，“SSH Keys”页面：然	后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴	<code>id\_rsa.pub</code>文件的内容：
	
	
	<img src="https://cdn.webxueyuan.com/cdn/files/attachments/001384908342205cc1234dfe1b541ff88b90b44b30360da000/0" />
	
	
	点“Add Key”，你就应该看到已经添加的Key：
	
	
	<img src="https://cdn.webxueyuan.com/cdn/files/attachments/0013849083502905a4caa2dc6984acd8e39aa5ae5ad6c83000/0" />
	
	为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确	实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub	只要知道了你的公钥，就可以确认只有你自己才能推送。
	
	首先，登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库.
	
	把本地仓库的内容推送到GitHub仓库
	
	<pre>
		$ git remote add origin https://github.com/praycis-lee/git-practice.git
	</pre>
	
	添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别	的，但是origin这个名字一看就知道是远程库。
	
	下一步，就可以把本地库的所有内容推送到远程库上：
	
	<pre>
		$ git push -u origin master
	</pre>
	
	由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git	不但会把本地的master分支内容推送的远程新的master分支，还会把本	地的master分支和远程的master分支关联起来，在以后的推送或者拉取	时就可以简化命令。
	
	从现在起，只要本地作了提交，就可以通过命令：
	
	<pre>
		$ git push origin master
	</pre>

	把本地master分支的最新修改推送至GitHub，现在，你就拥有了真正的	分布式版本库！

<h4>分支管理</h4>

* 每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止	到目前，只有一条时间线，在Git里，这个分支叫主分支，即	<code>master</code>分支。<code>HEAD</code>严格来说不是指向提交，而是指向<code>master</code>，<code>master</code>才是指向提交的，所以，<code>HEAD</code>指向的就是当前分支。
	
* 一开始的时候，master分支是一条线，Git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点：
	<img src="https://cdn.webxueyuan.com/cdn/files/attachments/0013849087937492135fbf4bbd24dfcbc18349a8a59d36d000/0">	
* 每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长
* 当我们创建新的分支，例如dev时，Git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上：

	<img src="https://cdn.webxueyuan.com/cdn/files/attachments/001384908811773187a597e2d844eefb11f5cf5d56135ca000/0">

* Git创建一个分支很快，因为除了增加一个dev指针，改改HEAD的指向，工作区的文件都没有任何变化！

* 不过，从现在开始，对工作区的修改和提交就是针对dev分支了，比如新提交一次后，dev指针往前移动一步，而master指针不变：	<img src="https://cdn.webxueyuan.com/cdn/files/attachments/0013849088235627813efe7649b4f008900e5365bb72323000/0">
* 假如我们在dev上的工作完成了，就可以把dev合并到master上。Git怎么合并呢？最简单的方法，就是直接把master指向dev的当前提交，就完成了合并：

	<img src="https://cdn.webxueyuan.com/cdn/files/attachments/00138490883510324231a837e5d4aee844d3e4692ba50f5000/0">
	

	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	