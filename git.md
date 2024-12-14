### git 常用命令

1. git 全局配置

   ```
   git config --global user.name "yourname"
   git config --global user.email "youremail"
   git config --global credential.helper store   #git本地记住密码	
   git remote add origin https://github.com/OliveKong/poster.git    设置本地项目的远程git仓库地址
   ```

2. 拉取分支

   ```
   git checkout -b    本地分支名 origin/远程分支名
   git clone originUrl   默认拉取的Origin上面的master分支；
   git clone -b branchName  cloneURL;
   
   git pull <远程库名> <远程分支名>:<本地分支名>
   git pull 默认情况下，如果我们的本地分支名与远程分支名是一样的，且已经建立追踪，直接使用
   git pull origin master  接取远程origin的master分支到当前分支
   git pull origin dev; 拉取远程的dev分支到本地分支

   git reset --hard origin/main ;    
   ```

3. 分支管理

   ```
   git branch iss53     创建新分支iss53；
   git checkout iss53   切换到新分支iss53
   git merge  dev    将dev分支合并到当前分支；
   git branch -d  分支名；  删除本地分支；
   git push origin  --delete 分支名；  删除远程分支
   ```

4. 从远程仓库clone一个项目

   ```
   git 本地创建一个新项目
   git clone git@gitlab.com:villiamAB/play-ball.git
   cd play-ball
   touch README.md
   git add README.md
   git commit -m "add README"
   git push -u origin master
   ```

5. 将本地的已有的项目提交到远程空仓库

   ```
   cd existing_folder
   git init
   git remote add origin git@gitlab.com:villiamAB/play-ball.git
   git add .
   git commit -m "Initial commit"
   # git pull origin master --allow-unrelated-histories 将本地仓库与远程仓库关联起 ；
   git push -u origin master
   ```
   
6. 将本地已有的git项目提交到远程空仓库
    ``` 
    cd existing_repo
    git remote rename origin old-origin
    git remote add origin git@gitlab.com:villiamAB/111.git
    git push -u origin --all
    git push -u origin --tags
    ```

7. git 从版本库中删除已经提交的文件 
   ```
   git rm --cached  fileName   删除指定的文件 
   git rm --cached .  删除全部文件 
   ```

8. git 本地忽略跟踪某个文件 
   ```
   git update-index --assume-unchanged /path/to/file 
   ```

9. git 本地恢复跟踪
   ```
   git update-index --no-assume-unchanged /path/to/file  #恢复跟踪
   ```

10. 查看本地忽略的文件列表
   ```
   git ls-files -v | grep '^h\ '

   git ls-files -v | grep '^h\ ' | awk '{print $2}'
   ```
