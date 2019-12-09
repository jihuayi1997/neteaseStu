# 常用git指令

1. 设置仓库名字邮箱：（--global表机器上所有仓库都使用这个配置）

   `git config --global user.name jihuayi`

   `git config --global user.email jihuayi@corp.netease.com`

2. 将一个目录变为git仓库：

   `git init`

3. 将read.txt放入git仓库：

   `git add readme.txt`（将修改放到暂存区）

   `git commit -m “wrote a readme file”`（提交所有修改到当前分支）

4. 查看仓库当前状态：

   `git status`

5. 查看修改的内容：

   `git diff readme.txt`

6. 版本控制系统日志：（--pretty=oneline表输出信息简化一行）

   `git log --pretty=oneline`

7. 回退版本：

   `git reset --hard HEAD^`

8. 去指定版本：（后面是版本id）

   `git reset --hard 1094a`

9. 查看命令历史：

   `git reflog`

10. 暂存区的修改撤销回工作区：

    `git reset HEAD readme.txt`

11. 丢弃工作区的修改：

    `git checkout -- readme.txt`（回到暂存区/版本库状态）

12. 从版本库删除文件：

    `git rm test.txt`

    `git commit -m “remove test.txt”`

13. 创建sshkey：（user/.ssh目录下的.pub可上传至远程仓库）

    `ssh-keygen -t rsa -C jihuayi@corp.netease.com`

14. 添加新远程库：（给url添加别名origin）

    `git remote add origin git@github.com:jihuayi1997/learngit.git`

15. 克隆远程库到本地：

    `git clone git@github.com:jihuayi1997/gitskills.git`

16. 创建分支并切换：

    `git switch -c dev`（相当于`git branch dev`+` git switch dev`）

17. 查看本地所有分支：（包括本地的远程分支）

    `git branch -a`

18. 将其他分支工作成果合并到当前分支： 

    `git merge dev`

* master修改dev也修改（有冲突）：返回错误，需手动调整

* master修改dev也修改（无冲突）：不使用ff，生成新版本，弹出窗口输入-m描述

* master修改dev不修改，无变化

* master不修改dev修改，默认使用ff 

19. 删除指定分支：

    `git branch -d dev`

    `git branch -D feature-vulcan`（强行丢弃未被合并过的分支）

20. 版本控制系统分支图：

    `git log --graph --pretty=oneline --abbrev-commit`

21. 储存现场工作：

    `git stash`

22. 恢复现场工作并删除stash：

    `git stash pop`（相当于`git stash apply`+` git stash drop`）

23.查看stash：

​	`git stash list`

24.把某修改过程“复制”到当前分支：（后面是版本id）

​	`git cherry-pick 4c805e2`

25.查看当前的远程库：

​	`git remote -v`

26. 创建本地分支关联本地的远程分支：

    `git switch -c dev origin/dev`

27. 本地推送远程：（有冲突会出错提示先pull后手动调整）

    `git push <远程主机名> <本地分支名>:<远程分支名>`

* 省略本地分支名：删除指定远程分支

* 省略远程分支名：将本地分支推到远程同名分支，没有则新建

28. 远程拉取本地：（相当于fetch+merge，有冲突会出错提示手动调整）

    `git pull <远程主机名> <远程分支名>:<本地分支名>`

* 省略本地分支名：拉取到当前分支

29. 打标签：

    `git tag v1.0`（默认打在最新commit上）

30. 显示所有标签：

    `git tag`

31. 给指定版本打标签：

    `git tag v0.9 f52c633`

32. 查看标签信息：

    `git show v0.9`

33. 创建带说明的标签：

    `git tag -a v0.1 -m “version 0.1 released” 1094adb`

34. 删除本地标签：

    `git tag -d v0.1`

35. 推送标签到远程：

    `git push origin v1.0`

    `git push origin --tags`（推所有）

    `git push origin :refs/tags/v0.9`（删除远程）

36. 忽略某些文件时需要写 .gitignore并传到版本库

37. 配置别名：（全局：uer/.gitconfig 非全局：.git/config）

    `git config --global alias.unstage ‘reset HEAD’`

38. 子模块更新：

    `git submodule update`