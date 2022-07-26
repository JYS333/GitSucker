# GitSucker

## git基础操作

### 刚开始使用git (beginer)

- 创建一个新的仓库 (create a new repository on the command line)

```
echo "# 仓库名" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://xxxxxxxxx
git push -u origin main
```


- 自己创建了项目并关联已经新建的远程仓库，创建仓库操作可以直接在github上 (push an existing repository from the command line)

```
git remote add origin https://xxxxxxxxx
git branch -M main
git push -u origin main
```


- 克隆远程仓库到本地

```
git clone https://xxxxxxxxx
```

- 查看远程仓库地址，拉取当前分支

```
git remote -v                                    查看远程仓库地址
git remote update origin --prune                 更新远程仓库地址
git fetch                                        更新本地的远程仓库信息
git pull                                         拉取远程分支
git pull origin 远程分支名                        拉取指定的远程分支
```

- 任意修改后，将修改文件添加至暂存区

```
git add .                                        添加本地修改至本地仓库

git restore —staged 文件名                        取消add
```

- 提交修改

```
git commit -m “xxxxxxx”                          commit操作，备注必写

git commit --amend                         	     修改commit文本
i + esc + :wq                                    进入编辑模式，退出并保存
```

- 推送

```
git push                                         推送到当前分支
git push origin 远程分支名                        推送到指定远程分支
git push origin HEAD:远程分支名                   推送到指定远程分支
```

---



### 本地分支 & 远程分支 (branch)

- 查看分支

```
创建仓库：在你的名下new一个repo后clone到本地，然后⬇️
git branch                                        查看本地分支
git branch -a                                     查看远程分支
```

- 删除分支

```
git branch -d 本地分支名                            删除出当前本地分支外的一个本地分支，-D强制删除
git push origin —delete 远程分支名                  删除一个指定的远程分支（慎用）

ps: `-d` 前确保本低分支已无可用提交，git也会提醒你，`-delete` 可以直接在repo上操作，不可撤销，谨慎使用
```

- 切换分支

```
git checkout 远程分支名                             切换到当前远程分支并创建同名本地分支
git checkout 本地分支名                             切换到该本地分支

ps: checkout 会自动识别是否存在远程分支，如果存在，则创建本地分支并关联同名远程分支，否则只创建本低分支，因此使用前确保 fetch 到最新的远程仓库信息
```

- 创建自己的本地分支并主动关联至远程分支

```
git checkout -b 本地分支名                          新建本地分支
git push origin 远程分支名                          新建远程分支
git branch --set-upstream-to=origin/远程分支名 本地分支名    

git branch --set-upstream-to 本地分支名 origin/远程分支名 重新关联新的远程分支

ps: 本地创建远程分支操作复杂且易混乱，建议直接在仓库创建，使用 fetch + checkout 管理。
但前提是公司内git仓库允许手动操作，否则github默认不允许手动创建、删除分支。
```

---



### 当你修改了代码 (modified)

- 查看修改了哪些文件

```
git status                                         查看本地文件修改列表
git diff 文件名                                     查看该文件修改细节，vscode可以直接在左边source control处看到
```

- 删除未保存到缓存区的修改

```
git checkout .                                     舍弃全部本地未add的修改（慎用）
git checkout 文件名                                 舍弃某一文件的修改
```

- 暂存修改 (stash)，stash后即可切换到其他分支工作

```
git stash save “备注”                              暂存当前本地分支修改，用来切换分支前使用
git stash pop                                     将先前save的修改恢复
git stash list                                    查看当前stash了哪些
git stash drop xxx                                删除指定stash

ps: stash pop时不要pop错分支
```

---



### 当你推拉代码时需要查看配置 (config)

- 查看config

```
git config —list                                  查看当前git配置表
```

- 指定本地的用户名、邮箱、密码等

```
git config user.name <用户名>                      更改用户名、邮箱、密码
git config user.email <用户邮箱>
git config user.password <用户密码>

git commit —amend —reset-author                   重置当前commit的username/email
```

- 大公司git repo，对权限校验比较严格

```
git config --global credential.helper store 完了pull一次输入账号密码以后就不用啦，前提是设置好了ssh key
本条针对JD：公司erp且mac电脑，修改记得先更新电脑开机密码，再改本地存储密码，再用自带terminal pull一遍，最后vscode才能用
```

---



### 当你的仓库是fork来的时 (upstream)

- fork的仓库，用以下方法保持同步更新

```
git remote -v                                    查看当前仓库的fetch push对象
git remote add upstream 原仓库地址                关联原仓库至upstream
git fetch upstream 
git pull upstream 原仓库分支名 即可更新
```



-----------------------------------------------------------

<font color=#F5F8FA>======================================= 这是一条假装很好看的分割线 =======================================</font>

-----------------------------------------------------------



## git高级操作

### 当你想把一个分支的某个commit提交到另一个分支 (cherry pick)

```
Step 0. git log                                    记一下要重新push的commit SHA
Step 1. git checkout 本地分支名                      切换到要提交的那个分支上
Step 2. git pull                                   更新最新内容
Step 3. git cherry-pick 上面拿到的SHA                把该次commit转移过来
Step 4. git log                                    查一下是不是加进去了
Step 5. git push
```

---



### revert & reset

- 先了解HEAD概念：HEAD为指针，指向最新一次commit，可以变更，配合下面操作实现回撤commit

- git revert

```
git revert HEAD^                                  撤销前一次commit
git revert HEAD^^                                 撤销前前一次commit
git revert commit SHA                             git log里拿到的SHA或者reflog拿到的短的或者远程提前后面那串短的

ps: git revert是提交一个新版本，这个新版本把要revert的内容反向修改回去，俗称中和，会改变源码，但不影响之后提交的内容
```

- git reset

```
git reset HEAD^                                   指针指向上一次commit
git reset HEAD^^                                  指针指向上上一次commit
git reset SHA                                     指向某一次commit

git reset xxx --soft                              soft会将之前的修改返回到本地
git reset xxx --hard                              hard会将之前的修改直接删除

ps: 将HEAD指向之前的commit，或指定的commit，并回退到那个版本，其之后的commit也被回退。慎用，可能会把别人的commit也给退回去，记录参考log。

```

---



### rebase & merge

- merge会保留所有的commit记录，但也会污染commit记录

- rebase是将待合并分支的commit拼接到原分支上，可以选择要提交的commit记录

```
举个🌰
1-2-3 是现在的分支状态
这个时候从原来的 master , checkout 出来一个 prod 分支
然后 master 提交了4.5，prod 提交了6.7
这个时候 master 分支状态就是1-2-3-4-5，prod 状态变成1-2-3-6-7
如果在 prod 上用 rebase master , prod 分支状态就成了1-2-3-4-5-6-7
如果是 merge
1-2-3-6-7-8
........ |4-5|
会出来一个8，这个8的提交就是把4-5合进来的提交

ps: rebase并不适用于多人维护的分支，原因尚未实操核实，以目前的开发经验来看，merge确实会污染合并提交记录，但会完整保留全部记录。多人开发的分支一般都用merge。
```

