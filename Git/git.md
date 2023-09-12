# Git Learning

[git教程链接](https://github.com/geeeeeeeeek/git-recipes/wiki/1.1-%E6%9E%9C%E5%A3%B3%E4%B8%AD%E7%9A%84-Git)

[github英文教程](https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud)



## 第1篇 果壳中的 Git

Git 是目前世界上被最广泛使用的现代软件版本管理系统。Git 本身亦是一个成熟并处于活跃开发状态的开源项目，它最初是由 Linux 操作系统内核的创造者 Linus Torvalds 在 2005 年创造。今天惊人数量的软件项目依赖 Git 进行版本管理，这些项目包括开源以及各种商业软件。Git 在职业软件开发者中拥有良好的声誉，Git 目前支持绝大多数的操作系统以及 IDE（Integrated Development Environments）。

Git 使用分散式架构，是分散式版本管理 DVCS（Distributed Version Control System）的代表。相较于例如 CVS 或者 Subversion 等集中式版本管理软件，Git 并不是将代码的所有修改历史保存在中心服务器中。在 Git 中取而代之的是，所有参与项目的开发者都拥有各自的代码完全拷贝，并在自己的拷贝上进行软件开发。

除了分散式的特点之外，Git 的设计也针对性能，安全性和柔软性作了特别优化。

### 性能

Git 的底层性能相较于其他版本管理软件有强大的优势。在 Git 中所有的操作包括提交修改，创建分支，融合分支，以及求取差分都经过了性能优化。这些优化来自于 Git 的开发者对实际一般代码开发模式的深度认识和广泛知识。

不同于某些版本管理软件，Git 在决定代码修改历史以及保存形式的时候不会被文件名的变化所愚弄，Git 关注的是文件的内容本身。在实际操作中，代码文件经历频繁的再命名，分解和合并。Git 使用一种混合了差分编码（delta encoding，仅保存代码修改的差分），压缩，直接保存，以及版本元数据（version metadata objects）的管理方式。

分散式的架构也给 Git 带来了极大的性能优势。

比如说，现有一名开发成员 Alice 对代码进行了一些改动，添加了一些在2.0版本中准备公开的功能，然后将这些修改以及一份简单的说明进行了提交。随后她又增加了一些另外的新功能，并又作了一次新提交。显然这两次修改在版本历史中被分开各自进行了保存。在这之后 Alice 把代码切换到了 1.3 版本，修复了一些旧版本中的 Bug（这和她新添加的功能没有关系）。这次修复的目的是为了让团队可以在公开 2.0 版本之前，释放一个 1.3.1 版本来解决 1.3 版本中的 Bug 问题。Alice 马上又可以回到她之前进行的 2.0 版本功能开发之中（通过切换代码分支），这些操作由于 Git 的分散属性，都不需要通过网络来连接到中央服务器进行，她甚至可以在飞机中完成这一切。当她完成所有工作，只需要向远程的代码库推送（Push）自己的修改即可。

### 安全性

Git 将保持所管理代码的整合性作为首要要务。所有的文件内容，文件相互关系，以及文件目录结构，版本，标签以及修改，都经过加密哈希校验算法（SHA1）的保护。这可以防止各种意外的代码修改失误，或者是第三者的恶意修改，使得代码修改历史完全可追迹。

使用 Git 你可以确信你拥有代码的完整修改历史。

某些其他的版本管理软件对发布后的代码不进行任何保护。这对于完全依赖于软件开发的团队来说可以是一种非常严重的安全脆弱性问题。

### 柔软性

Git 的关键设计目标之一就是保持柔软性。Git 在以下方面都展现出了其柔软性：支持各种非线性的开发工作流程，对或大或小的软件项目都可以良好支持，以及兼容各种操作系统和协议。

Git 支持将分支和标签作为一级基本对象（不同于 SVN），所以所有对分支和标签的操作也都会被保存到修改历史中。并不是所有的版本管理软件支持这一层面的追迹。

### 使用 Git 进行版本管理

Git 对今天绝大多数软件开发团队来说都是最佳的选择。虽然每个团队都有各自的特点和目标，但是这里我们依然可以列举一些对他们来说Git优于其他选择的理由：

#### Git 很棒

Git 兼备了功能性，高性能，安全性和柔软性，这些是很多软件开发团队以及个人所需要的要素。我们已经在上面详细讨论了这些特性。对很多软件开发团队来说，以以上标准货比三家的最终结果都是选择Git。

#### Git 已经成为了默认的行业标准

Git 是受到最广泛使用和支持的版本管理软件。这使得 Git 在以下这些方面具有极大的吸引力。我们在 Atlassian（此 Tutorial 作者所处的公司）的大多数代码都是用 Git 来进行管理的。

绝大多数的软件开发者都有过 Git 的使用经历，很大一部分在校或者刚刚毕业的学生甚至只用过 Git 进行版本管理。虽然在一些公司开发成员可能在从其他版本管理软件迁移到 Git 的过程中要经历比较陡峭的学习曲线，但是大多数开发者以及他们未来的潜在开发者（学生）都已经具备了使用 Git 的基本技能，这就意味着他们不再需要额外的培训。

Git 的普及还带来很多其他的好处，Git 的市场占有率意味着很多第三方的服务和 IDE 都开始默认支持 Git。比如我们的 DVCS 客户端 [Source Tree](https://www.atlassian.com/software/sourcetree)，项目开发管理软件 [JIRA](https://www.atlassian.com/software/jira)，以及代码托管服务 [Bitbucket](https://www.atlassian.com/software/bitbucket)。

如果你是一个开发新手并期待在未来构建自己的专业开发技能，Git 毫无疑问是你在版本管理上的第一选择。

#### Git 是一个高质量的开源项目

Git 本身是一个拥有良好支持和管理的开源软件项目。Git 的开发者在过去的十年中展现了良好的公平性，成熟的开发手段以保障其用户将来的需求，以及定期的更新以保持其可用性和功能性。开源的特性使得项目代码本身受到无微不至的检查，现在有无数的企业都依赖于 Git 的超高软件开发质量。

Git 同时享有极好的社区支持和庞大的用户群体。你可以找到各种内容深入浅出的学习资料，包括书籍，教程，以及专题网站。甚至还有广播以及视频教程存在。

保持开源降低了编程爱好者的投入成本因为他们不需要花一分钱来使用 Git。在开源项目中，Git 无疑扮演了前一世代版本管理软件，比如 SVN 和 CVS，的成功接班人。

#### 对于 Git 的批评意见

对于 Git 的一个常见批评是它非常难以掌握。Git 中的某些术语对刚上手的朋友或者是使用其他系统的朋友可能会比较陌生，比如说，`revert` 这个命令在 Git 中和在 SVN 或者 CVS 中具有不同的含义。不过尽管如此，Git 依然向用户提供了非常强大的功能。学习掌握这些功能也许会花一些时间，但是一旦你学会了这些技能，它们会帮助你大大提高团队的开发效率。

对于曾经使用非分散式版本管理的团队来说，保存代码的中央服务器可能是他们所不想舍去的。不过，虽然 Git 的确是分散式的架构设计，但是你依然可以设立一个「官方」的代码库来强制保存所有的修改。使用 Git 时，由于所有的开发者都拥有完整的代码库拷贝，所以他们的工作不会被中央服务器的性能甚至有无左右。即便他们下线或者在外，他们依然可以随时查看代码库的修改历史。得益于 Git 的分散式特性，你可以保持自己原有的工作方式但得到 Git 带来的额外好处，有时候你甚至会发现自己不曾意识到有些事居然还可以这样干。

现在你明白了什么是版本管理，什么是 Git 以及为什么我们要选择使用 Git ，接下来你可以选择阅读下一篇文章，在那里我们将解释 Git 给团队和业务所带来的好处。





## 第2篇 从零搭建本地代码仓库

本篇完全面向入门者。我假设你从零开始创建一个项目并且想用 Git 来进行版本控制，我们会讨论如何在你的个人项目中使用 Git，比如如何初始化你的项目，如何管理新的或者已有的文件，如何在远端仓库中储存你的代码。

### 2.1 git快速入门

#### 安装 Git

- Mac 用户：Xcode Command Line Tools 自带 Git (`xcode-select --install`)

- Linux 用户：`sudo apt-get install git`

- Windows 用户：下载

  

#### 利用SSH来连接到GitHub

通过添加SSH-Key的方式，可以在将代码推送到github上实现免登录

生成SSH（Secure Shell）密钥对是连接到GitHub等代码托管平台的常见步骤。以下是在Linux、macOS或Windows上生成SSH密钥对的通用步骤：

1. 打开终端或命令提示符：

   - 在Linux和macOS上，打开终端应用程序。
   - 在Windows上，打开命令提示符或PowerShell。

2. 输入以下命令来生成SSH密钥对：

   ```sh
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```

   将 `"your_email@example.com"` 替换为您在GitHub上注册账号时使用的邮箱地址。您可以选择使用不同的邮箱地址，但建议使用与GitHub账号相关联的邮箱。

3. 系统会提示您选择保存密钥对的位置和文件名。默认情况下，密钥对将保存在用户主目录的`.ssh`文件夹中。您可以选择使用默认路径，也可以自定义路径。按Enter键接受默认路径或输入自定义路径。

4. 接下来，系统会要求您输入一个用于保护私钥文件的密码（称为密码短语）。这是为了增加安全性，确保未经授权的人无法访问您的私钥。输入密码短语并确认，或者按Enter键跳过不设置密码短语。

5. SSH密钥对生成完成后，您会在您选择的位置（默认为`~/.ssh`）找到两个文件：

   - `id_rsa`: 这是私钥文件，需要妥善保管，不要共享或泄露给他人。
   - `id_rsa.pub`: 这是公钥文件，您可以将其分享给GitHub或其他需要它的服务。

6. 打开`id_rsa.pub`文件，并将其中的内容复制到您的GitHub账号设置中。登录GitHub，然后转到“Settings”（设置） -> “SSH and GPG keys”（SSH和GPG密钥）页面，点击“New SSH key”（新的SSH密钥），将复制的公钥内容粘贴到提供的文本框中，并为该SSH密钥提供一个标题。最后，点击“Add SSH key”（添加SSH密钥）。

现在，您的SSH密钥已与GitHub关联，您可以使用SSH协议来与GitHub进行安全的代码传输和访问操作。





#### **检出仓库**

执行如下命令以创建一个本地仓库的克隆版本： `git clone /path/to/repository`

如果是远端服务器上的仓库，你的命令会是这个样子： `git clone username@host:/path/to/repository` （通过 SSH） 或者： `git clone https:/path/to/repository.git` （通过 https）

比如说 `git clone https://github.com/geeeeeeeeek/git-recipes.git` 可以将 git 教程 clone 到你指定的目录。



#### **创建新仓库**

创建新文件夹，打开，然后执行 `git init` 以创建新的 git 仓库。

> 下面每一步中，你都可以通过 `git status` 来查看你的 git 仓库状态。



#### **工作流**

你的本地仓库由 git 维护的三棵“树”组成。第一个是你的 `工作目录`，它持有实际文件；第二个是 `缓存区（Index）`，它像个缓存区域，临时保存你的改动；最后是 `HEAD`，指向你最近一次提交后的结果。 

> 事实上，第三个阶段是 commit history 的图。HEAD 一般是指向最新一次 commit 的引用。现在暂时不必究其细节。



#### **添加与提交**

你可以计划改动（把它们添加到缓存区），使用如下命令：

```sh
git add < filename >
git add *
```

这是 git 基本工作流程的第一步。使用如下命令以实际提交改动：

```sh
git commit -m "代码提交信息"
```

现在，你的改动已经提交到了 HEAD，但是还没到你的远端仓库。

> 在开发时，良好的习惯是根据工作进度及时 commit，并务必注意附上有意义的 commit message。创建完项目目录后，第一次提交的 commit message 一般为「Initial commit.」。



#### **推送改动**

你的改动现在已经在本地仓库的 HEAD 中了。执行如下命令以将这些改动提交到远端仓库：

```
git push origin master
```

可以把 master 换成你想要推送的任何分支。

如果你还没有克隆现有仓库，并欲将你的仓库连接到某个远程服务器，你可以使用如下命令添加：

```sh
git remote add origin <server>
```

如此你就能够将你的改动推送到所添加的服务器上去了。

> - 这里 origin 是 < server > 的别名，取什么名字都可以，你也可以在 push 时将 < server > 替换为 origin。但为了以后 push 方便，我们第一次一般都会先 remote add。
> - 如果你还没有 git 仓库，可以在 GitHub 等代码托管平台上创建一个空（不要自动生成 README.md）的 repository，然后将代码 push 到远端仓库。

至此，你应该可以顺利地提交你的项目了。在下一节中，我们将涉及更多的命令，来完成更有用的操作。比如从远端的仓库拉取更新并且合并到你的本地，如何通过分支多人协作，如何处理不同分支的冲突等等。





#### git常用命令

设置用户签名

`git config --global user.name 用户名`

`git config --global user.email 邮箱`

查看git配置信息

` cat ~/.gitconfig`



`git remote add origin git@github.com:yourName/repositoryname.git`  连接到远程仓库



`git init` 在指定路径下初始化本地仓库

`git status` 查看git状态

`git add` 文件名添加文件，文件存放到暂存区



将暂存区的文件提交到本地库

`git commit -m "first commit" 文件名`

`git log` git 日志



文件修改之后，更新本地库

`git add 文件名`

`git status` 查看本地git状态

`git commit -m "信息" 文件名` 

`git status` 查看本地库状态



 `git reset --hard 版本号`

（git reflog 可以查看到7位版本号）



`git clone -b 分支名 仓库地址`

> clone 的两种方式
>
> ssh:需要使用到SSH KEY
>
> https:需要登录用户名和密码





### 2.2 创建代码仓库

这一章简要地带你了解一些最重要的 Git 命令。在这节中，我会向你介绍开始一个新的版本控制项目需要的所有工具，后面的几节包含了你每天都会用到的Git操作。

在这节之后，你应该能够创建一个新的 Git 仓库，缓存你的项目以免丢失，以及查看你项目的历史。

#### git init

`git init` 命令创建一个新的 Git 仓库。它用来将已存在但还没有版本控制的项目转换成一个 Git 仓库，或者创建一个空的新仓库。大多数Git命令在未初始化的仓库中都是无法使用的，所以这就是你运行新项目的第一个命令了。

运行 `git init` 命令会在你项目的根目录下创建一个新的 `.git` 目录，其中包含了你项目必需的所有元数据。除了 `.git` 目录之外，已经存在的项目不会被改变（就像 SVN 一样，Git 不强制每个子目录中都有一个 `.git` 目录）。

##### 用法

```sh
git init
```

将当前的目录转换成一个 Git 仓库。它在当前的目录下增加了一个 `.git` 目录，于是就可以开始记录项目版本了。

```sh
git init <directory>
```

在指定目录创建一个空的 Git 仓库。运行这个命令会创建一个名为 `directory`，只包含 `.git` 子目录的空目录。

```sh
git init --bare <directory>
```

初始化一个裸的 Git 仓库，但是忽略工作目录。共享的仓库应该总是用 `--bare` 标记创建（见下面的讨论）。一般来说，用 `—bare` 标记初始化的仓库以 `.git` 结尾。比如，一个叫`my-project`的仓库，它的空版本应该保存在 `my-project.git` 目录下。

##### 讨论

和 SVN 相比，`git init` 命令是一个创建新的版本控制项目非常简单的途径。Git 不需要你创建仓库，导入文件，检查正在修改的拷贝。你只需要 `cd` 到你的项目目录下，运行 `git init`，你就有了一个功能强大的 Git 仓库。

但是，对大多数项目来说，`git init` 只需要在创建中央仓库时执行一次——开发者通常不会使用 `git init` 来创建他们的本地仓库。他们往往使用 `git clone` 来将已存在的仓库拷贝到他们的机器中去。

###### 裸仓库

`-—bare` 标记创建了一个没有工作目录的仓库，这样我们在仓库中更改文件并且提交了。中央仓库应该总是创建成裸仓库，因为向非裸仓库推送分支有可能会覆盖已有的代码变动。将`-—bare`看成是用来将仓库标记为储存设施，而不是一个开发环境。也就是说，对于所有的 Git 工作流，中央仓库是裸仓库，开发者的本地仓库是非裸仓库。



##### 栗子

因为 `git clone` 创建项目的本地拷贝更为方便，`git init` 最常见的使用情景就是用于创建中央仓库：

```sh
ssh <user>@<host>

cd path/above/repo

git init --bare my-project.git
```

首先，你用SSH连入存放中央仓库的服务器。然后，来到任何你想存放项目的地方，最后，使用 `-—bare` 标记来创建一个中央存储仓库。开发者会将 `my-project.git` 克隆到本地的开发环境中。

#### git clone

`git clone` 命令拷贝整个 Git 仓库。这个命令就像 `svn checkout` 一样，除了「工作副本」是一个完备的Git仓库——它包含自己的历史，管理自己的文件，以及环境和原仓库完全隔离。

为了方便起见，`clone` 自动创建了一个名为 `origin` 的远程连接，指向原有仓库。这让和中央仓库之间的交互更加简单。

##### 用法

```sh
git clone <repo>
```

将位于 `<repo>` 的仓库克隆到本地机器。原仓库可以在本地文件系统中，或是通过 HTTP 或 SSH 连接的远程机器。

```sh
git clone <repo> <directory>
```

将位于 `<repo>` 的仓库克隆到本地机器上的 `<directory>` 目录。

##### 讨论

如果项目在远程仓库已经设置完毕，`git clone` 是用户获取开发副本最常见的方式。和 `git init`相似，`clone` 通常也是一次性的操作——只要开发者获得了一份工作副本，所有版本控制操作和协作管理都是在本地仓库中完成的。

###### 仓库间协作

这一点很重要，你要理解 Git 中「工作副本」的概念和 SVN 仓库 check out 下来的「工作副本」是很不一样的。和 SVN 不同的是，Git 不会区分工作副本和中央仓库——它们都是功能完备的 Git 仓库。

这就使得 Git 的协作和 SVN 截然不同。SVN 依赖于中央仓库和工作副本之间的关系，而 Git 协作模型是基于仓库和仓库之间的交互的。相对于 SVN 的提交流程，你可以在 Git 仓库之间 `push` 或 `pull` 提交。

当然，你也完全可以给予某个特定的仓库一些特殊的含义。比如，指定某个 Git 仓库为中央仓库，你就可以用 Git 进行中央化的工作流。重点是，这是通过约定实现的，而不是写死在版本控制系统本身。

##### 栗子

下面这个例子演示用 SSH 用户名 john 连接到 example.com，获取远程服务器上中央仓库的本地副本：

```
git clone ssh://john@example.com/path/to/my-project.git

cd my-project

# 开始工作
```

第一行命令在本地机器的 `my-project` 目录下初始化了一个新的 Git 仓库，并且导入了中央仓库中的文件。接下来，你 `cd` 到项目目录，开始编辑文件、缓存提交、和其它仓库交互。同时注意 `.git` 拓展名克隆时会被去除。它表明了本地副本的非裸状态。



#### git config

`git config` 命令允许你在命令行中配置你的 Git 安装（或是一个独立仓库）。这个命令定义了所有配置，从用户信息到仓库行为等等。一些常见的配置命令如下所列。

##### 用法

```sh
git config user.name <name>
```

定义当前仓库所有提交使用的作者姓名。通常来说，你希望使用 `--global` 标记设置当前用户的配置项。

```sh
git config --global user.name <name>
```

定义当前用户所有提交使用的作者姓名。

```sh
git config --global user.email <email>
```

定义当前用户所有提交使用的作者邮箱。

```sh
git config --global alias.<alias-name> <git-command>
```

为Git命令创建一个快捷方式（别名）。

```sh
git config --system core.editor <editor>
```

定义当前机器所有用户使用命令时用到的文本编辑器，如 `git commit`。`<editor>` 参数用编辑器的启动命令（如 vi）替代。

```sh
git config --global --edit
```

用文本编辑器打开全局配置文件，手动编辑。

```sh
vim ~/.gitconfig
```

编辑gitconfig配置文件

##### 讨论

所有配置项都储存在纯文本文件中，所以 `git config` 命令其实只是一个提供便捷的命令行接口。通常，你只需要在新机器上配置一次 Git 安装，以及，你通常会想要使用 `--global` 标记。

Git 将配置项保存在三个单独的文件中，允许你分别对单个仓库、用户和整个系统设置。

- /.git/config – 特定仓库的设置。
- ~/.gitconfig – 特定用户的设置。这也是 `--global` 标记的设置项存放的位置。
- $(prefix)/etc/gitconfig – 系统层面的设置。

当这些文件中的配置项冲突时，本地仓库设置覆盖用户设置，用户设置覆盖系统设置。如果你打开期中一份文件，你会看到下面这些：

```sh
[user]

name = John Smith

email = john@example.com

[alias]

st = status

co = checkout

br = branch

up = rebase

ci = commit

[core]

editor = vim
```

你可以用 `git config` 手动编辑这些值。

##### 栗子

你在安装 Git 之后想要做的第一件事是告诉它你的名字和邮箱，个性化一些默认设置。一般初始的设置过程看上去是这样的：

```sh
# 告诉Git你是谁

git config --global user.name "John Smith"

git config --global user.email john@example.com

# 选择你喜欢的文本编辑器

git config --global core.editor vim

# 添加一些快捷方式(别名)

git config --global alias.st status

git config --global alias.co checkout

git config --global alias.br branch

git config --global alias.up rebase

git config --global alias.ci commit
```

它会生成上一节中所说的 `~/.gitconfig` 文件。



### 2.3 Git add

#### git add

`git add` 命令将工作目录中的变化添加到缓存区。它告诉 Git 你希望下一个提交中包含这个文件的更新。不过，`git add` 不会实际上并不会改变你的仓库。直到你运行 `git commit` ，更改都没有真正被记录。

使用这些命令的同时，你还需要 `git status` 来查看工作目录和缓存区的状态。

##### 工作原理

`git add` 和 `git commit` 这两个命令组成了最基本的 Git 工作流。它们的作用是将项目的诸多版本记录到仓库历史中。不管使用哪种团队协作模型，每个 Git 用户都必须理解这两个命令。

一个项目的编写离不开这个基本模式：编辑、缓存和提交。首先，你在工作目录下编辑你的文件。当你想要备份当前的项目状态时，你使用 `git add` 缓存更改。当你觉得这个被缓存的副本已经就绪，你使用 `git commit` 将它提交到项目历史。`git reset` 命令用于撤销提交或被缓存的快照。

除了 `git add` 和 `git commit` 之外，`git push` 也是完整的 Git 协作流程中的重要一环。`git push` 用于将提交的更改发送到远端仓库。随后，其它团队成员也可以看到这些更改。

![Git Tutorial: git add Snapshot](https://camo.githubusercontent.com/df510811108dc41a3b21f46d87400d87c9534c0ec4dacb3df9d90fa25818d974/68747470733a2f2f7761632d63646e2e61746c61737369616e2e636f6d2f64616d2f6a63723a30663237653030342d663266352d343839302d393231642d3635666137376261323737342f30312e737667)

`git add` 命令和 `svn add` 不同。后者直接将**文件**添加到仓库中，而 `git add` 作用于更抽象的**更改**（更低一层）。也就是说，哪怕对于同一个文件，每次修改时你都需要使用 `git add` ，而 `svn add` 只需要使用一次。这听上去多此一举，但却使项目更易于管理。

##### 缓存区

`git add` 命令最主要的作用是将工作目录中的更改添加到 Git 的缓存区。缓存区是 Git 特有的功能之一，如果你之前使用的是 SVN（甚至是更古老的 Mercurial），那你可得花点时间理解了。你可以把它理解成工作目录和项目历史中间的缓冲区。缓存区是 Git 三个树状文件结构之一，另外两个是工作目录和提交历史。

缓存允许你把紧密相关的几份更改合并成一份快照，而不是直接提交所有新的更改。也就是说你可以同时进行多个无关的更改，最后分成几次将相关更改添加到缓存区并提交。对于任何版本控制系统来说，保持提交的原子性非常重要，这有利于追踪 bug 以及用最小的代价撤销更改。

##### 用法

```sh
git add <文件>
```

缓存 `<文件>` 中的更改，准备下次提交。

```sh
git add <目录>
```

缓存 `<目录>` 下的所有更改，准备下次提交。

```sh
git add -p
```

开始交互式的缓存。你可以将某个文件的其中一处更改加入到下次提交缓存。Git 会显示一系列更改，并等待用户命令。使用 `y` 缓存某一处更改，使用 `n` 忽略某一处更改，使用 `s` 将某一处分割成更小的几份，使用 `e` 手动编辑某一处更改，使用 `q` 退出编辑。

##### 栗子

对于新项目来说，`git add` 和 `svn import` 的作用类似。使用下面两个命令为当前目录创建首个提交：

```sh
git add .
git commit
```

项目设置好之后，新的文件可以通过路径传递给 `git add` 来添加到缓存区：

```sh
git add hello.py
git commit
```

上面的命令同样可以用于记录已有文件的更改。如前所述，Git 不会区分被缓存的更改来自新文件，还是仓库中已有的文件。



### 2.4 检查仓库状态

#### git status

`git status` 命令显示工作目录和缓存区的状态。你可以看到哪些更改被缓存了，哪些还没有，以及哪些还未被 Git 追踪。status 的输出 不会告诉你任何已提交到项目历史的信息。如果你想看的话，应该使用 `git log` 命令。

##### 用法

```sh
git status
```

列出已缓存、未缓存、未追踪的文件。

##### 讨论

`git status` 是一个相对简单的命令。 它告诉你 `git add` 和 `git commit` 的进展。status 信息还包括了添加缓存和移除缓存的相关指令。样例输出显示了三类主要的 `git status` 输出：

```
# On branch master
# Changes to be committed:
# (use "git reset HEAD <file>..." to unstage)
#
#modified: hello.py
#
# Changes not staged for commit:
# (use "git add <file>..." to update what will be committed)
# (use "git checkout -- <file>..." to discard changes in working directory)
#
#modified: main.py
#
# Untracked files:
# (use "git add <file>..." to include in what will be committed)
#
#hello.pyc
```

###### 忽略文件

未追踪的文件通常有两类。它们要么是项目新增但还未提交的文件，要么是像 `.pyc`、`.obj`、`.exe` 等编译后的二进制文件。显然前者应该出现在 `git status` 的输出中，而后者会让我们困惑究竟发生了什么。

因此，Git 允许你完全忽略这些文件，只需要将路径放在一个特定的 `.gitignore` 文件中。所有想要忽略的文件应该分别写在单独一行，`*` 字符用作通配符。比如，将下面这行加入项目根目录的`.gitignore`文件可以避免编译后的Python模块出现在`git status`中：

```
*.pyc
```

##### 栗子

在提交更改前检查仓库状态是一个良好的实践，这样你就不会不小心提交什么奇怪的东西。这个例子显示了缓存和提交快照前后的仓库状态：

```sh
# Edit hello.py
git status
# hello.py is listed under "Changes not staged for commit"
git add hello.py
git status
# hello.py is listed under "Changes to be committed"
git commit
git status
# nothing to commit (working directory clean)
```

第一个 status 的输出显示文件还未缓存。`git add` 操作会影响第二个 `git status`，最后的 status 输出告诉你已经没有可以提交的东西了——工作目录和最近的提交一致。一些 Git 命令（比如 `git merge`）需要工作目录整洁，以免意外覆盖更改。

#### git log

`git log` 命令显示已提交的快照。你可以列出项目历史，筛选，以及搜索特定更改。`git status` 允许你查看工作目录和缓存区，而 `git log` 只作用于提交的项目历史。



log 输出可以有很多种自定义的方式，从简单地筛选提交，到用完全自定义的格式显示。其中一些最常用的 `git log` 配置如下所示。

##### 用法

```sh
git log
```

使用默认格式显示完整地项目历史。如果输出超过一屏，你可以用 `空格键` 来滚动，按 `q` 退出。

```sh
git log -n <limit>
```

用 `<limit>` 限制提交的数量。比如 `git log -n 3` 只会显示 3 个提交。

```sh
git log --oneline
```

将每个提交压缩到一行。当你需要查看项目历史的上层情况时这会很有用。

```sh
git log --stat
```

除了 `git log` 信息之外，包含哪些文件被更改了，以及每个文件相对的增删行数。

```sh
git log -p
```

显示代表每个提交的一堆信息。显示每个提交全部的差异（diff），这也是项目历史中最详细的视图。

```sh
git log --author="<pattern>"
```

搜索特定作者的提交。`<pattern>` 可以是字符串或正则表达式。

```sh
git log --grep="<pattern>"
```

搜索提交信息匹配特定 `<pattern>` 的提交。`<pattern>` 可以是字符串或正则表达式。

```sh
git log <since>..<until>
```

只显示发生在 `<since>` 和 `<until>` 之间的提交。两个参数可以是提交 ID、分支名、`HEAD` 或是任何一种引用。

```sh
git log <file>
```

只显示包含特定文件的提交。查找特定文件的历史这样做会很方便。

```sh
git log --graph --decorate --oneline
```

还有一些有用的选项。`--graph` 标记会绘制一幅字符组成的图形，左边是提交，右边是提交信息。`--decorate` 标记会加上提交所在的分支名称和标签。`--oneline` 标记将提交信息显示在同一行，一目了然。

##### 讨论

`git log` 命令是 Git 查看项目历史的基本工具。当你要寻找项目特定的一个版本或者弄明白合并功能分支时引入了哪些变化，你就会用到这个命令。

```
commit 3157ee3718e180a9476bf2e5cab8e3f1e78a73b7
Author: John Smith
```

大多数时候都很简单直接。但是，第一行需要解释下。`commit` 后面 40 个字的字符串是提交内容的 SHA-1 校验总和（checksum）。它有两个作用。一是保证提交的正确性——如果它被损坏了，提交会生成一个不同的校验总和。第二，它是提交唯一的标识 ID。

这个 ID 可以用于 `git log` 这样的命令中来引用具体的提交。比如，`git log 3157e..5ab91` 会显示所有ID在 `3157e` 和 `5ab91` 之间的提交。除了校验总和之外，分支名、HEAD 关键字也是常用的引用提交的方法。`HEAD` 总是指向当前的提交，无论是分支还是特定提交也好。

~字符用于表示提交的父节点的相对引用。比如，`3157e~1` 指向 `3157e` 前一个提交,`HEAD~3` 是当前提交的回溯3个节点的提交。

所有这些标识方法的背后都是为了让你对特定提交进行操作。`git log` 命令一般是这些交互的起点，因为它让你找到你想要的提交。

##### 栗子

*用法* 一节提供了 `git log` 很多的栗子，但请记住，你可以将很多选项用在同一个命令中：

```sh
git log --author="John Smith" -p hello.py
```

这个命令会显示 `John Smith` 作者对 `hello.py` 文件所做的所有更改的差异比较（diff）。

..句法是比较分支很有用的工具。下面的栗子显示了在 `some-feature` 分支而不在 `master` 分支的所有提交的概览。

```sh
git log --oneline master..some-feature
```





### 2.5 检出之前的提交

**分支的操作**

| 命令名称                | 作用                       |
| ----------------------- | -------------------------- |
| `git branch 分支名`     | 创建分支                   |
| `git branch -v`         | 查看分支                   |
| `git checkout 分支名`   | 切换分支                   |
| `git merge 分支名`      | 把指定分支合并到当前分支上 |
| `git branch -d 分支名 ` | 删除指定分支               |

#### git checkout

`git checkout` 这个命令有三个不同的作用：检出文件、检出提交和检出分支。在这一章中，我们只关心前两种用法。

检出提交会使工作目录和这个提交完全匹配。你可以用它来查看项目之前的状态，而不改变当前的状态。检出文件使你能够查看某个特定文件的旧版本，而工作目录中剩下的文件不变。

##### 用法

```sh
git checkout master
git checkout -b 新分支名 FETCH_HEAD
```

回到 master 分支。分支会在下一节中讲到，而现在，你只需要将它视为回到项目「当前」状态的一种方式。

```sh
git checkout <commit> <file>
```

查看文件之前的版本。它将工作目录中的 `<file>` 文件变成 `<commit>` 中那个文件的拷贝，并将它加入缓存区。

```sh
git checkout <commit>
```

更新工作目录中的所有文件，使得和某个特定提交中的文件一致。你可以将提交的哈希字串，或是标签作为 `<commit>` 参数。这会使你处在分离 HEAD 的状态。

##### 讨论

版本控制系统背后的思想就是「安全」地储存项目的拷贝，这样你永远不用担心什么时候不可复原地破坏了你的代码库。当你建立了项目历史之后，`git checkout` 是一种便捷的方式，来将保存的快照「加载」到你的开发机器上去。

检出之前的提交是一个只读操作。在查看旧版本的时候绝不会损坏你的仓库。你项目「当前」的状态在 `master` 上不会变化。在开发的正常阶段，`HEAD` 一般指向 master 或是其他的本地分支，但当你检出之前提交的时候，`HEAD` 就不再指向一个分支了——它直接指向一个提交。这被称为「分离 `HEAD`」状态 ，可以用下图可视化：



 [head](https://kodekloud.com/blog/git-detached-head/#)

[git头指针分离](https://blog.csdn.net/start_mao/article/details/94722393)

![img](https://kodekloud.com/blog/content/images/2023/06/data-src-image-ce09e4a5-6e67-49a8-9246-fce32b8794f8.png)

正常的head指向的是分支

![img](https://kodekloud.com/blog/content/images/2023/06/data-src-image-029c3ea3-bf35-492d-b695-de3d28d90dae.png)

hēad指向的是某次提交，出现了head 



在另一方面，检出旧文件不影响你仓库的当前状态。你可以在新的快照中像其他文件一样重新提交旧版本。所以，在效果上，`git checkout` 的这个用法可以用来将单个文件回滚到旧版本 。



##### 栗子

###### 查看之前的版本

这个栗子假定你开始了一个疯狂的实验，但你不确定你是否想要保留它。为了帮助你决定，你想看一看你开始实验之前的项目状态。首先，你需要找到你想要看的那个版本的 ID。

```sh
git log --oneline
```

假设你的项目历史看上去和下面一样：

```sh
b7119f2 继续做些丧心病狂的事
872fa7e 做些丧心病狂的事
a1e8fb5 对 hello.py 做了一些修改
435b61d 创建 hello.py
9773e52 初始导入
```

你可以这样使用 `git checkout` 来查看「对 hello.py 做了一些修改」这个提交：

```sh
git checkout a1e8fb5
```

这让你的工作目录和 `a1e8fb5` 提交所处的状态完全一致。你可以查看文件，编译项目，运行测试，甚至编辑文件而不需要考虑是否会影响项目的当前状态。你所做的一切 *都不会* 被保存到仓库中。为了继续开发，你需要回到你项目的「当前」状态：

```
git checkout master
```

这里假定了你默认在 master 分支上开发，我们会在以后的分支模型中详细讨论。

一旦你回到 master 分支之后，你可以使用 `git revert` 或 `git reset` 来回滚任何不想要的更改。

##### 检出文件

如果你只对某个文件感兴趣，你也可以用 `git checkout` 来获取它的一个旧版本。比如说，如果你只想从之前的提交中查看 `hello.py` 文件，你可以使用下面的命令：

```sh
git checkout a1e8fb5 hello.py
```

记住，和检出提交不同，这里 *确实* 会影响你项目的当前状态。旧的文件版本会显示为「需要提交的更改」，允许你回滚到文件之前的版本。如果你不想保留旧的版本，你可以用下面的命令检出到最近的版本：

```sh
git checkout HEAD hello.py
```





### 2.6 回滚之前的错误修改

#### git checkout

见上一章[「2.5 检出之前的提交」](https://github.com/geeeeeeeeek/git-recipes/wiki/2.5-检出之前的提交)。

#### git revert

`git revert` 命令用来撤销一个已经提交的快照。但是，它是通过搞清楚如何撤销这个提交引入的更改，然后在最后加上一个撤销了更改的 新 提交，而不是从项目历史中移除这个提交。这避免了Git丢失项目历史，这一点对于你的版本历史和协作的可靠性来说是很重要的。



##### 用法

```sh
git revert <commit>
```

生成一个撤消了 `<commit>` 引入的修改的新提交，然后应用到当前分支。

##### 讨论

撤销（revert）应该用在你想要在项目历史中移除一整个提交的时候。比如说，你在追踪一个 bug，然后你发现它是由一个提交造成的，这时候撤销就很有用。与其说自己去修复它，然后提交一个新的快照，不如用 `git revert`，它帮你做了所有的事情。

###### 撤销（revert）和重设（reset）对比

理解这一点很重要。`git revert` 回滚了「单独一个提交」，它没有移除后面的提交，然后回到项目之前的状态。在 Git 中，后者实际上被称为 `reset`，而不是 `revert`。

撤销和重设相比有两个重要的优点。首先，它不会改变项目历史，对那些已经发布到共享仓库的提交来说这是一个安全的操作。至于为什么改变共享的历史是危险的，请参阅 `git reset` 一节。

其次，`git revert` 可以针对历史中任何一个提交，而 `git reset` 只能从当前提交向前回溯。比如，你想用 `git reset` 重设一个旧的提交，你不得不移除那个提交后的所有提交，再移除那个提交，然后重新提交后面的所有提交。不用说，这并不是一个优雅的回滚方案。

##### 栗子

下面的这个栗子是 `git revert` 一个简单的演示。它提交了一个快照，然后立即撤销这个操作。

```sh
# 编辑一些跟踪的文件

# 提交一份快照
git commit -m "Make some changes that will be undone"

# 撤销刚刚的提交
git revert HEAD
```

这个操作可以用下图可视化：

![Diagram of Revert - Before and After](https://images.datacamp.com/image/upload/v1671196209/Diagram_of_Revert_Before_and_After_4b427cf59b.png)

注意第四个提交在撤销后依然在项目历史中。`git revert` 在后面增加了一个提交来撤销修改，而不是删除它。 因此，第三和第五个提交表示同样的代码，而第四个提交依然在历史中，以备以后我们想要回到这个提交。

#### git reset

如果说 `git revert` 是一个撤销更改安全的方式，你可以将 `git reset` 看做一个 *危险* 的方式。当你用 `git reset` 来重设更改时(提交不再被任何引用或引用日志所引用)，我们无法获得原来的样子——这个撤销是永远的。使用这个工具的时候务必要小心，因为这是少数几个可能会造成工作丢失的命令之一。

![Diagram of Before and After Git Reset](https://images.datacamp.com/image/upload/v1671196209/Diagram_of_Before_and_After_Git_Reset_9af0fcc3e8.png)

和 `git checkout` 一样，`git reset` 有很多种用法。它可以被用来移除提交快照，尽管它通常被用来撤销缓存区和工作目录的修改。不管是哪种情况，它应该只被用于 *本地* 修改——你永远不应该重设和其他开发者共享的快照。

##### 用法

```sh
git reset <file>
```

从缓存区移除特定文件，但不改变工作目录。它会取消这个文件的缓存，而不覆盖任何更改。

```sh
git reset
```

重设缓冲区，匹配最近的一次提交，但工作目录不变。它会取消 *所有* 文件的缓存，而不会覆盖任何修改，给你了一个重设缓存快照的机会。

```sh
git reset --hard
```

重设缓冲区和工作目录，匹配最近的一次提交。除了取消缓存之外，`--hard` 标记告诉 Git 还要重写所有工作目录中的更改。换句话说：它清除了所有未提交的更改，所以在使用前确定你想扔掉你所有本地的开发。

```sh
git reset <commit>
```

将当前分支的末端移到 `<commit>`，将缓存区重设到这个提交，但不改变工作目录。所有 `<commit>` 之后的更改会保留在工作目录中，这允许你用更干净、原子性的快照重新提交项目历史。

```sh
git reset --hard <commit>
```

将当前分支的末端移到 `<commit>`，将缓存区和工作目录都重设到这个提交。它不仅清除了未提交的更改，同时还清除了 `<commit>` 之后的所有提交。

##### 讨论

上面所有的调用都是用来移除仓库中的修改。没有 `--hard` 标记时 `git reset` 通过取消缓存或取消一系列的提交，然后重新构建提交来清理仓库。而加上 `--hard` 标记对于作了大死之后想要重头再来尤其方便。

撤销(revert)被设计为撤销 *公开* 的提交的安全方式，`git reset`被设计为重设 *本地* 更改。因为两个命令的目的不同，它们的实现也不一样：重设完全地移除了一堆更改，而撤销保留了原来的更改，用一个新的提交来实现撤销。



###### 不要重设公共历史

当有 `<commit>` 之后的提交被推送到公共仓库后，你绝不应该使用 `git reset`。发布一个提交之后，你必须假设其他开发者会依赖于它。

移除一个其他团队成员在上面继续开发的提交在协作时会引发严重的问题。当他们试着和你的仓库同步时，他们会发现项目历史的一部分突然消失了。下面的序列展示了如果你尝试重设公共提交时会发生什么。`origin/master` 是你本地 `master` 分支对应的中央仓库中的分支。

一旦你在重设之后又增加了新的提交，Git 会认为你的本地历史已经和 `origin/master` 分叉了，同步你的仓库时的合并提交（merge commit）会使你的同事困惑。

重点是，确保你只对本地的修改使用 `git reset`，而不是公共更改。如果你需要修复一个公共提交，`git revert` 命令正是被设计来做这个的。

##### 栗子

###### 取消文件缓存

`git reset` 命令在准备缓存快照时经常被用到。下面的例子假设你有两个文件，`hello.py` 和 `main.py`它们已经被加入了仓库中。

```sh
# 编辑了hello.py和main.py

# 缓存了目录下所有文件
git add .

# 意识到hello.py和main.py中的修改
# 应该在不同的快照中提交

# 取消main.py缓存
git reset main.py

# 只提交hello.py
git commit -m "Make some changes to hello.py"

# 在另一份快照中提交main.py
git add main.py
git commit -m "Edit main.py"
```

如你所见，`git reset` 帮助你取消和这次提交无关的修改，让提交能够专注于某一特定的范围。

###### 移除本地修改

下面的这个栗子显示了一个更高端的用法。它展示了你作了大死之后应该如何扔掉那几个更新。

```sh
# 创建一个叫`foo.py`的新文件，增加代码

# 提交到项目历史
git add foo.py
git commit -m "Start developing a crazy feature"

# 再次编辑`foo.py`，修改其他文件

# 提交另一份快照
git commit -a -m "Continue my crazy feature"

# 决定废弃这个功能，并删除相关的更改
git reset --hard HEAD~2
```

`git reset HEAD~2` 命令将当前分支向前倒退两个提交，相当于在项目历史中移除刚创建的这两个提交。记住，这种重设只能用在 *非公开* 的提交中。绝不要在将提交推送到共享仓库之后执行上面的操作。



#### git clean

`git clean` 命令将未跟踪的文件从你的工作目录中移除。它只是提供了一条捷径，因为用 `git status` 查看哪些文件还未跟踪然后手动移除它们也很方便。和一般的 `rm` 命令一样，`git clean` 是无法撤消的，所以在删除未跟踪的文件之前想清楚，你是否真的要这么做。

`git clean` 命令经常和 `git reset --hard` 一起使用。记住，reset 只影响被跟踪的文件，所以还需要一个单独的命令来清理未被跟踪的文件。这个两个命令相结合，你就可以将工作目录回到之前特定提交时的状态。

##### 用法

```sh
git clean -n
```

执行一次git clean的『演习』。它会告诉你那些文件在命令执行后会被移除，而不是真的删除它。

```sh
git clean -f
```

移除当前目录下未被跟踪的文件。`-f`（强制）标记是必需的，除非 `clean.requireForce` 配置项被设为了 `false`（默认为 `true`）。它 *不会* 删除 `.gitignore` 中指定的未跟踪的文件。

```sh
git clean -f <path>
```

移除未跟踪的文件，但限制在某个路径下。

```sh
git clean -df
```

移除未跟踪的文件，以及目录。

```sh
git clean -xf
```

移除当前目录下未跟踪的文件，以及 Git 一般忽略的文件。

##### 讨论

如果你在本地仓库中作死之后想要毁尸灭迹，`git reset --hard` 和 `git clean -f` 是你最好的选择。运行这两个命令使工作目录和最近的提交相匹配，让你在干净的状态下继续工作。

`git clean` 命令对于 build 后清理工作目录十分有用。比如，它可以轻易地删除 C 编译器生成的 `.o` 和 `.exe` 二进制文件。这通常是打包发布前需要的一步。`-x` 命令在这种情况下特别方便。

请牢记，和 `git reset` 一样， `git clean` 是仅有的几个可以永久删除提交的命令之一，所以要小心使用。事实上，它太容易丢掉重要的修改了，以至于 Git 厂商 *强制* 你用 `-f` 标志来进行最基本的操作。这可以避免你用一个 `git clean` 就不小心删除了所有东西。

##### 栗子

下面的栗子清除了工作目录中的所有更改，包括新建还没加入缓存的文件。它假设你已经提交了一些快照，准备开始一些新的实验。

```sh
# 编辑了一些文件
# 新增了一些文件
# 『糟糕』

# 将跟踪的文件回滚回去
git reset --hard

# 移除未跟踪的文件
git clean -df
```

在执行了 reset/clean 的流程之后，工作目录和缓存区和最近一次提交看上去一模一样，而 `git status`会认为这是一个干净的工作目录。你可以重新来过了。

注意，不像 `git reset` 的第二个栗子，新的文件没有被加入到仓库中。因此，它们不会受到 `git reset --hard` 的影响，需要 `git clean` 来删除它们。







## 第3篇 远程团队协作和管理

### 3.1 快速指南

### 3.2 保持同步

SVN 使用唯一的中央仓库作为开发者之间沟通的桥梁，在开发者的工作拷贝和中央仓库之间传递变更集合（changeset），协作得以发生。这和Git的协作模型有所不同，Git 给予每个开发者一份自己的仓库拷贝，拥有自己完整的本地历史和分支结构。用户通常共享一系列的提交而不是单个变更集合。Git 允许你在仓库间共享整个分支，而不是从工作副本提交一个差异集合到中央仓库。

下面的命令让你管理仓库之间的连接，将分支「推送」到其他仓库来发布本地历史，或是将分支「拉取」到本地仓库来查看其它开发者的贡献。

#### git remote

`git remote` 命令允许你创建、查看和删除和其它仓库之间的连接。远程连接更像是书签，而不是直接跳转到其他仓库的链接。它用方便记住的别名引用不那么方便记住的 URL，而不是提供其他仓库的实时连接。

例如，下图显示了你的仓库和中央仓库以及另一个开发者仓库之间的远程连接。你可以向 Git 命令传递 origin 和 john 的别名来引用这些仓库，替代完整的 URL。

![Using git remote to connect other repositories](https://wac-cdn.atlassian.com/dam/jcr:df13d351-6189-4f0b-94f0-21d3fcd66038/01.svg?cdnVersion=1137)

##### 用法

```sh
git remote
```

列出你和其他仓库之间的远程连接。

```sh
git remote -v
```

和上个命令相同，但同时显示每个连接的 URL。

```sh
git remote add <name> <url>
```

创建一个新的远程仓库连接。在添加之后，你可以将 `<name>` 作为 `<url>` 便捷的别名在其他 Git 命令中使用。

```sh
git remote rm <name>
```

移除名为的远程仓库的连接。

```sh
git remote rename <old-name> <new-name>
```

将远程连接从 `<old-name>` 重命名为 `<new-name>`。

##### 讨论

Git 被设计为给每个开发者提供完全隔离的开发环境。也就是说信息并不是自动地在仓库之间传递。开发者需要手动将上游提交拉取到本地，或手动将本地提交推送到中央仓库中去。`git remote` 命令正是将 URL 传递给这些「共享」命令的快捷方式。

##### 名为 origin 的远程连接

当你用 `git clone` 克隆仓库时，它自动创建了一个名为 origin 的远程连接，指向被克隆的仓库。当开发者创建中央仓库的本地副本时非常有用，因为它提供了拉取上游更改和发布本地提交的快捷方式。这也是为什么大多数基于 Git 的项目将它们的中央仓库取名为 origin。

##### 仓库的 URL

Git 支持多种方式来引用一个远程仓库。其中两种最简单的方式便是 HTTP 和 SSH 协议。HTTP 是允许匿名、只读访问仓库的简易方式。比如：

```sh
http://host/path/to/repo.git
```

但是，直接将提交推送到一个 HTTP 地址一般是不可行的（你不太可能希望匿名用户也能随意推送）。如果希望对仓库进行读写，你需要使用 SSH 协议：

```sh
ssh://user@host/path/to/repo.git
```

你需要在托管的服务器上有一个有效的 SSH 账户，但不用麻烦了，Git 支持开箱即用的 SSH 认证连接。

##### 栗子

除了 origin 之外，添加你同事的仓库连接通常会带来一些便利。比如，如果你的同事 John 在 `dev.example.com/john.git` 上维护了一个公开的仓库，你可以这样添加连接：

```sh
git remote add john http://dev.example.com/john.git
```

通过这种方式访问每个开发者的仓库，中央仓库之外的协作变得可能。这给维护大项目的小团队带来了极大的便利。

#### git fetch

`git fetch` 命令将提交从远程仓库导入到你的本地仓库。拉取下来的提交储存为远程分支，而不是我们一直使用的普通的本地分支。你因此可以在整合进你的项目副本之前查看更改。

##### 用法

```sh
git fetch <remote>
```

拉取仓库中所有的分支。同时会从另一个仓库中下载所有需要的提交和文件。

```sh
git fetch <remote> <branch>
```

和上一个命令相同，但只拉取指定的分支。

##### 讨论

当你希望查看其他人的工作进展时，你需要 fetch。fetch 下来的内容表示为一个远程分支，因此不会影响你的本地开发。这是一个安全的方式，在整合进你的本地仓库之前，检查那些提交。类似于 svn update，你可以看到中央仓库的历史进展如何，但它不会强制你将这些进展合并入你的仓库。

##### 远程分支

远程分支和本地分支一样，只不过它们代表这些提交来自于其他人的仓库。你可以像查看本地分支一样查看远程分支，但你会处于分离 HEAD 状态（就像查看旧的提交时一样）。你可以把它们视作只读的分支。如果想要查看远程分支，只需要向 `git branch` 命令传入 `-r` 参数。远程分支拥有 remote 的前缀，所以你不会将它们和本地分支混起来。比如，下面的代码片段显示了从 origin 拉取之后，你可能想要查看的分支：

```sh
git branch -r
# origin/master
# origin/develop
# origin/some-feature
```

同样，你可以用寻常的 `git checkout` 和 `git log` 命令来查看这些分支。如果你接受远程分支包含的更改，你可以使用 `git merge` 将它并入本地分支。所以，不像 SVN，同步你的本地仓库和远程仓库事实上是一个分两步的操作：先 fetch，然后 merge。`git pull` 命令是这个过程的快捷方式。

##### 栗子

这个例子回顾了同步本地和远程仓库 `master` 分支的常见工作流：

```sh
git fetch origin
```

它会显示会被下载的分支：

```sh
a1e8fb5..45e66a4 master -> origin/master
a1e8fb5..9e8ab1c develop -> origin/develop
* [new branch] some-feature -> origin/some-feature
```

在下图中，远程分支中的提交显示为方块，而不是圆圈。正如你所见，`git fetch` 让你看到了另一个仓库完整的分支结构。

<img src="https://wac-cdn.atlassian.com/dam/jcr:f8458bba-d80a-457e-98f5-3544679eb16b/01%20Synchronize%20origin%20with%20git%20fetch.svg?cdnVersion=1137" alt="起源及主要分支" style="zoom: 33%;" />

若想查看添加到上游 master 上的提交，你可以运行 `git log`，用 `origin/master` 过滤：

```sh
git log --oneline master..origin/master
```

用下面这些命令接受更改并并入你的本地 `master` 分支：

```sh
git checkout master
git log origin/master
```

我们可以使用 `git merge origin/master`：

```sh
git merge origin/master
```

origin/master 和 master 分支现在指向了同一个提交，你已经和上游的更新保持了同步。



#### git pull

在基于 Git 的协作工作流中，将上游更改合并到你的本地仓库是一个常见的工作。我们已经知道应该使用 `git fetch`，然后是 `git merge`，但是 `git pull` 将这两个命令合二为一。

##### 用法

```sh
git pull <remote>
```

拉取当前分支对应的远程副本中的更改，并立即并入本地副本。效果和 `git fetch` 后接 `git merge origin/.` 一致。

```sh
git pull --rebase <remote>
```

和上一个命令相同，但使用 `git rebase` 合并远程分支和本地分支，而不是使用 `git merge`。

##### 讨论

你可以将 `git pull` 当做 Git 中对应 `svn update` 的命令。这是同步你本地仓库和上游更改的简单方式。下图揭示了 pull 过程中的每一步。

![远程源上的主要与回购上的主要](https://wac-cdn.atlassian.com/dam/jcr:63e58c34-b273-4e48-a6b1-6e3ba4d4a0ea/01%20bubble%20diagram-01.svg?cdnVersion=1137)

![变基](https://wac-cdn.atlassian.com/dam/jcr:d5633068-d448-4140-953e-2ab31553ce10/03%20bubble%20diagram-03-updated@2x%20kopiera.png?cdnVersion=1137)

你认为你的仓库已经同步了，但 `git fetch` 发现 origin 中 `master` 的版本在上次检查后已经有了新进展。 接着 `git merge` 立即将 `remote master` 并入本地的分支。

##### 基于 Rebase 的 Pull

`--rebase` 标记可以用来保证线性的项目历史，防止合并提交（merge commits）的产生。很多开发者倾向于使用 rebase 而不是 merge，因为「我想要把我的更改放在其他人完成的工作之后」。在这种情况下，与普通的 `git pull` 相比而言，使用带有 `--rebase` 标记的 `git pull` 甚至更像 svn update。

事实上，使用 `--rebase` 的 pull 的工作流是如此普遍，以致于你可以直接在配置项中设置它：

```sh
git config --global branch.autosetuprebase always # In git < 1.7.9
git config --global pull.rebase true              # In git >= 1.7.9
```

在运行这个命令之后，所有的 `git pull` 命令将使用 `git rebase` 而不是 `git merge`。

##### 栗子

下面的栗子演示了如何和一个中央仓库的 `master branch` 同步：

```sh
git checkout master
git pull --rebase origin
```

简单地将你本地的更改放到其他人已经提交的更改之后。

[详解git fetch与git pull的区别](https://blog.csdn.net/riddle1981/article/details/74938111)



#### git push

Push 是你将本地仓库中的提交转移到远程仓库中时要做的事。它和 `git fetch` 正好相反，fetch 将提交导入到本地分支，而 push 将提交导出到远程分支。它可以覆盖已有的更改，所以你需要小心使用。这些情况请见下面的讨论。

##### 用法

```sh
git push <remote> <branch>
```

将指定的分支推送到 `<remote>` 上，包括所有需要的提交和提交对象。它会在目标仓库中创建一个本地分支。为了防止你覆盖已有的提交，如果会导致目标仓库非快速向前合并时，Git 不允许你 push。

```
git push <remote> --force
```

和上一个命令相同，但即使会导致非快速向前合并也强制推送。除非你确定你所做的事，否则不要使用 `--force` 标记。

```
git push <remote> --all
```

将所有本地分支推送到指定的远程仓库。

```
git push <remote> --tags
```

当你推送一个分支或是使用 `--all` 选项时，标签不会被自动推送上去。`--tags` 将你所有的本地标签推送到远程仓库中去。

##### 讨论

`git push` 最常见的用法是将你的本地更改发布到中央仓库。在你积累了一些本地提交，准备和同事们共享时，你（可以）用交互式 rebase 来清理你的提交，然后推送到中央仓库去。



上图显示了当你本地的 master 分支进展超过了中央仓库的 `master` 分支，当你运行 `git push origin master` 发布更改时发生的事情。注意，`git push` 和在远程仓库内部运行 `git merge master` 事实上是一样的。

##### 强制推送

Git 为了防止你覆盖中央仓库的历史，会拒绝你会导致非快速向前合并的推送请求。所以，如果远程历史和你本地历史已经分叉，你需要将远程分支 pull 下来，在本地合并后再尝试推送。这和 SVN 让你在提交更改集合之前要和中央仓库同步是类似的。

`--force` 这个标记覆盖了这个行为，让远程仓库的分支符合你的本地分支，删除你上次 pull 之后可能的上游更改。只有当你意识到你刚刚共享的提交不正确，并用 `git commit --amend` 或者交互式 rebase 修复之后，你才需要用到强制推送。但是，你必须绝对确定在你使用 `--force` 标记前你的同事们都没有 pull 这些提交。

##### 只推送到裸仓库

此外，你只应该推送到那些用 `--bare` 标记初始化的仓库。因为推送会弄乱远程分支结构，很重要的一点是，永远不要推送到其他开发者的仓库。但因为裸仓库没有工作目录，不会发生打断别人的开发之类的事情。

##### 栗子

下面的栗子描述了将本地提交推送到中央仓库的一些标准做法。首先，确保你本地的 `master` 和中央仓库的副本是一致的，提前 fetch 中央仓库的副本并在上面 rebase。交互式 rebase 同样是共享之前清理提交的好机会。接下来，`git push` 命令将你本地 `master` 分支上的所有提交发送给中央仓库.

```
git checkout master
git fetch origin master
git rebase -i origin/master
# Squash commits, fix up commit messages etc.
git push origin master
```

因为我们已经确信本地的 `master` 分支是最新的，它应该导致快速向前的合并，`git push` 不应该抛出非快速向前之类的问题。



## 第4篇 Git 命令详解

- **第1章** [图解 Git 命令](https://github.com/geeeeeeeeek/git-recipes/wiki/4.1-图解-Git-命令)

  如果你稍微理解 Git 的工作原理，这篇文章能够让你理解的更透彻。
  
  此页图解 git 中的最常用命令。如果你稍微理解 git 的工作原理，这篇文章能够让你理解的更透彻。
  
  ## 基本用法
  
  ![enter image description here](https://camo.githubusercontent.com/39476425160e695a009ba5a4081faa0edf157ddd752cd87640f8be106d94971c/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f62617369632d75736167652e737667)
  
  上面的四条命令在工作目录、stage 缓存(也叫做索引)和 commit 历史之间复制文件。
  
  - `git add files` 把工作目录中的文件加入 stage 缓存
  - `git commit` 把 stage 缓存生成一次 commit，并加入 commit 历史
  - `git reset -- files` 撤销最后一次 `git add files`，你也可以用 `git reset` 撤销所有 stage 缓存文件
  - `git checkout -- files` 把文件从 stage 缓存复制到工作目录，用来丢弃本地修改
  
  你可以用 `git reset -p`、`git checkout -p` 或 `git add -p` 进入交互模式，也可以跳过 stage 缓存直接从 commit历史取出文件或者直接提交代码。
  
  ![enter image description here](https://camo.githubusercontent.com/c51a47eb0b0a2f63c3a9f16bb30f07151ee4c5b5bb684733346013dd2f30c043/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f62617369632d75736167652d322e737667)
  
  - `git commit -a ` 相当于运行 `git add` 把所有当前目录下的文件加入 stage 缓存再运行 `git commit`。
  - `git commit files` 进行一次包含最后一次提交加上工作目录中文件快照的提交，并且文件被添加到 stage 缓存。
  - `git checkout HEAD -- files` 回滚到复制最后一次提交。
  
  ## 约定
  
  后文中以下面的形式使用图片：
  
  ![enter image description here](https://camo.githubusercontent.com/2837c240d61c5e0f0f17ea69fe2a53c346e2c481d4b591c30e8ade1049e2cf69/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f636f6e76656e74696f6e732e737667)
  
  绿色的5位字符表示提交的 ID，分别指向父节点。分支用橙色显示，分别指向特定的提交。当前分支由附在其上的 `_HEAD_` 标识。
  
  这张图片里显示最后 5 次提交，`_ed489_` 是最新提交。 `_master_` 分支指向此次提交，另一个 `_maint_` 分支指向祖父提交节点。
  
  ## 命令详解
  
  ### Diff
  
  有许多种方法查看两次提交之间的变动，下面是其中一些例子。
  
  ![enter image description here](https://camo.githubusercontent.com/e3195469185664615298cb3f8ec2ef5b8cbc6cd8ef98589b1c0c42e20e032f63/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f646966662e737667)
  
  ### Commit
  
  提交时，Git 用 stage 缓存中的文件创建一个新的提交，并把此时的节点设为父节点。然后把当前分支指向新的提交节点。下图中，当前分支是 `_master_`。
  
  在运行命令之前，`_master_` 指向 `_ed489_`，提交后，`_master_` 指向新的节点`_f0cec_` 并以 `_ed489_` 作为父节点。
  
  ![img](http://marklodato.github.io/visual-git-guide/commit-main.svg)
  
  即便当前分支是某次提交的祖父节点，Git 会同样操作。下图中，在 `_master_` 分支的祖父节点 `_maint_` 分支进行一次提交，生成了 `_1800b_`。
  
  这样，`_maint_ `分支就不再是 `_master_` 分支的祖父节点。此时，[merge](https://github.com/geeeeeeeeek/git-recipes/wiki/4.1-图解-Git-命令#merge) 或者 [rebase](https://github.com/geeeeeeeeek/git-recipes/wiki/4.1-图解-Git-命令#rebase) 是必须的。
  
  ![img](http://marklodato.github.io/visual-git-guide/commit-stable.svg)
  
  如果想更改一次提交，使用 `git commit --amend`。Git 会使用与当前提交相同的父节点进行一次新提交，旧的提交会被取消。
  
  ![enter image description here](https://camo.githubusercontent.com/9db30c0a1bda49613ded6826d3dce1f510faaa7d8c1d60237c963b3e4d86699c/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f636f6d6d69742d616d656e642e737667)
  
  另一个例子是[分离HEAD提交](https://github.com/geeeeeeeeek/git-recipes/wiki/4.1-图解-Git-命令#detached)，在后面的章节中介绍。
  
  ### Checkout
  
  `git checkout` 命令用于从历史提交（或者 stage 缓存）中拷贝文件到工作目录，也可用于切换分支。
  
  当给定某个文件名（或者打开 `-p` 选项，或者文件名和-p选项同时打开）时，Git 会从指定的提交中拷贝文件到 stage 缓存和工作目录。比如，`git checkout HEAD~ foo.c` 会将提交节点 `_HEAD~_`（即当前提交节点的父节点）中的 `foo.c` 复制到工作目录并且加到 stage 缓存中。如果命令中没有指定提交节点，则会从 stage 缓存中拷贝内容。注意当前分支不会发生变化。
  
  ![enter image description here](https://camo.githubusercontent.com/7f1374ff4868f364cf2100fdfa3a2c03a1506bfe7542cafa11e1a9924c095477/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f636865636b6f75742d66696c65732e737667)
  
  当不指定文件名，而是给出一个（本地）分支时，那么 `_HEAD_` 标识会移动到那个分支（也就是说，我们「切换」到那个分支了），然后 stage 缓存和工作目录中的内容会和 `_HEAD_` 对应的提交节点一致。新提交节点（下图中的 `a47c3`）中的所有文件都会被复制（到 stage 缓存和工作目录中）；只存在于老的提交节点（`ed489`）中的文件会被删除；不属于上述两者的文件会被忽略，不受影响。
  
  ![enter image description here](https://camo.githubusercontent.com/6fdaec181d4765be1e03cb7898751048f161ae61461d3d97237cd5b4a3c74956/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f636865636b6f75742d6272616e63682e737667)
  
  如果既没有指定文件名，也没有指定分支名，而是一个标签、远程分支、SHA-1 值或者是像 `_master~3_` 类似的东西，就得到一个匿名分支，称作 `_detached HEAD_`（被分离的 `_HEAD_` 标识）。这样可以很方便地在历史版本之间互相切换。比如说你想要编译 1.6.6.1 版本的 Git，你可以运行 `git checkout v1.6.6.1`（这是一个标签，而非分支名），编译，安装，然后切换回另一个分支，比如说 `git checkout master`。然而，当提交操作涉及到「分离的 HEAD」时，其行为会略有不同，详情见在[下面](https://github.com/geeeeeeeeek/git-recipes/wiki/4.1-图解-Git-命令#detached)。
  
  ![enter image description here](https://camo.githubusercontent.com/f096f3d8b818a414a306555d65bcee5a256a7b188505b24f50d0b2883eee34da/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f636865636b6f75742d64657461636865642e737667)
  
  ### HEAD 标识处于分离状态时的提交操作
  
  当 `_HEAD_` 处于分离状态（不依附于任一分支）时，提交操作可以正常进行，但是不会更新任何已命名的分支。你可以认为这是在更新一个匿名分支。
  
  ![enter image description here](https://camo.githubusercontent.com/a98350c9e6a6b5ddd32695ba8b24baad0a130569473267c72c266f3bf5cc3d04/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f636f6d6d69742d64657461636865642e737667)
  
  一旦此后你切换到别的分支，比如说 `_master_`，那么这个提交节点（可能）再也不会被引用到，然后就会被丢弃掉了。注意这个命令之后就不会有东西引用 `_2eecb_`。
  
  ![enter image description here](https://camo.githubusercontent.com/7cf6472df283c9426a8a8d0d78792e00bef5ccd012e6859efad9d6929d5100a0/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f636865636b6f75742d61667465722d64657461636865642e737667)
  
  但是，如果你想保存这个状态，可以用命令 `git checkout -b name` 来创建一个新的分支。
  
  ![enter image description here](https://camo.githubusercontent.com/376eb29d48982023b806f310bd1651ccba9933fe47122c0d8d8753c1d8da1a3b/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f636865636b6f75742d622d64657461636865642e737667)
  
  ### Reset
  
  `git reset` 命令把当前分支指向另一个位置，并且有选择的变动工作目录和索引。也用来在从历史commit历史中复制文件到索引，而不动工作目录。
  
  如果不给选项，那么当前分支指向到那个提交。如果用 `--hard` 选项，那么工作目录也更新，如果用 `--soft` 选项，那么都不变。
  
  ![enter image description here](https://camo.githubusercontent.com/964f8fbafe8f65c492fb8f8801114ed803571a450e4a96caa6535ef39d9d1129/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f72657365742d636f6d6d69742e737667)
  
  如果没有给出提交点的版本号，那么默认用 `_HEAD_`。这样，分支指向不变，但是索引会回滚到最后一次提交，如果用 `--hard` 选项，工作目录也同样。
  
  ![enter image description here](https://camo.githubusercontent.com/cdf53c2fb57aab7f7a2f35b82d742bee4fd0f650b9931fbdc354d0c4eff6230a/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f72657365742e737667)
  
  如果给了文件名(或者 `-p` 选项), 那么工作效果和带文件名的[checkout](https://github.com/geeeeeeeeek/git-recipes/wiki/4.1-图解-Git-命令#checkout)差不多，除了索引被更新。
  
  ![enter image description here](https://camo.githubusercontent.com/6b30e7e3d906544d6eac864ffbef84d3d4b0e52b4091817ef4fcd28752e73c88/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f72657365742d66696c65732e737667)
  
  ### Merge
  
  `git merge` 命令把不同分支合并起来。合并前，索引必须和当前提交相同。如果另一个分支是当前提交的祖父节点，那么合并命令将什么也不做。
  
  另一种情况是如果当前提交是另一个分支的祖父节点，就导致 `_fast-forward_` 合并。指向只是简单的移动，并生成一个新的提交。
  
  ![enter image description here](https://camo.githubusercontent.com/412fffb55e529007593f64087ff31c5667551f6123cdb9f399bbaacc00b5dc66/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f6d657267652d66662e737667)
  
  否则就是一次真正的合并。默认把当前提交（`_ed489_` 如下所示）和另一个提交（`_33104_`）以及他们的共同祖父节点（`_b325c_`）进行一次[三方合并](http://en.wikipedia.org/wiki/Three-way_merge)。结果是先保存当前目录和索引，然后和父节点 `_33104_` 一起做一次新提交。
  
  ![enter image description here](https://camo.githubusercontent.com/bbabb78c6744d67b4d0d05a57eae071f5573d852682f9fb6e07c715c1ab85c8d/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f6d657267652e737667)
  
  ### Cherry Pick
  
  `git cherry-pick` 命令「复制」一个提交节点并在当前分支做一次完全一样的新提交。
  
  ![enter image description here](https://camo.githubusercontent.com/51aa233a9087e2e706a957caa3338925eae65c357edcc6522b4fed5568621629/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f6368657272792d7069636b2e737667)
  
  ### Rebase
  
  `git rebase` 是合并命令的另一种选择。合并把两个父分支合并进行一次提交，提交历史不是线性的。rebase 在当前分支上重演另一个分支的历史，提交历史是线性的。
  
  本质上，这是线性化的自动的 [cherry-pick](https://github.com/geeeeeeeeek/git-recipes/wiki/4.1-图解-Git-命令#cherry-pick)。
  
  ![enter image description here](https://camo.githubusercontent.com/cfbab89c71e5aeb0bf1537c6624953be13f9de6cd1d8839585f4cfb06a427ccd/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f7265626173652e737667)
  
  上面的命令都在 `_topic_` 分支中进行，而不是 `_master_` 分支，在 `_master_` 分支上重演，并且把分支指向新的节点。注意旧提交没有被引用，将被回收。
  
  要限制回滚范围，使用 `--onto` 选项。下面的命令在 `_master_` 分支上重演当前分支从 `_169a6_` 以来的最近几个提交，即 `_2c33a_`。
  
  ![enter image description here](https://camo.githubusercontent.com/a7590c0e80f874e0b3194c8ae6384fde68f7169bb231613381e82ab7484eb08d/687474703a2f2f6d61726b6c6f6461746f2e6769746875622e696f2f76697375616c2d6769742d67756964652f7265626173652d6f6e746f2e737667)
  
  同样有 `git rebase --interactive` 让你更方便的完成一些复杂操作，比如丢弃、重排、修改、合并提交。
  
  

## 第5篇 Git 实用贴士

- **第1章** [代码合并：Merge、Rebase 的选择](https://github.com/geeeeeeeeek/git-recipes/wiki/5.1-代码合并：Merge、Rebase-的选择)

  `git rebase` 和 `git merge` 都是用来合并分支，只不过方式不太相同。`git rebase` 经常被人认为是一种 Git 巫术，初学者应该避而远之。但如果使用得当，它能省去太多烦恼。在这篇文章中，我们会通过比较找到 Git 工作流中所有可以使用 rebase 的机会。

- **第2章** [代码回滚：Reset、Checkout、Revert 的选择](https://github.com/geeeeeeeeek/git-recipes/wiki/5.2-代码回滚：Reset、Checkout、Revert-的选择)

  git reset、git checkout 和 git revert 都是用来撤销代码仓库中的某些更改，所以我们经常弄混。在这篇文章中，我们比较最常见的用法，分析在什么场景下该用哪个命令。

- **第3章** [Git log 高级用法](https://github.com/geeeeeeeeek/git-recipes/wiki/5.3-Git-log-高级用法)

  任何一个版本控制系统设计的目的都是为了记录你代码的变化——谁贡献了什么，找出 bug 是什么时候引入的，以及撤回一些有问题的更改。`git log` 可以格式化 commit 输出的形式，或过滤输出的 commit 从而找到项目中你需要的任何信息。

- **第4章** [Git 钩子：自定义你的工作流](https://github.com/geeeeeeeeek/git-recipes/wiki/5.4-Git-钩子：自定义你的工作流)

  Git 钩子是在 Git 仓库中特定事件发生时自动运行的脚本。它可以让你自定义 Git 内部的行为，在开始周期中的关键点触发自定义的行为，自动化或者优化你开发工作流中任意部分。

- **第5章** [Git 提交引用和引用日志](https://github.com/geeeeeeeeek/git-recipes/wiki/5.5-Git-提交引用和引用日志)

  提交是 Git 的精髓所在，你无时不刻不在创建和缓存提交、查看以前的提交，或者用各种 Git 命令在仓库间转移你的提交。在这章中，我们研究提交的各种引用方式，以及涉及到的 Git 命令的工作原理。我们还会学到如何使用 Git 的引用日志查看看似已经删除的提交。

🥢🥢🥢🥢🥢🥢

### 版权说明

- ✍️ [童仲毅 (geeeeeeeeek@github)](https://github.com/geeeeeeeeek)
- 除非另行注明，这个项目中的所有内容采用知识共享-署名（[CC BY 2.5 AU](http://creativecommons.org/licenses/by/2.5/au/deed.zh)）协议共享。
- 不少文章在原基础上翻译或演绎而来，页面上方标注了原作者、原文链接以及原文采用的协议。如有版权疑问，请在 Issue 中提出。
- 欢迎通过 Issue 或者 Pull Request 推荐你认为合适的资料，让这份菜单更充实一些。

🥢🥢🥢🥢🥢🥢

### 为什么要做这份菜单

> 在整理 Git 资料的时候，我发现社区贡献了非常多高质量的博客文章、指南等等。尤其英文的那些资料，除了大家熟知的「Git 图解」，还有好多优秀的文章仍无人翻译。此外，这些资料往往只涉及某些特定的话题，如果能有一份菜单将这些菜谱以特定的方式串起来，那么对于 Git 学习者来说将会是极大的便利。尤其对于我这样热爱查阅社区资料胜过出版物的懒人 :] 随着我的学习节奏还会不断有新的菜谱加入进来，或许不会很频繁，不过也没有确定的终点。
>
> 📅 *写于 2015 年*

