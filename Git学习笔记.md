title: Git学习笔记
date: 2016-06-28 18:40:53
tags:
---
### 初始化git仓库
* git init
### 查看项目状态 
* git status 
### 添加文件到staging area
* git add "filename.txt"
### 提交文件到repostory
* git commit -m "注释"
### 利用通配符添加文件到staging area
* git add "*.txt"
### 提交所有变动文件
* git commit -m “Add all changed file”
### 查看log
* git log
### 提交本地仓库到远端github仓库
* git remote add origin https://github.com/try-git/try_git.git
### 推送本地仓库的改变到远端仓库 -u指定默认分支
* git push -u origin master
### 拉远端最新代码到本地
* git pull origin master
### 查看文件变化
* git diff head
### 添加文件夹的文件
* git add folder/file.txt
### 查看staged的变化
* git diff --staged
### 移除stageing area的文件
* git reset file.txt
### 检出文件
* git checkout -- file.txt
### 创建分支
* git branch branch_name
### 切换分支
* git checkout branch_name
### 删除文件（并未删除磁盘中文件）
* git rm '*.txt'
### 提交分支中所有文件
* git commit -m "commit files"
### 切换为主分支
* git checkout master
### 合并分支
* git merge branch_name
### 删除分支
* git branch -d branch_name
### 推送本地仓库到远端
* git push




