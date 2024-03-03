> 现在也有一个客户端可以管理git了

# 基本认知

> git是==分布式==版本控制工具，为什么要强调他是分布式的版本控制工具呢，因为还有一种工具叫做==集中式==版本控制工具

官网: https://git-scm.com/

版本控制：

>版本控制（Revision control）是一种在开发的过程中用于管理我们对文件、目录或工程等内容的修改历史，方便查看更改历史记录，备份以便恢复以前的版本的软件工程技术。简单来说就是用于管理多人协同开发项目的技术。

版本控制工具：
1. 集中式版本控制工具（SVN）
	> 就像是那种文件共享工作一样，协同工作
	> 如果服务器坏啦，那就凉凉喽
2. 分布式版本控制工具
	> 每个人都能够在自己的电脑上进行修改
	> 在本地就能进行版本控制
	> 然后把最新完成的提交到远程库上
	
代码托管平台：
- Github（外网）
- Gitee（国内）
- GitLab（局域网）

# git安装

1. 官网下载安装包
2. 选择安装路径
3. 勾选配置
![[Pasted image 20240223183802.png]]
4.  选择默认编辑器（vim）
5. 是否修改分支名字默认分支名为master
6. 是否修改环境变量，第一个，第二个都可以
7. 换行符
8. 选择终端窗口
9. 选择合并模式
10. 选择凭据管理器选择推荐的
# git常用命令

| 命令名称 | 作用 |  |
| ---- | ---- | ---- |
| `git config --global user.name 用户名` | `设置用户签名` |  |
| `git config --global user.email 邮箱` | `设置用户签名邮箱` |  |
| 以下命令重要 |  |  |
| `git init` | `初始化本地仓库` |  |
| `git status` | `查看本地状态` |  |
| `git add 文件路径` | `添加到暂存区` |  |
| `git commite -m "提交日志"` | `提交到本地库` |  |
| `git reflog` | `查看历史记录` |  |
| `git reset --hard` | `版本穿梭` |  |
|  |  |  |
***用户签名：

> 在windows的家目录中可以看到一个`.gitconfig`的文件，在这里面可以看到自己配置的信息
> 这里配置的用户签名和日后提交远程仓库没有任何关系

***初始化本地仓库***：

- 使用到命令`git init

***查看本地仓库状态：

- 使用到命令`git status`

***添加和删除暂存区：

- 使用到的命令`git add 文件路径` 添加
- 使用到的命令`git rm  --cached 文件路径` 删除

***提交到本地仓库：

- 使用到的命令`git commit -m "提交日志" 文件名（可以加可以不加，不加提交所有）`

>  注意7位版本号

***查看提交日志:

- 使用到命令：`git  log --oneline` 查看提交信息（简化）
- 使用到的命令：`git log` ：将日志所有信息展示出来，含提交信息、时间、作者
- 使用到的命令`git diff`：将本次变更内容展示出来
- `git log --all`：将本次项目下的所有提交内容全部展示出来

- `git log --oneline`：将提交中的变化情况的主要信息展示出来

- `git log --graph`：将提交中的变化情况以线段的形式展示出来

- `git log --reverse`：逆向显示所有日志  

- `git log --author=Linus`：显示作者为Linux的操作日志  

- `git log -n`：显示n条日志  

- `git history`：命令行操作日志

***版本穿梭：

- 使用到命令：`git reset --hard 版本号`

# git分支

> 1. 同时并行推进多个功能开发，提高开发效率
> 2. 各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响，失败了直接删除即可、

## 分支操作命令

| 命令名称 | 作用 |
| ---- | ---- |
| git branch 分支名 | 创建分支 |
| git branch -v | 查看分支 |
| git checkout 分支名 | 切换分支 |
| git merge 分支名 | 把指定的分支合并到当前所在分支 |

## 运用

***查看分支：

- 使用到的命令`git branch -v`

***创建分支：

- 使用到的命令`git branch 分支名`
- `git branch -m oldName newName`本地分支重命名(还没有推送到远程)
![[Pasted image 20240226112138.png]]

***切换分支：

- 使用到的命令：`git checkout 分支名`
- `git switch 分支名`

***合并分支：正常合并

- 使用到的命令：`git merge 分支名` 假设当前在main分支，就会把{{分支名}}合并到main的分支上

***合并分支：冲突合并

> 产生原因：合并分支时，两个分支在同一个文件的同一个位置有两套完全不同的修改.Git无法替我们决定用哪个，需要人为决定

# Git团队协作机制

- 团队内协作

需要将别人添加进入团队
![[Pasted image 20240227120424.png]]
![[Pasted image 20240226131722.png]]

- 跨团队协作

使用Fork
Pull request

# GitHub操作

>官网：www.github.com

***创建远程仓库

第一步：创建仓库（一般远程仓库与本地库的名是一样的）
![[Pasted image 20240226114624.png]]

- `git remote -v`查看远程库别名

- `git remote add 最好库名(别名) 仓库连接`


***代码推送 Push

> 注意：需要凭据管理器

`git push 别名 分支`

***代码拉取 Pull

`git pull 别名 分支`

***代码克隆 Clone

> 注意：不需要登录凭据许可证

`git clone 远程仓库地址`
- 拉取代码
- 初始化本地库
- 创建别名

***SSH免密登录

- 第一步：生成秘钥

`ssh -keygen -rsa -C 邮箱`


# Idea集成Git

***配置忽略文件：

创建文件`xx.ignore` 建议使用`git.ignore` 前缀名其实可以随便起

通过该命令配置忽略文件：
```bash
git config --global core.excludesfile ~/.gitignore
```

第一种网上找的
```java
### IntelliJ IDEA ###
.idea
*.iws
*.iml
*.ipr
modules.xml

target/

**/.idea
**/*.iws
**/*.iml
**/*.ipr
**/modules.xml


**/mvnw
**/mvnw.cmd
**/.mvn
**/target/
**/.gitignore

### Maven ###
pom.xml.tag
pom.xml.releaseBackup
pom.xml.versionsBackup
pom.xml.next
release.properties
dependency-reduced-pom.xml
buildNumber.properties
.mvn/timing.properties
.mvn/wrapper/maven-wrapper.jar

### Java ###
# Compiled class file
*.class
```

第二种网上找的：
```java
*.class

# VS Code IDE Files
.classpath
.settings/
.project
.vscode/
.attach_pid*

# Maven builds
*/target/
target/
**/.factorypath
gen/**

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.ear

# virtual machine crash logs, see https://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*

# Intellij IDEA files
.idea
*.iml

.DS_Store

# generated stuff
kubernetes/bin
kubernetes/swagger.json.unprocessed
```


# Idea集成Github

> 蓝色的文件是自动跟踪，直接提交即可

在IDEA中配置选择git
![[Pasted image 20240226192041.png]]
选择你git安装目录下的bin目录下的git.exe![[Pasted image 20240226192346.png]]

- 菜单栏VCS：创建本地仓库
![[Pasted image 20240226193847.png]]
- 右键提交文件到本地库
![[Pasted image 20240226193405.png]]
- 切换版本
![[Pasted image 20240226195137.png]]
- 创建分支/切换分支
![[Pasted image 20240226195720.png]]
![[Pasted image 20240226195904.png]]
- 合并
![[Pasted image 20240226200602.png]]
合并冲突点merge，进行选择合并
![[Pasted image 20240226201320.png]]

# IDEA远程仓库

> 使用口令登录更好登录

我的githubtoken口令：`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`

- 使用idea向远程库分享文件（创建远程库，然后push）
![[Pasted image 20240226205238.png]]
- 拉取
![[Pasted image 20240226210152.png]]

# Gitee码云
创建远程仓库

Idea集成Gitee

码云连接Github 进行代码复制与迁移

导入已有仓库

# GitLab（局域网）

## GitLab服务器的搭建和部署

## Idea集成GitLab
