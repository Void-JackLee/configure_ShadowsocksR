# 前言
在本文中，你将学习如何在Linux服务器上配置ShadowsocksR。

# 准备
* 一台电脑(最好是运行Linux或者macOS)或者一台手机
* 可能需要一台VPS([注册阿里云服务器](https://www.jianshu.com/p/d132e1a14fcb))

# 开始
### 1. 登陆服务器
* 首先我们先打开安卓手机或电脑终端。![终端](https://upload-images.jianshu.io/upload_images/16312001-5e622ebf96d9b0d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 在“[注册阿里云服务器](https://www.jianshu.com/p/d132e1a14fcb)”文中我已经提到了如何使用私钥登陆vps，所以本文直接输入以下命令登陆：
```
ssh -i [你钥匙的名字] root@[你之前看到的那个服务器ip]
例如：
ssh -i mykey.pem root@88.88.88.88
```
登陆成功![登陆成功](https://upload-images.jianshu.io/upload_images/16312001-6a85d8141f2d2813.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2. 开始配置ShadowsocksR
由于我的服务器已配置完毕，所以我在本机上做演示。
首先先安装git，输入命令：
```
yum install git
```
中途停下来让你输入y/n的话就输入y并回车。![安装完毕](https://upload-images.jianshu.io/upload_images/16312001-640b143384bf00e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以下内容参考[ShadowsocksR 多用户版安装教程](https://github.com/shadowsocksr-backup/shadowsocks-rss/wiki/Server-Setup(manyuser-with-mudbjson))

键入下列命令安装基本库，可能会有些缓慢：
```
yum install python-setuptools && easy_install pip
```

键入以下命令来下载ShadowsocksR：
```
git clone -b manyuser https://github.com/shadowsocksr-backup/shadowsocksr.git
```
![正在下载](https://upload-images.jianshu.io/upload_images/16312001-75f9df6b16ec4c16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)完成后我们输入
```
ls
```
看一眼文件夹里面有什么：![看到了我们下载完成的ssr](https://upload-images.jianshu.io/upload_images/16312001-a911362302ec3d57.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后键入下列命令进入文件夹：
```
cd shadowsocksr
```
![进入文件夹](https://upload-images.jianshu.io/upload_images/16312001-11ac0f9ea820f74a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)再次ls看下有什么：
```
ls
```
![就是这些了](https://upload-images.jianshu.io/upload_images/16312001-2a80696b858bd7c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)**下面我们便来配置ShadowsocksR：**
先初始化下：
```
./initcfg.sh
```
执行完之后，我们就可以开始编辑配置文件了。
在这里，我将使用一个Linux下十分流行并强大的编辑工具vim来编辑。
```
格式：
vim [文件名]
```
现在我们来编辑我们的配置文件：
```
vim userapiconfig.py
```
回车执行后就是这样的（按照图片中提示移动光标）：![按照图片中提示移动光标](https://upload-images.jianshu.io/upload_images/16312001-ed370ffa41e66bce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)或者这样移动光标：
直接输入，然后回车：
```
:set mouse+=a
```
![输入并回车](https://upload-images.jianshu.io/upload_images/16312001-0a7c9baa6d609469.gif?imageMogr2/auto-orient/strip)这样就可以使用鼠标了，如果你是用安卓的话，就可以用触摸屏来点击了。

不管怎么样，把光标移到下图的地方就行：![移动光标到这个地方](https://upload-images.jianshu.io/upload_images/16312001-6ddd6bed4132cd29.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)然后我们按键盘上面的那个i键（代表insert，插入），下面就会出现这样：![插入（中文），英文的话就是INSERT](https://upload-images.jianshu.io/upload_images/16312001-7b7e8369c0935076.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)像下图一样删除127.0.0.1并换成你的服务器ip，并改为API_INTERFACE = 'mudbjson'![记得把图中那个ip换成你自己服务器的那个ip](https://upload-images.jianshu.io/upload_images/16312001-c76c108e01c84aa8.gif?imageMogr2/auto-orient/strip)**完成后按键盘左上角的esc键，然后输入:wq（w：write保存，q：quit退出）回车完成输入。**![esc按键](https://upload-images.jianshu.io/upload_images/16312001-ef00d1f1c641d726.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)![保存并退出](https://upload-images.jianshu.io/upload_images/16312001-cb7da718d1a89d7e.gif?imageMogr2/auto-orient/strip)

### 3. 添加SSR用户
输入下列命令来配置SSR用户：
```
python mujson_mgr.py
```
然后他就会给你看一大坨帮助：![帮助](https://upload-images.jianshu.io/upload_images/16312001-6b9c11675db40e3f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)这里翻译了下：
```
usage: python mujson_manage.py -a|-d|-e|-c|-l [OPTION]...

Actions:  # 动作
  -a                   add/edit a user  # 添加或编辑一位用户
  -d                   delete a user  # 删除一位用户
  -e                   edit a user  # 编辑一位用户
  -c                   set u&d to zero # 把用户的u(update:上传)和d(download:下载)流量清空
  -l                   display a user infomation or all users infomation # 显示一位用户的信息或所有用户的信息

Options:  # 选项
  -u USER              the user name  # 用户名
  -p PORT              server port (only this option must be set if add a user)  # 服务的端口号，如果是添加用户的操作，那么必须加上该选项来指定端口
  -k PASSWORD          password  # 用户密码
  -m METHOD            encryption method, default: aes-128-ctr  # 加密方式，默认是aes-128-ctr
  -O PROTOCOL          protocol plugin, default: auth_aes128_md5  # 协议，默认是auth_aes128_md5
  -o OBFS              obfs plugin, default: tls1.2_ticket_auth_compatible  # 混淆方式(为了躲避绿墙)，默认是tls1.2_ticket_auth_compatible
  -G PROTOCOL_PARAM    protocol plugin param  # 协议附加参数，可用于限制该端口的设备连接数量，例如-G 3就是限制3部设备连接
  -g OBFS_PARAM        obfs plugin param  # 混淆附加参数
  -t TRANSFER          max transfer for G bytes, default: 8388608 (8 PB or 8192 TB)  # 一个用户最大使用流量，默认8 PB(即8192 TB)
  -f FORBID            set forbidden ports. Example (ban 1~79 and 81~100): -f "1-79,81-100"  # 设置禁止的端口，也就是用户无法访问这几个端口，例子：禁止 1~79 和 81~100，那么输入-f "1-79,81-100"
  -i MUID              set sub id to display (only work with -l)  # 设置显示的子id（只针对-l有效）
  -s SPEED             set speed_limit_per_con  # 设置端口每部设备的速度
  -S SPEED             set speed_limit_per_user  # 设置整个端口的速度

General options:  # 通用选项
  -h, --help           show this help message and exit  # 显示这个页面
```
所以，我们可以用一下命令来添加一位用户：
```
python mujson_mgr.py -a -p 7392 -u jack -k 123456 -m chacha20 -o http_simple
```
仔细看过帮助的朋友应该已经知道来这句话的意思了：添加一位名叫jack的用户，并分配了7392端口给他，密码是123456，加密方式为chacha20，混淆方式为http_simple。![添加完成](https://upload-images.jianshu.io/upload_images/16312001-2d90d289f5dfaa36.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#####补充
这里的加密方式、协议、混淆方式有很多种，客户端上都有列举，大家可以自行查看，具体选择什么，大家可以谷歌，这里我推荐chacha20作为加密，http_simple作为混淆方式，协议默认，也就是我试例命令里打的那样。![差不多就这样吧](https://upload-images.jianshu.io/upload_images/16312001-a37205606bbc90fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3. 开启服务
输入下列命令开启服务：
```
./run.sh
```
但是，假如你在客户端尝试连接的话会失败的，如图所示，客户端测试发现连流量都发不出去，因为我们没有在防火墙那里放行这个端口，被堵住了。![被堵住了](https://upload-images.jianshu.io/upload_images/16312001-2de40e7a85e3e64c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)所以我们得前往我们的vps设置端去更改下。![配置规则](https://upload-images.jianshu.io/upload_images/16312001-e74ab2575e3d1764.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)进去之后在右上角点击“添加安全组规则”![添加](https://upload-images.jianshu.io/upload_images/16312001-186f37c879744ba6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)按图填写：![添加安全组](https://upload-images.jianshu.io/upload_images/16312001-3be23b304d5deabc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)确定之后就好了。

##### 但是，请注意，使用chacha20加密方式还是用不了的：
![可以发了，但没回来的，还是上不了网](https://upload-images.jianshu.io/upload_images/16312001-258a73a0442c75b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**没有进来的流量，怎么办**
现在来分析一波，我们回到服务端，执行
```
./stop.sh
```
先停止服务，然后我们再运行含有log记录的，看下究竟是什么回事：
```
./logrun.sh
```
开启之后，我们ls一下看下log存哪里了：
```
ls
```
嗯，果然多了个：![就他了](https://upload-images.jianshu.io/upload_images/16312001-b7c9b6280f962e6d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)**好，我们在手机上在尝试上下网，当然还是上不去的，但是这时候log就已经记录下来了，我们看下是什么回事把！**
```
vim ssserver.log
```
#### 把光标滑到最低端：
![哇，全是错误，难怪不行](https://upload-images.jianshu.io/upload_images/16312001-4ff80ba6a9801ac1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 **解决方案**
把没有的东西装起来呗。。。
先直接输入
```
:q
```
回车后退出vim，然后我们来安装这个缺失的支持库：
```
yum install epel-release -y
yum install libsodium -y
```
分别执行完这两个命令后就ok了![接收开始有流量了！](https://upload-images.jianshu.io/upload_images/16312001-c7f22d53453706bb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
# 至此，SSR就配置完毕了，本文已列举了配置过程中可能会遇到的问题，但如果还有别的问题，可以在评论区留言。



