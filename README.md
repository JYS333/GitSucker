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
git add .                                        添加所有本地修改至本地仓库
git add 文件名                                    添加某一文件的修改至本地仓库

git restore —staged 文件名                        取消add某一文件
```

- 提交修改

```
git commit -m “xxxxxxx”                          commit操作，备注必写

git commit --amend                         	     修改commit文本
i + esc + :wq                                    进入编辑模式，退出并保存
```

- commit规范，重点关注 `:` 前的<type>字段

```
标准格式：type（必需）、scope（可选）和subject（必需）空一行后 <body>(可选)
<type>(<scope>): <subject>
// 空一行
<body>

But...一般人不会写这么细，不然每次提交都累死，所以一般只需要关注 <type> 和 <subject> 即可

type种类：
feat：新功能（feature）
fix：修补bug
docs：文档（documentation）
style： 格式（不影响代码运行的变动）
refactor：重构（即不是新增功能，也不是修改bug的代码变动）
test：增加测试
chore：构建过程或辅助工具的变动
revert: feat(pencil): add 'graphiteWidth' option (撤销之前的commit)

以上只是基本通用的几个type，各个公司内部可能会有一些自己的拓展规定

一个🌰：git commit -m "feat: 新增首页按钮一键赚取100w美元的功能"
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

ps: stash pop时不要pop错分支，否则会有意想不到的conflict需要merge
```

- merge后的本地修改如果不想要，或者merge错了

```
git merge --abort                                撤销merge，使用该命令其实往往发生在你和另一位朋友在没有互相沟通的情况下私自去掉了部分对方的冲突代码，导致项目报错，然后还不得不revert回去再改一遍（笑，是吧龙哥
```

----



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



- Mac系统在使用git时常常会将自动生成的 `.DS_Store` 文件也上传至仓库，这是不应该的
- 该文件为Mac的desktop service文件，是本地用来规范桌面文件的一些自定义属性的，包括文件图标的位置、文件夹上次打开时窗口的大小、展现形式和位置等。这有助于保留为特定文件夹配置的设置，例如，将桌面文件夹设置为查看按名称排序的图标，同时将下载文件夹配置为将文件显示为列表并按日期排序，最近修改的先显示



- Step1. 使用以下命令将  `. DS_Store`  加入全局的  `.gitignore`  文件

```
echo .DS_Store >> ~/.gitignore_global
```

- Step2. 将这个全局的 .gitignore 文件加入Git的全局config文件中，在这以后他就不会再出现在Git仓库中了

```
git config --global core.excludesfile ~/.gitignore_global
```

- 在终端中输入以下命令可以阻止 `.DS_Store` 文件自动生成，好坏自辩

```
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool TRUE
```

- 在终端中找到 `.DS_Store` 文件的目录，输入以下命令即可删除（因为本地是隐藏该文件的）

```
find ./ -name ".DS_Store" -depth -exec rm {} \;
```

----



### 当你的仓库是fork来的时 (upstream)

- fork的仓库，用以下方法保持同步更新

```
git remote -v                                    查看当前仓库的fetch push对象
git remote add upstream 原仓库地址                关联原仓库至upstream
git fetch upstream 
git pull upstream 原仓库分支名 即可更新
```

-----------------------------------------------------------

----



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

----



### 比较两个分支区别

- 个人经验：仅仅使用常用的命令去比较大分支的区别过于不直观，比如两个完全独立开发的分支，最后想要合并却发现一大堆冲突时，则可以提前先看一下两个分支的区别，做一些删改操作。按实际需求来，下面推荐一个app：` beyond compare` 

```
Mac破解版下载地址：http://macwk.com/soft/beyond-compare                // 记得关一下ad blocker
```

- 使用方法：安装后主页面有**左右**两个导入文件的地方，因此需要先将待对比的两个分支，分别clone到两个文件夹中，并checkout到对应的分支（也就是本地要有两个相同的git repo但分支不同），然后再导入，导入后右键选择对比内容差异即可直观看到用颜色和符号清晰标记的左右两个分支有哪些文件、文档和代码的具体区别了。
