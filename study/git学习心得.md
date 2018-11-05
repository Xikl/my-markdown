# git学习心得
#### 1.git中 查看版本 
    git log 
    git log --pretty=oneline
#### 2.git回退版本
     git reset --hard HEAD^
- 上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100

#### 3.git中 cat的可以查看文件信息
    cat readme.md
#### 4.git 中reflog 记录每一次命令
    git reflog
#### 5.git 删除当前工作区的修改
    git checkout -- README.md
#### 6.git add 后 没有commit 想要删除修改
    - git reset HEAD README.md
    - git checkout -- README.md
#### 7.git 已经提交了 但是又想删除修改怎么办
- 可以直接回退版本 @see 2

#### 8.删除文件
    git rm test.txt
- 进行这个操作默认为 就提交了 比我们自己手动删除 在add . 然后commit 要方便一点

#### 9.强制 上传 
- 错误原因
    + 情景：
    在github上创建项目，然后本地git init
    然后没有git pull -f --all
    然后git add .  | git commit -am "init"
    导致github上的版本里有readme文件和本地版本冲突，下面给出冲突原因：
```
    git push -u origin master -f 
```

#### 10.将origin master  关联起来
```
git push -u origin master
```
#### 11. 使用https除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用ssh协议而只能用https。
- Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

#### 12.创建分支
- 创建分支
```
git checkout -b dev  
```
- git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
```
git branch dev
git checkout dev
```
- 使用 git branch 查看分支
- 合并分支中的操作 
```
git merge dev
```
- 注 操作合计：
```
Git鼓励大量使用分支：

查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>
```
```
git checkout master
git pull 
git merge develop
git status
git push
```