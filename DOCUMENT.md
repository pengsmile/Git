# 前端开发流之Git

标签（空格分隔）： Git

---

本文将略过安装步骤，直接进入主题

安装后先配置Git

> git config --global user.name "My Name"
  git config --global user.email myEmail@example.com

 - 创建一个新仓库 
    首先新建目录,在其内右击打开命令窗口
> git init

    初始化后会在文件夹内出现一个隐藏git文件，所有git需要的数据都会存放在这个目录里

 - 检查状态
    git status是一个非常重要的命令，可以查看仓库当前状态是否为最新代码
    git status -s 以精简的方式显示文件状态
    A：新文件第一次被添加进版本管理 
    M:红色为修改过未被添加进暂存区的，绿色为已经添加进暂存区的
>  $ git status
    On branch master
        Initial commit
    Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   index.html

    告诉我们新文件index尚未跟踪，这是因为这个文件是新的，git不知道是应该跟踪它的变动呢，还是直接忽略不管呢。为了跟踪我们的新文件，我们需要暂存它。


----------


 - 暂存区
    在git内有个概念叫暂存区
    > git add .  【它会监控工作去状态树，使用它会把工作时所有变化提交至暂存区，不包括被删除文件】
      git add -u 【它仅监控已经被add的文件，被修改后提交至暂存区，注！不会提交新文件】
        git add -A 【是上面两个功能的合集（git add --all的缩写）】

 - 提交Commit
    > git commit -m 【用于提交暂存区的文件,将暂存区内容添加到仓库中】
    Git 为你的每一个提交都记录你的名字与电子邮箱地址，所以第一步必需配置用户名和邮箱地址。
    git commit -a 【允许直接跳过git add】
    $ git commit --amend -m [message] 【使用一次新的commit，替代上一次提交】

 -  取消已暂存内容
    简而言之，执行 git reset HEAD 以取消之前 git add 添加，但不希望包含在下一提交快照中的缓存。


----------


 - 删除文件
    要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除，然后提交

    > git rm <file>
    
    如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f
    
    > git rm -f <file>
    
    如果把文件从暂存区域移除，但仍然希望保留在当前工作目录中，换句话说，仅是从跟踪清单中删除，使用 --cached 选项即可
    > git rm --cached <file>
    
    不从工作区中删除文件：

    > git rm --cached README 
        rm 'README'
        $ ls
        README

    可以递归删除，即如果后面跟的是一个目录做为参数，则会递归删除整个目录中的所有子目录和文件：

    > git rm –r * 


----------


 - 移动或重命名
    git mv 命令用于移动或重命名一个文件、目录、软连接。


----------


 - 分支Branch
    >  git branch 【可以用来列出分支,创建分支和删除分支;列出本地所有分支,当前分支会被星号标示出,后面带名字即创建分支】
       git branch -v 【可以看见每一个分支的最后一次提交】
       git branch -r 【查看远程版本分支列表】
       git branch -a 【查看所有分支列表，包括本地和远程】
       git branch -d dev  【删除dev分支，如果在分支中有一些未merge的提交，那么会删除分支失败，此时可以使用 git branch -D dev：强制删除dev分支】
       git branch -vv  【可以查看本地分支对应的远程分支】
       git branch -m oldName newName 【分支重命名】
       git checkout - 【切换到上一个分支】
       git push origin --delete [branch-name] 【删除远程分支】
       git branch -dr [remote/branch] 【删除远程分支】
       git merge 分支名 【用于合并指定分支到当前分支】
       git checkout -b [branch] 【新建一个分支，并切换到该分支】
       git branch --track [branch] [remote-branch] 【新建一个分支，与指定的远程分支建立追踪关系】
       git checkout [branch-name] 【切换到指定分支，并更新工作区】
       git cherry-pick [commit] 【选择一个commit，合并进当前分支】


----------


 - 分支切换
    > git checkout master 【切换到主分支】
      git checkout -- file 【当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时】
      git reset HEAD file 【当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步,第一步使用此命令，第二布按上一个命令即可】
      git checkout -b  分支名 【创建并切换分支】


----------


 - 远程仓库
    为了便于管理，Git要求每个远程主机都必须指定一个主机名。git remote命令就用于管理主机名。
    git remote 命令即可，就会列出远程库的名字。在我们clone了刚才的项目之后，默认会看到一个origin的远程仓库。
    
    > git remote add name url 【添加远程仓库，一旦添加以后别名就代表远程库的版本库地址，可以直接使用。】

    可以添加多个地址
    删除某个远程仓库地址
    > git remote remove name
    
    重命名远程库
    
    > git remote rename 原名字 新名字
    
    查看指定主机的详细信息
    
    >  git remote show 主机名
    


----------


 - 推送 PUSH
    git push命令会有两个参数，远端仓库的名字，以及分支的名字
    git push <远程主机名>   <本地分支名>:<远程分支名>。
    注！这里的主机名使用上面配置好的origin
    git push origin master 将本地的master分支推送到了远程库的master分支，**如果远程分之不存在将会被创建**。
    如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。注意第一个分支是不可以被删除的，我第一个创建的分支是joey/master
    如果省略远程分支名，则表示将本地分支推送到与之存在Tracking的远程分支。 （后面的：和远程分支名字省略）
    
    > git push origin test:master 【提交本地test分支作为远程的master分支】
      git push origin test:test  【提交本地test分支作为远程的test分支】
      git push origin :test  【刚提交到远程的test将被删除，但是本地还会保存的】


----------


 - pull 拉取
    
    > git pull origin 分支名 【将远程存储库中拉取至本地，也可以说关联】
      git fetch origin master 【相当于是从远程获取最新版本到本地，不会自动合并】
      git merge 【合并分支】


----------


 - 回滚
    ### 恢复暂存区的指定文件到工作区
    $ git checkout [file]
    
    ## 恢复某个commit的指定文件到暂存区和工作区
    $ git checkout [commit] [file]
    
    ## 恢复暂存区的所有文件到工作区
    $ git checkout .
    
    ## 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
    $ git reset [file]
    
    ## 重置暂存区与工作区，与上一次commit保持一致
    $ git reset --hard
    
    ## 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
    $ git reset [commit]
    
    ## 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
    $ git reset --hard [commit]
    
    ## 重置当前HEAD为指定commit，但保持暂存区和工作区不变
    $ git reset --keep [commit]
    
    ## 新建一个commit，用来撤销指定commit
    ## 后者的所有变化都将被前者抵消，并且应用到当前分支
    $ git revert [commit]
    
    ## 暂时将未提交的变化移除，稍后再移入
    git stash
    git stash pop
    