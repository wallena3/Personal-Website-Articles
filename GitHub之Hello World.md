Hello World项目是在计算机编程领域历史悠久的传统。它是一个简单的练习，来使你在学习一些新的东西时起步。让我们开始学习GitHub吧～～



你将会学到如何:

建立和使用仓库(repository)
开启并管理一个新的分支(branch)
对一个文件做出改变并且把他们提交(commits)到GitHub
打开并合并拉(pull)请求
什么是GitHub？



GitHub是一个为版本控制和合作工作而生的代码托管平台。他可以让你和其他人，无论身在何处都可以一起工作。

本教程将教会您GitHubDE四要素:repositories, branches, commits, 以及pull请求。你将创建自己的Hello World仓库并且学习GitHub的pull请求工作流，一个流行的方式来创建和评审代码。

无需编程

完成此次教程，你需要一个GitHub账号，还有接入互联网。你不需要懂得如何编程，如何使用命令行，或者如何安装Git(GitHub建立在其之上的版本控制软件)

小贴士：在一个新的浏览器窗口(或者标签)打开本教程，这样你就可以在完成教程中的步骤时同时看到本教程。

步骤1:创建一个Repository

一个仓库通常用来组织一个单独的项目。仓库可以包含文件夹和文件，图片，视频，电子表格，数据集，任何你的项目需要的东西都可以放在其中。我们建议仓库应该包含一个README文件，或者一个关于你的项目的信息的文件。GitHub使得让你在创建一个新的仓库的同时很容易的加入自述文件。GitHub还提供了其他常用的选项，比如许可文件。

来创建一个新的仓库

在GitHub主页的右上角，你的用户名旁边，点击+然后点击New repository。
给你的仓库命名为hello-world。
写一个简短的描述。
选择初始化仓库时生成README。
create-new-repo

点击Create repository。:tada:

步骤2:创建一个Branch



分支是一种可以让你在同一时间对同一个仓库的不同版本上工作的方式。

默认情况下你的仓库有一个名为"master"的分支，它被认为是决定性的的分支。在提交代码到master之前，我们使用分支来进行实验，做出更改。

当你创建一个master的分支时，你在做master在一个时间点的一个拷贝，或者快照。如果你在属于你自己的分支中工作时，有其他人在mastert中做出了更改，你可以在这些更改中拉取(pull)。

这个图表明了:

一个master分支
一个名为feature的新分支(因为我们在这个分支上做特征工作)
feature在他合并到master之前所经历的旅程
branching

你是否保存过不同版本的文件？像是：

story.txt
story-joe-edit.txt
story-joe-edit-reviewed.txt
在GitHub仓库中，分支们完成相似的工作。

在GitHub上，我们的开发人员，作家，设计师使用分支来保证bug的修复，并且让特征工作从master(产品)分支中分离出来。当一个更改准备完毕时，我们把他们的分支合并到master中。

创建一个新的branch

前往你的新仓库hello-world
点击写着branch:master的下拉菜单
在新建分支的text中输入readme-edits
选择蓝色的Create branch方框，或者敲回车
readme-edits



现在你已经有两个分支了，master和readme-edits。他们看起来很相似，但是不会持续很久的。下一步我们将在新分支中加入我们的改变。

步骤3:做出更改和commit



太棒了！现在你正在master的拷贝—readme-edits分支的代码视图中。让我们来做一些编辑。

在GitHub中，保存更改被称作commits。每一个commit都有一个与之相关的commit信息，这个信息是来描述为什么做出一个特定的更改。commit信息捕获你做出更改的历史，这样一来其他的代码贡献者可以知晓你做了什么，以及为什么这么做。

做出更改和commit

点击README.md文件
在文件编辑视图的右上角点击铅笔图标
在编辑器中写一些关于你自己的信息
写一个commit信息来描述你的更改
点击Commit changes按钮
commit

这些更改只会在你的readme-edits分值中的README文件生效，所以这个分支现在含有和master不同的内容了。

步骤4：开启一个pull请求

超棒的编辑！既然你已经更改了一个master的分支，你可以开启一个pull请求。

pull请求是在GitHub上合作的核心。当你打开一个pull请求，你提出你的更改建议，请求别人评审，并pull你的贡献代码融合到他们的分支中。pull请求显示两个分支中内容的差异。这些更改中，增加的代码被标记成绿色，删除的代码被标记成红色。

当你做出了更改，你可以开启一个pull请求然后开始讨论，甚至在代码便携完成之前也可以。

通过使用在GitHub中的pull请求信息的通知系统，你可以对特定的人或者团队请求反馈，无论他们在大厅里或者是在10个时区之外。

你甚至可以在你自己的repository中开启pull请求，然后自己合并他们。这是在你参加更大的项目前学习GitHub工作流的一个很棒的方法。

针对README的更改开启一个Pull请求



步骤	截图示意
点击Pull Request选项卡，然后在这个页面点击绿色的New pull request按钮	pr-tab
选择你创造的分支，readme-edits,来和master比较(原始版本)	pick-branch
在比较页面检查更改的不同之处，确保这些内容是你想提交的	diff
当你对于你想提交的更改满意时，点击Creatr Pull Request这个大绿钮	create-pr
给你的pull请求起一个标题，然后对于你的更改写一份简短的描述	pr-form
当你完成了你的信息，点击Create pull request！

小贴士：你可以在评论以及pull请求中使用emoji以及拖放图片和gifs。

步骤5：合并你的Pull请求

在这最终的一步中，是时候让你的更改合并了—合并你的readme-edits分支到master分支中。

点击绿色的Merge pull request按钮来让更改合并入master中
点击Confirm merge
继续，删除这个分支，因为他的更改已经被合并了。使用紫色框中的Delete branch按钮来删除。
merge-button

delete-button

庆祝吧！

通过学完本教程，你已经学会如何创建项目，在GitHub上发起pull请求！:tada::octocat::zap:

这是你在本教程中所完成的任务：

创建一个开源的仓库
建立并且管理一个新的分支
更改文件并且把这些更改提交到GitHub上
开启并合并一个pull请求
看一下你的GitHub简介，你会发现你的新的贡献广场！

如果你想学习更多关于强大的pull请求的知识，我们推荐阅读GitHub流指南。也许你也想访问GitHub研究网站来参加一些开源项目:octocat:

小贴士：来看看我们其他的指导教程以及YouTube频道来得到更多GitHub操作说明



完

Reference:https://guides.github.com/activities/hello-world/
