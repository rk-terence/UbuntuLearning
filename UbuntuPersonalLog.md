# 2018.4.19

下载安装了 `polipo` 、知道了一些命令。使用 `shadowsocks-qt5` 进行科学上网。同时也下载了另一个版本的shadowsocks，暂时不管用（非客户端）。

## 1. polipo

使用polipo进行了sock5代理转http代理。希望可以管用。具体使用方法：

`sudo service polipo start` 可以开启polipo的服务。

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