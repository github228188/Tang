# Git

## 一、本地库初始化

> 命令 ：**git init**
>
> 注意 ：.git 目录中存放的是本地库相关的子目录和文件，不要删除，也不要胡 乱修改。

## 二、设置签名

> 1. ##### 项目级别、仓库级别签名：仅当前本地库范围内有效
>
>    命令：
>
>    + **git `config` user.name tom_pro**
>
>    + **git `config` user.email goodMorning_pro@atguigu.com**
>    + 信息保存位置：./.git/config 文件 

> 2. ##### 系统用户级别 ：登录当前操作系统的用户范围 
>
>    + **git config `--global` user.name tom_glb**
>    + **git config `--global` goodMornming_pro@atguigu.com**

> 3. ##### 级别优先级 
>
>    `就近原则：项目级别优先于系统用户级别，二者都有时采用项目级别的签名` 
>
> + 如果只有系统用户级别的签名，就以系统用户级别的签名为准 
>
> + 二者都没有不允许

## 三、基本操作

>1. ##### 状态查看
>
>   + 命令 : `git status`
>   + 作用 ：查看工作区、暂存区状态

> 2. ##### 添加
>
>    + 命令 ：`git add [file name]`
>    + 作用 ：将工作区的“新建、修改”添加到暂存区

> 3. ##### 提交
>
>    + 命令 ：`git commit -m "commit message" [file name]`
>    + 作用 ： 将暂存区的内容提交到本地库

> 4. ##### 查看历史记录
>
>    + 命令 ：`git log`
>    + 多屏显示控制方式：
>      + 空格向下翻页
>      + b 向上翻页
>      + q 退出
>    + 命令 ： `git log --pretty=oneline`
>    + 命令 ：`git log --oneline`
>    + 命令 ：`git reflog`
>    + 注意：`HEAD@{移动到当前版本需要多少步}`

> 5. ##### 前进后退
>
>    + 基于索引值操作[`推荐`]
>      + 命令 ：`git reset --hard [局部索引值]`
>      + 例如 ：`git reset --hard a6ace91`
>    + 使用^符号：只能后退
>      + `git reset --hard HEAD^`
>      + 注：一个^表示后退一步，n个表示后退n步
>
>    + 使用~符号：只能后退
>      + 命令 ：`git reset -- hard HEAD~n`
>      + 注 ：表示后退n步

> 6. ##### 比较文件的差异
>
>    + 命令 ：`git diff [file name]`
>      + 将工作区中的文件和暂存区中的文件进行比较
>    + 命令 ：`git diff [本地库中历史版本] [文件名]`
>      + 将工作区中的文件和本地库历史记录作比较
>    + 注意 ：不带文件名比较多个文件

## 四、分支操作

> 1. ##### 创建分支
>
>    + 命令 ：`git branch [分支名]`

>2. ##### 查看分支
>
>   + 命令 ： `git branch -v`

> 3. ##### 切换分支
>
>    + 命令 ：`git checkout [分支名]`

> 4. ##### 合并分支
>
>    + 命令 ：`git merge [有新内容分支名]`

## 五、创建远程库地址别名

> 1. ##### 查看当前所有远程库别名
>
>    + 命令 ：`git remote -v`

> 2. ##### 添加远程库
>
>    + 命令 ： `git remote add [别名] [远程地址]`

> 3. ##### 推送
>
>    + 命令 : `git push [别名] [ 分支名]`

> 4. ##### 克隆
>
>    + 命令 ：`git clone [远程地址/别名]`

> 5. ##### 拉取
>
>    + `pull=fetch+merge` 
>
>    + 命令 ：`git fetch [远程库地址别名] [远程分支名]` 
>    + 命令 ：`git merge [远程库地址别名/远程分支名]` 
>    + 命令 ：`git pull [远程库地址别名] [远程分支名]` 

> 6. ##### 远程登陆
>
>    + 进入当前用户的家目录 
>
> + 命令 ： **cd ~** 
>
>    + 删除.ssh 目录
>
>      + **rm -rvf .ssh** 
>    + 运行命令生成.ssh 密钥目录 
>   + **ssh-keygen -t rsa -C atguigu2018ybuq@aliyun.com** 
>      
> + [**注意：这里****-C** **这个参数是大写的** **C**] 
>      
>    + 进入.ssh 目录查看文件列表 
>   + 命令 ：**cd .ssh** 
>    
>   + 命令 ：**ls -lF** 
>    
>    + 查看 id_rsa.pub 文件内容
>
>      + 命令 ：**cat id_rsa.pub**  
>
>        [***`github`***]
>
>    + 复制 id_rsa.pub 文件内容，登录 GitHub，点击用户头像→Settings→SSH and GPG keys 
>
>    + New SSH Key 
>
>    + 输入复制的密钥信息 
>
>    + 回到 Git bash 创建远程地址别名 
>
>      + 命令 ：**git remote add origin_ssh git@github.com:atguigu2018ybuq/huashan.git** 
>   
>    + 推送文件进行测试

## 六、eclipse中整合git案例

### 1.创建远程仓库

<img src="D:\user\notes\picture\git01.png">

<img src="D:\user\notes\picture\git02.png">

### 2.在eclipse中获取密钥

<img src="D:\user\notes\picture\git03.png">

### 3.将密钥添加到github

<img src="D:\user\notes\picture\git04.png">

<img src="D:\user\notes\picture\git05.png">

### 4.在eclipse中设置签名

<img src="D:\user\notes\picture\git06.png">

### 5.克隆一个远程仓库到本地

<img src="D:\user\notes\picture\git07.png">

<img src="D:\user\notes\picture\git08.png">

### 6.提交代码

<img src="D:\user\notes\picture\git09.png">

<img src="D:\user\notes\picture\git10.png">

## 七、IDEA中整合git

### 1.IDEA添加git

> file --- > settings

<img src="D:\user\notes\picture\git11.png">

### 2.创建git本地仓库

<img src="D:\user\notes\picture\git12.png">

### 3.提交到本地库

<img src="D:\user\notes\picture\git13.png">

### 4.提交到远程（ctrl + shift + k）

<img src="D:\user\notes\picture\git14.png">

`注：远程仓库一定要是本地提交的时候创建的，不然会被拒绝，如果远程分支在第一次提交到远程之前就存在则需要加--allow-unrelated-histories`

