# 2018.4.19

下载安装了 `polipo` 、知道了一些命令。使用 `shadowsocks-qt5` 进行科学上网。同时也下载了另一个版本的shadowsocks，暂时不管用（非客户端）。

## 1. polipo

使用polipo进行了sock5代理转http代理。希望可以管用。具体使用方法：

`sudo service polipo ` start可以开启polipo的服务。

`export http_proxy="http://127.0.0.1:8123/"` 可以暂时打开端口作为http_proxy。

一些其他相关的代码：（特别是git）

```shell
http_proxy=http://localhost:8123 apt-get update

http_proxy=http://localhost:8123 curl www.google.com

http_proxy=http://localhost:8123 wget www.google.com

git config --global http.proxy 127.0.0.1:8123
git clone https://github.com/xxx/xxx.git
git xxx
git xxx
git config --global --unset-all http.proxy
```

十分感谢 `polipo` , 因为它我成功地在ubuntu上通过terminal安装了 `typora`. 从此, typora可以从terminal启动了.



==但是这样设置之后出现了一些问题==

问题:

---

- git无法push操作, 说是端口1080拒绝. 而shadowsocks正是使用的1080端口;但是此时shadowsocks客户端已经被停止了.
- curl也无法正常抓取网页, 即使是百度.

---

参考了[stack overflow](https://stackoverflow.com/questions/24543372/git-cannot-clone-or-push-failed-to-connect-connection-refused)的一个最高票答案, 删除掉了一些和proxy有关的环境变量. 至于为什么这些环境变量会导致问题的出现, 我的猜测:

> It seems that git tries to use a local proxy. Please check your global network-settings  and those of git.

大概是因为我的shadowsocks-qt5产生了变量. 或者是另外一个ubuntu系统设置的作用到整个系统产生了全局变量, 让git或者是curl发生了异常.



总结一下,  目前我实现的科学上网还是比较复杂的. 需要打开`shadowsocks-qt5` + `系统设置` .
同时, 如果想利用socks5代理转http, 还需要 `polipo` 的帮助. 并且这样做会带来副作用, 消除副作用需要通过unset全局变量的方式.



# 2018.4.21

今天仍旧对Ubuntu系统做了一点实验.

## 文件夹的分布:

> 只有一个 `/` 表示时本计算机
>
> /下, 有home, etc, bin等等文件夹.
>
> ~代表的是 /home/<currentuser>.

## 环境变量

目前, 希望把`/Softwares/clion-1.../bin` 加入到PATH环境变量里面, 加入之后都没有成功. 尝试的时候还是不能从任意目录运行 `clion.sh` . 不知道具体原因. 

### 更新:

原因似乎已经找到. 

因为我犯了一个错误. 我错认为必须使用`./clion.sh` 命令才可以打开Clion. 但是, 事实上, BASH里面输入`clion.sh` 就可以打开 CLION. 

借此, 也学习到了一个知识点:

​	在Linux系统里面,  `./`代表的是当前目录.

## 关于SHELL里面的命令

`env|grep -i proxy`

其中, `|` 是管道命令, 把前一项的结果传递给后面一项. 

此处, env可以打印出所有的environment variables, 然后给grep. 之后, grep根据参数 -i (不区分大小写)找到有proxy的变量. 

>`grep` : Global Search Regular Expression and Print Out the Line. 强大的文本搜索工具.

### 接4.19的代理设置

通过实验, 已经查明, 那几个环境变量是我在系统设置中使用了代理并应用于整个系统导致的. 
因此, 应该仅仅打开shadowsocks也可以实现翻墙. 但是使不使用端口就不确定了.



# 2018.4.23

## 重要：无系统设置配置代理

昨日对代理的理解更加深入了一层。`shadowsocks-qt5` 的工作机制是：

开始工作，但是只对制定的端口工作。其他的软件如果想要启动代理，必须通过使用`shadowsocks-qt5` 使用的端口才可实现。如果系统设置里面开启了全局代理，那么就会产生proxy的环境变量，让任何一个程序都试图使用那个端口。这样有时候并不好。加入我仅想让Chrome软件翻墙，那么我只需要设置Chrome，让Chrome使用那个端口作为代理就好了。



## 安装软件：stardict console version

安装成功，但是词典资源比较难找，所以现在还不能进行查词操作。启动方法：

Bash中输入sdcv即可。



## 下载MATLAB安装包

感觉matlab安装之后太大了，所以决定先不安装matlab。

今后如果有机会的话，会尝试安装MATLAB。毕竟如果在ubuntu里面也可以使用的话就很方便了。



# 2018.4.26

今天在Ubuntu系统里面通过一些命令，把Ubuntu的本地时间改为了+8世界时。至此，Windows和Linux双系统的时间显示不再冲突了。

# 2018.4.28

前几天一直在做国创，在Windows 10 系统里面的国创的`DataAnalysis`仓库里面对做了很多的commit。今天打开Ubuntu，重新进入那个仓库，发现了问题：

1. 使用`git status`命令，发现`working tree`并不是clean的。然而，在windows系统里面，这个仓库最近的提交我已经commit了。
2. 使用`git fetch`命令，发现本地仓库和远程仓库是同步的。

这两个问题说明，Ubuntu的git的`Local Repository`没有和已经和Windows上面的同步了。但是，`Index（暂存区）`两个系统没有同步。至此送了一口气，只需`git reset --hard HEAD`，返回到Windows最近的提交即可。

但是这样的话，问题即使可以解决，但是还是存在问题的。目前还不清楚彻底的解决方法。猜测的问题来源：

1. Ubuntu系统的Git里面的暂存区和Windows的暂存区不兼容。
2. Ubuntu和Windows的Git不是同一个账户，从而导致差异。（但是，现在存储区是一样的，所以觉得这种可能性不大）



# 2018.4.29

之前在电脑上安装了`sdcv`，方便自己查词使用。但是，`sdcv`的词库太难找了，所以放弃，使用`sudo apt-get remove sdcv`将之卸载。

随后尝试下载有道官方的deb包进行有道词典的安装。下载下来之后，发现屏幕取词确实存在bug，并且最小化到系统托盘之后无法再打开。很惹人麻烦，遂卸载。卸载的时候不知道如何卸载，于是百度。使用dpkg。dpkg需要使用命令`sudo dpkg -r youdao-dict`。

卸载之后，发现仍然在运行。可能因为它是基于python的，重启之后就不再运行了。

> [Linux之apt与dpkg的区别](https://blog.csdn.net/baidu_28149499/article/details/56307190)

之后尝试下载了一个GitHub上面的开源项目：`wudao-dict`。这个词典是有道词典的命令行版本。

按照解说进行下载安装，Git克隆不成功，所以直接在网络上下载。解压包，然后按readme进行setup。成功之后，在命令行可以运行。但是，可能是因为不是root权限，使用查词功能。遂百度在终端中开启root的方法（仅本终端）。百度到的开启终端的方法：

`sudo su - root`

键入这个命令，就可以暂时开启终端的root权限，就不用每个命令都输`sudo`了。



# 2018.5.2

今日在宿舍，再次遇到了无线网络连不上的问题（估计是因为宿舍里面的ZJUWLAN信号弱），遂再拾让Ubuntu连接学校有线VPN的想法。具体参考了这个网页：

[ubuntu VPN连接教程（玉泉 16.04 闪讯 均可用）](https://www.cc98.org/topic/4651082)

某位学长牛人撰写了这个软件包。

使用这个软件包之后，成功地使用VPN连上了。使用命令：

```shell
sudo vpn-connect -c # 设置用户名和密码（忽略域的设置，现在ZJU似乎已经不用这个了）
sudo vpn-connect # 等待一会儿，连接成功。
```

再次发现，ZJU的大神还是无处不在，自己要向他们的优秀之处学习。

希望自己今后能够有机会了解一下其中的原理。





# 2018.5.3

## Morning

今后有机会的话，准备转战Ubuntu，如果遇到Linux系统无法轻易解决的问题，再到Windows系统上面解决。所以，我准备安装Matlab。

此外，看到了一个Ubuntu使用utorrent的CSDN帖子，先收藏，之后有机会可以试一试。

## Afternoon

经过一个上午的下载，Ubuntu上面的Matlab下载成功。发现了些许问题：

1. 缩放问题。显示过小。通过命令行的settings调整，把PersonalValue调整到1.25，大致解决问题。

2. 中文字符乱码。经过调查，发现是因为在Windows下面编写的代码的语言是GBK编码的。然而，Matlab在Linux下面的默认编码模式是UTF-8. 故出现问题。现在在网上找到了一个帖子，可能可以解决问题：

   [ubuntu下文件编码查看与转换](https://blog.csdn.net/Eric_LH/article/details/77435097)

   ```shell
   # 1.查看文件编码使用file命令 
   file filename.txt 
   # output: filename.txt UTF-8 Unicode text, with escape sequences

   # 2.编码格式转换使用iconv命令 
   # iiconv的命令格式如下： 
   iconv -f encoding -t encoding inputfile 
   # 比如将一个UTF-8 编码的文件转换成GBK编码 
   iconv -f UTF-8 -t GBK file1 -o file2
   ```

   摘自该网页。

3. 再次切换到Ubuntu系统，使用上述命令之后，发现得到了转换。不过，有个小问题不是很明白，就是使用命令`file PreprocessData_Beta`（ANSI，GBK）的时候，输出是：

   `PreProcessData_Beta.m: ISO-8859 text`

   ISO-8859似乎是一个国际的编码方式，不是太了解为什么。按照我的理解，此处应该显示的是`GBK`.




# 2018.5.8
Today, several new progress.
## New commands learned in BASH
### about files:
1. about files
   sudo touch myfile
   sudo gedit myfile
   sudo rm myfile
   sudo mv file1 file2	# change the name of a file
   sudo mv filename target_destination
   
2. about directories
   sudo mkdir mydir
   sudo rmdir mydir # delete an empty direcroty
   sudo rm -rf mydir	# delete an non-empty direcroty
   sudo mv dir1 dir2 # change the location of dir1 to dir2

3. about `cp`
   grammar:
   > cp [option] file_or_directory target_file_or_directory

## Java JDK 'installation'
Today I had the first class of Java.

During this session, I learned new concepts of JDK, JVM, heap and stack in Java, and so on. 

I tried to download JDK on my own computer. Firstly, `IntelliJ IDEA`. When installation (if that can be called an installation) is finished, the IDE told me that there is no JDK in my PC. So I started downloading it.

I downloaded from oracle's official site, and the version:

```
java version "10.0.1" 2018-04-17
Java(TM) SE Runtime Environment 18.3 (build 10.0.1+10)
Java HotSpot(TM) 64-Bit Server VM 18.3 (build 10.0.1+10, mixed mode)
```

This version is the newest. After installation, I started to add it into the environment variables. I tried `/etc/profile`, **but that seems to be available only on the current terminal**. When I closed the terminal, `java -version` will return nothing but the tip of how to install packages related with Java.

## New progress about the environment variables
There are 3 levels of environment variables. 
1. System level
   environment variables can be useful for all users.
   to add them, you can:

2. User level
   useful for only one user.
   `~/.bashrc`
   `~/.bash_profile`
   
3. Shell level
   only available in current terminal.
   
**Frequently used commands to set environment variables**
> echo $
> export # Set new env.
> env 
> set
> unset
> readonly
> To modify an env, just simply use `MYNAME = "xxx"`.

**several frequently used envs**
1. PATH
2. HOME
3. HISTSIZE
4. LOGNAME
5. HOSTNAME
6. SHELL
7. LANG/LANGUAGE
8. MAIL
9. PS1
10. PS2
11. SECONDS

Time is limited, so when I get in touch with those variables listed above, I will learn them.

## Solve the problem of sougou-qimpanel
Recently, I frequently meet the problem that sougoupinyin doesn't work correctly. It is not that easy to solve without looking for help on the Internet. 

On website
[Ubuntu16.04下搜狗输入法不能输入中文（重启搜狗输入法）](https://blog.csdn.net/miaoqiucheng/article/details/53350963)
I have found one possible solution. 

I should type in in BASH:

```shell
killall fcitx # kill the progress fcitx
killall sogou-qimpanel # kill the sougoupinyin
fcitx # Restart fcitx
```

I hope this can work next time I meet this problem.

I have tried to solve this through the tips above. But that seems not work. I have tried `killall fcitx`, but it seems to restart soon after I kill it. I do not know why, maybe it can automatically start up. Question not solved. I need to get more knowledge about it.



# 2018.5.9

Today, more of the permissions in Linux system.

This action is motivated by the hardness of using wudao-dictionary. When I use this, I must use `sudo su - root` to let the bash get the root access. Otherwise, I need to add additional `sudo` to every command related to wudao-dictionary.

This added to my inconvience. I want to solve this.

From the *Python Traceback*, I can see that it is because of the writing of a file located in */Softwares/.../usr/...* is denied on accound of lack of permission.

After consulting to the Internet, I find that `chmod` can help me change the permission status.

The instructions are copied from `chmod --help`

用法：chmod [选项]... 模式[,模式]... 文件...
　或：chmod [选项]... 八进制模式 文件...
　或：chmod [选项]... --reference=参考文件 文件...
Change the mode of each FILE to MODE.
With --reference, change the mode of each FILE to that of RFILE.

  -c, --changes          like verbose but report only when a change is made
  -f, --silent, --quiet  suppress most error messages
  -v, --verbose          output a diagnostic for every file processed
      --no-preserve-root  do not treat '/' specially (the default)
      --preserve-root    fail to operate recursively on '/'
      --reference=RFILE  use RFILE's mode instead of MODE values
  -R, --recursive        change files and directories recursively
      --help		显示此帮助信息并退出
      --version		显示版本信息并退出

Each MODE is of the form '[ugoa]*([-+=]([rwxXst]*|[ugo]))+|[-+=][0-7]+'.

GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
请向<http://translationproject.org/team/zh_CN.html> 报告chmod 的翻译错误
Full documentation at: <http://www.gnu.org/software/coreutils/chmod>
or available locally via: info '(coreutils) chmod invocation'

Some specific examples

```shell
chmod u+x,g+w f01　　//为文件f01设置自己可以执行，组员可以写入的权限
chmod u=rwx,g=rw,o=r f01
chmod 764 f01
chmod a+x f01　　//对文件f01的u,g,o都设置可执行属性
```

After the modification using `chmod`, I can successfully use `wudao-dict` without root access in terminal!




# 2018.5.10
## 
Yesterday evening, I installed zsh and oh-my-zsh, for I have seen it recommended several times on Zhihu. 

`zsh` is a shell. And oh-my-zsh is a configuration tool to make zsh more powerful.

After installation, I followed the oh-my-zsh github page to configure it. Everything seems fine, and now my default shell is zsh.

A good try, isn't it?

## Problem of sogou-qimpanel
I can no longer stand the unstability of sogou-qimpanel. I can logout to solve this temporatily, but this wastes my time. More importantly, this will let me lose the work I am doing.

After balancing, I chose to remove sogou-qimpanel.

I installed fictx-googlepinyin, but soon discovered that this is much similar to the original Input Method of Ubuntu. So, I removed googlepinyin, and then returned to the original Input Method. 

Up to now, no big problems happen to me. This is not that convience but stable.

## evince
I've found the package name of `文档查看器` is `evince`.

Sometimes, not knowing the exact name of a package is frustrating. Now, I don't have the quick method to check what the name of the program is.

## COMMAND --help
I have found this command extremely useful. This can help me be familiar with the command without consulting the Internet.

For example, I learned how to get the detailed info of a dir (`ls -l`) through `ls --help`.

## An Attempt to Use VIM (Failed)
I have found it hard to start using vim.
I give up. Gedit is good.


# 2018.5.16
Today, more of theoretical learning of BASH shell language.

## !
`!` is called *Event Designator*, which can help us easily refer to commands in history.

There are three ways to use it:

- `!n` It will turn into the nth command.
- `!-n` the last nth command.
- `!!` means `!-1`
- `!string` It will turn into the most recent command that started with *string*.
- `!?string[?]` a command that contains *string*. The second *?* can be omitted.

What's more, in bash language, 

`#!/bin/sh` means this script will be interpreted and implemented by */bin/sh.*


## About Quotation Marks in BASH
In Bash, '', "", and ` ` have different meanings.

- ''
  '' means to simply transfer what's inside into a string. For example,
  `'\$HOME'`
  will be the same as `\$HOME`.
  
- ""
  "" can also turn what's inside into a string. However, This time, some special characters will take effect. For example, 
  **!, $, \, and |**
  
- ` `
  ` ` can into a the output of the implementation of what's inside.


# 2018.5.17
Now I started the learning of VIM.

I know it will take me some time to be familiar with the operations, but I believe this can save time (What is more important is that I think this is cool). 

I have just finished the tutor, and now I basically know what vim can do and what vim can not do. I will further do some other work to make *vim* more of my own preferred style.

Next step:
**Prompts**.

## Auto completion of brackets and paratheses!
I have made the auto completion of brackets and paratheses complete!
Just like what I did on VS!
COOL!!!

## Coding Prompts!
coding prompts is a built-in function in VIM. I can use [Ctrl+p] or [Ctrl+n] to activate it whenever I want.


# 2018.5.18
Today, I continued to edit with VIM. VIM is a great editor, but there are still some problems.

## VIM-GTK
One thing is, I previously cannot paste from the clipboard of the Linux System.
To address this, I surfed the Internet. On the Internet, they suggested installing vim-gtk. It seems to be a extension of VIM, with which I can have more regesters to visit the clipboard.

I installed that, and now I can copy text from outside VIM.

## Multiple files in VIM
I think this is the time to learn how to operate with multiple files on VIM.
I've read the tutorial file, and now I know the basic commands to edit different files and change to different files.


# 2018.5.22
Today, new learning of VIM.

## VScode on Windows.
I turned back to windows, and found that the font *consolas* is beautiful. VScode is beautiful, and it also supports the VIM plug-in. For now, I think VScode is more convenient than pure vim. But I think as I know more about VIM, my thoughts will change. I will have more powerful *.vimrc* and more powerful syntax highlighting and more powerful language support in VIM.

However, this takes time. To get this more of mine, I need to spend more time on it.

## VIM syntax highlighting
I learned how VIM does the syntax highlighting through the help file.

First, VIM have some built in types like *statement, type, ...*. With these built-in types, I can define some keywords that can be categorized into the categories. So the highlight strategy can work. The color and the appearance of a letter can be changed through a file.

## VIM indent
VIM has built-in indent ways for C or C-like languages, such as c, cpp, and java. For these languages, I can simply turn `cindent` on. However, I found this not helpful on my own PC. After several attempts to find out the reason, I found that it was because I have set the `ioremap` for brackets autocomplete. This is will cause the original function of Cindent not to work properly.

Maybe I should choose one to satisfy another. For now, I think auto-indent for a language is very useful, so I disabled the ioremap. Now, cindent appears good for me, except that no autocompletions are made.

I up to now don't know how to get both satisfied. Maybe this can be done, but as my knowledge of VIM is so limited, I cannot do it right now.


# 2018.5.24 About directory names in Linux System
In most Linux systems, the names of directories are very similiar. They seem to obey the same rule.

For example, on my ubuntu system, there are directories named *usr, etc, bin, and so on*.

- usr
  In *usr*, there are apps. It means that Apps are installed in *usr* instead of other directories by default.

- etc
  In a linux system, *etc* are the abbreviation of *et cetera*. This is a bit because that configuration files are stored in it.

- share
  In share directory, there are files can be shared. my understanding is this: other files can visit this directory to help with their connection with other apps.

  For example, in DIR: */usr/share/applications/*, there are many files that mark an app. They are shortcuts for applications. Just a few minutes ago, I established a desktop file in that direcroty to get a quick start of TIM(based on wine, it is an AppImage file). After a few failed attempts, I succeeded.
 

# 2018.5.25 new command in shell

## &
In linux system, `&` command means to let the command before it run at the backstage. For example, if I run a command that needs to run for a long time, and I do not want to open another shell to do another work during this time, I can use `&`. However, the command that it runs will return results on the same shell, and you will propably be annoyed by it.

## nohup
`nohup` is the abbreviation of *no hang up*. From its name, we can see that the function is that when you exit from the terminal, the work will not be terminated. Instead, the work will continue when you terminate the shell. If you want to cancel it, you should use `ps` command to find the PID of it, and then kill it.

Example:

```shell
nohup command &
```

However, when I use this method for netease-cloud-music, this doesn't work, but pycharm work well. This may be due to the inner cause of netease-cloud-music.


Through this learning, I think it is the time that I find a systematic book about linux to learn it. My knowledge from Baidu is just too fragmented, and whenever I meet a problem, I need to find how to do it online, and then many other fields that I don't understand will come out. This feeling is not good.
