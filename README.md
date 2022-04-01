# GitSucker
Let's learn some Git! ;)


### git基础操作
##### 当你刚开始使用git时
git init                                生成.git文件，创建一个git repo
git clone https://xxxxxxxxx              克隆远端仓库到本地
git remote -v                                    查看远端仓库地址
git pull                                               拉取远端分支
git add .                                            添加本地修改至本地仓库
git restore —staged 文件名            取消add
git commit -m “xxxxxxx”                commit操作，备注必写
git commit --amend                         修改commit文本
i                                                          进入编辑模式
esc + :wq                                           退出并保存
git push                                             推送到当前分支
git push origin <远程分支名>          推送到指定远程分支
git push origin HEAD:远程分支名    推送到指定远程分支
git remote update origin --prune    更新远端仓库


##### 本地分支 & 远程分支 (branch)
创建仓库：在你的名下new一个repo后clone到本地，然后⬇️
git branch                                         查看本地分支
git branch -a                                    查看远程分支
git branch -d 本地分支名                删除出当前本地分支外的一个本地分支，-D强制删除
git push origin —delete 远程分支名      删除一个指定的远程分支（慎用）
git checkout 远程分支名                  切换到当前远程分支并创建同名本地分支
git checkout 本地分支名                  切换到该本地分支
git checkout -b <本地分支名> origin/远程分支名  新建分支，切换到分支并与远程分支关联
重新创建本地新分支并创建对应的远程分支，关联起来
git branch —set-upstream-to=origin/远程分支
git branch --set-upstream-to local_branch_name origin/remote_branch_name 重新关联新的远程分支
git checkout -b <本地分支名> origin/远程分支名  新建分支，切换到分支并与远程分支关联（同名），用来规范提交

Tip: 本地创建远程分支操作复杂且易混乱，建议直接在仓库创建


##### 当你修改了修改代码 (modified)
git status 查看本地文件修改列表
git diff <文件名> 查看该文件修改细节，vscode可以直接在左边source control处看到
git checkout .                                    舍弃全部本地修改（慎用）
git stash save “备注”                       暂存当前本地分支修改，用来切换分支前使用
git stash pop                                     将先前save的修改恢复
git stash list                             查看当前stash了哪些


##### 当你推拉代码时需要查看配置 (config)
git config —list                                  查看当前git配置表
git config user.name <用户名>        更改用户名和邮箱
git config user.email <用户邮箱>
git commit —amend —reset-author 重置当前commit的user/email
git config --global credential.helper store 完了pull一次输入账号密码以后就不用啦，前提是设置好了ssh key
公司erp且mac电脑，修改记得先更新电脑开机密码，再改本地存储密码，再用自带terminal pull一遍，最后vscode才能用


##### 当你的仓库是fork来的时 (upstream)
git remote -v 查看当前仓库的fetch push对象
git remote add upstream 原仓库地址   关联原仓库至upstream
git fetch upstream 
git pull upstream 原仓库分支名 即可更新

-----------------------------------------------------------

### git高级操作
##### 当你想把一个分支的某个commit提交到另一个分支 (cherry pick)
0.git log                                              拿一下要重新push的commit SHA
1.git checkout 本地远程分支名          切换到要提交的那个分支上
2.git pull                                             更新最新内容
3.git cherry-pick 上面拿到的SHA    把该次commit转移过来
4.git log                                             查一下是不是加进去了
5.git push


##### revert & reset
先了解HEAD概念：HEAD为指针，指向最新一次commit，可以变更，配合下面操作实现回撤commit
git revert
git revert HEAD^                               撤销前一次commit
git revert HEAD^^                             撤销前前一次commit
git revert commit SHA                      git log里拿到的SHA或者reflog拿到的短的或者远端提前后面那串短的

Tip: git revert是提交一个新版本，这个新版本把要revert的内容反向修改回去，俗称中和，会改变源码，但不影响之后提交的内容


git reset
git reset HEAD^                                指针指向上一次commit
git reset HEAD^^                              指针指向上上一次commit
git reset SHA                                     指向某一次commit

Tip: 将HEAD指向之前的commit，或指定的commit，并回退到那个版本，其之后的commit也被回退。慎用，可能会把别人的commit也给退回去，记录参考log。
—soft 会将修改返出来
—hard 直接丢弃修改


##### rebase & merge