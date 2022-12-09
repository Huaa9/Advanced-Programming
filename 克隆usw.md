# SSH密钥
```c++
//在终端输入
ssh-keygen
//接受默认位置并输入密码
ls ~/.ssh
// ctrl+h打开隐藏文件，找到.ssh。在git上将.pub内容复制进ssh密钥
```
# 克隆
安装git `sudo apt intall git`
1. 网页https克隆或者ssh克隆，后者往后不用输入密码了。
2. `lone 网页 or ssh`根目录下将自动生成文件夹。
3. `cd`入文件夹，`ls`列出当前根目录下文件。
## 新文件/vim指令
4. 用vim可以建立新的file。eg. `vi helloWorld.txt ` i, esc, :wq。
5. `git status`显示当前状态，与之前main中不同的文件会被标红。
6. `git add helloWorld.txt`上传文件helloWorld.txt至缓存区。`git add ./`add all
7. `git commit -m"想说的话"`提交文件，写提交了什么，退出即结束。
## push
8. `git push`push文件进git lab项目。
  ```c++
    git push -u origin new_branch //origin为默认储存库名字，当前分支推送到远程仓库
    git push origin originrName:new_branch; //关联
    git checkout -b 本地分支 origin/远程分支 //创建并关联 
  ```
## 分支
9. `git branchn`显示所有分支。
10. `git branch new_branch`创建分支。`git checkout -b new_branch`创建分支并转到。
11. `git checkout new_branch` or `switch new_branch`
## merge
12. 在main分支下写`git merge new_branch`表示将new_branch并入main。
13. `git merge --abort`取消merge。
## conflict
14. `git log --graph --pretty=oneline --abbrev-commit`分支图
15. 修改代码，解决问题。
16. `git branch -d new_branch`删除分支。`给i他铺设origin--deletenew_branch`删除远程分支。`git push origin :name`可以直接推送空分支。
## 其他
1. `git branch -a`查看远程分支
2. `git pull`下载远程代码并合并。
3. `rm helloWorld.txt`移除文件。

[回到顶部](#ssh密钥)字母小写
