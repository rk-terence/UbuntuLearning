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