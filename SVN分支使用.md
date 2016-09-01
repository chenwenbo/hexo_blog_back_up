title: SVN分支使用
date: 2016-01-19 10:27:15
tags: SVN
categories: 工具
---
 
总结：
    开发过程中，经常会有一些新功能的迭代，但是为了不影响主干分支的代码，这个时候我们通常会拉一个分支出来，完成功能的迭代之后再对主干分支进行合并。这样做的好处可以避免新功能的开发影响到主分支的功能。正文介绍了如何使用eclipse的SVN插件进行分支的操作及合并。


首先说下为什么我们需要用到分支-合并。比如项目demo下有两个小组，svn下有一个trunk版。由于客户需求突然变化，导致项目需要做较大改动，此时项目组决定由小组1继续完成原来正进行到一半的工作【某个模块】，小组2进行新需求的开发。那么此时，我们就可以为小组2建立一个分支，分支其实就是trunk版【主干线】的一个copy版，不过分支也是具有版本控制功能的，而且是和主干线相互独立的，当然，到最后我们可以通过【合并】功能，将分支合并到trunk上来，从而最后合并为一个项目。 

下面是在eclipse下使用subeclipse插件详细使用过程： 
首先建立一个工程，名字叫Facebook 
1.建立分支，为新的分支指定访问URL：Facebook3[注释不要忘了] 
 

 

 

2.建立好分之后，使用“切换”功能切换到分支下进行开发。 
 

 
我新建了一个FB3.html的文件并在分支下进行提交。 
 

 

3.切换回trunk版【即URL为Facebook的版本】 
 
你会发现trunk版里并没有出现我们刚刚提交的FB3.html，因为FB3.html是属于分支的，接下来我们要做的就是“合并”，通过合并，我们可以将分支下进行的更改合并到trunk版里。 
 

 

下面是合并的主要配置： 
起始路径：trunk版的路径【若需要把trunk版的改动合并到分支则相反】 
目标路径：从哪里获取改动【这里是分支路径】 
你可以使用指定的版本号，这里采用最新修订版。 

 

4.点击合并，你会发现trunk版下新增了一个文件FB3.html 
这样我们就将分支下所做的改动合并到了trunk版里。 
 

值得注意的是： 
1.在建立分支的时候最好添加注释。 
2.进行合并前最好保证两个版本都是干净的【即没有未提交或者冲突的文件存在】 
3.合并时的目标路径：需要把谁的改动合并到其他版本就填谁的URL。 


整个过程的SVN命令行输出如下： 
Xml代码  

	1. copy -rHEAD svn://192.168.1.192/placii/staggingarea/xiangqi/Facebook svn://192.168.1.192/placii/staggingarea/xiangqi/Facebook3  
	2. propset subclipse:tags "1538,Facebook2,/Facebook2,branch  
	3. 1540,Facebook3,/Facebook3,branch" E:/myeclipse/workspace/Facebook  
	4. switch svn://192.168.1.192/placii/staggingarea/xiangqi/Facebook3 E:/myeclipse/workspace/Facebook -rHEAD  
	5.     At revision 1541.  
	6. add -N E:\myeclipse\workspace\Facebook\WebRoot\FB3.html  
	7.     A         E:/myeclipse/workspace/Facebook/WebRoot/FB3.html  
	8. commit -m "" E:/myeclipse/workspace/Facebook/WebRoot/FB3.html  
	9.     Adding         E:/myeclipse/workspace/Facebook/WebRoot/FB3.html  
	10.     Transmitting file data ...  
	11.     Committed revision 1542.  
	12. switch svn://192.168.1.192/placii/staggingarea/xiangqi/Facebook E:/myeclipse/workspace/Facebook -rHEAD  
	13.     D  E:/myeclipse/workspace/Facebook/WebRoot/FB3.html  
	14.     Updated to revision 1542.  
	15.     ===== File Statistics: =====  
	16.     Deleted: 1  
	17. merge svn://192.168.1.192/placii/staggingarea/xiangqi/Facebook@HEAD svn://192.168.1.192/placii/staggingarea/xiangqi/Facebook3@HEAD E:/myeclipse/workspace/Facebook  
	18.     A  E:/myeclipse/workspace/Facebook/WebRoot/FB3.html  
	19.     Merge complete.  
	20.     ===== File Statistics: =====  
	21.     Added: 1  



希望本文能有所帮助。 
其他参考资料： 
http://www.iteye.com/wiki/subclipse/1626-subclipse-getting-started-guide-and-reference-c 



===========================关于合并========================== 
我在合并的时候发现，合并后文件被直接覆盖掉了，而没有出现本该出现的【冲突】，后来经过仔细研究发现，是操作问题。 

 
假设我原来的项目是placii,建立了一个分支是placiiStore.现在需要将分支placiiStore合并到主干线上。那配置应该如图所示 
1.【起始路径】：这里需要填分支的路径。 
2.第一个修订号：建立分支时的版本号。在建立分支时候记录下svn的console 
我的是
Xml代码  

	1. copy -rHEAD svn://192.168.1.192/placii/trunk/code/server/source%20code/placii svn://192.168.1.192/placii/trunk/code/server/source%20code/placiiStore  
	2. propset subclipse:tags "1527,placiiStore,/source code/placiiStore,branch  
	3. 1549,placiiStore,/source%20code/placiiStore,branch" E:/myeclipse/workspace/placii  
	4. switch svn://192.168.1.192/placii/trunk/code/server/source code/placiiStore E:/myeclipse/workspace/placii -rHEAD  
	5.     At revision 1550.  


3.目标路径：这里使用起始路径。 
4.目标版本号：使用最新版即 HEAD. 

点击合并，如果有人在主干线版本上做了更改，而你再分支上也对这个文件作了更改，将会产生冲突。然后手动把冲突的代码合并一下，右键-标记为解决，这就达到我们的目的了。 


参考链接：http://energykey.iteye.com/blog/512745
