<!--
 * @Author: 周炳艳 434387059@qq.com
 * @Date: 2023-11-10 16:20:21
 * @LastEditors: 周炳艳 434387059@qq.com
 * @LastEditTime: 2023-12-25 10:20:56
 * @FilePath: /utils/07.前端工程化/git.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
# git命令相关

### 删除本地分支
git branch -d +分支名称来删除本地分支


### 暂存
git stash save "暂存的备注"
查看暂存记录
git stash list
取回暂存代码
git stash pop
取消【取回暂存代码】
git reset --hard
清空暂存
git stash clear

### 提交当前正在开发的分支代码
git add .
git commit -m "xxx开发中"

git commit --no-verify -m "xxx开发中"

###  提交新开发分支至远程
git push -u origin fixbug_20181108

###  与开发分支操作一致
远程分支存在的情况
git checkout -b xxxx origin/xxx

回退版本 在git仓库要控制分支权限
git reset --hard HEAD^
git push -f origin master

正式版发布&打tag
上线正式环境时git的步骤及注意事项⚠️
###  查看当前分支下git的状态并保证所有文件无改动
git status
###  切换分支至master,打tag基于master版本
git checkout master
###  保证当前master分支是最新的
git pull origin master
###  拉取代码至本地仓库，保证新分支索引同步至本地且各个分支仓库的索引是最新的
git pull
###  合并开发分支至本地master仓库，如有冲突先解决冲突，然后push回master
git merge origin develop
git push origin master
###  以上操作完成后即可将代码发布至 alpha 环境
---
###  以下操作可将代码发布至 prod 环境
###  查看远程tag列表
git ls-remote --tags
###  打tag，jenkins prod版本必须基于tag才能发布上线
git tag -m "新增日志管理" 20221009.1 && git push origin --tag


### 删除本地tag和远程tag
git tag -d 20220910.1　　　　
git push origin :refs/tags/20220910.1


### git拉取远程分支
1.git clone ****
2.切换分支git checkout branch
3.拉取远端branch分支并合并到当前分支：git pull origin branch

### 重新获取远端分支
1.切换到其他分支：git checkout master
2.删除本地分支内容:git branch -d 分支名
3.建立本地分支并将远程分支拉取至本地
git checkout -b 本地分支名 origin/远程分支名

### 代码合并
1.将本地的branch分支合并到当前分支：git merge branch
2. 将某次提交的内容合并到当前分支：git cherry-pick

