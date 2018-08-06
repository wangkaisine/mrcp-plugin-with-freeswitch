# mrcp-plugin-with-freeswitch 

这是我的第一个Github工程，特别感谢 [Cotin网站](https://cotin.tech) 《构建简单的智能客服系统》[（一）](https://cotin.tech/AI/FreeswitchSetting/)、 [（二）](https://cotin.tech/AI/UniMRCPASR/) 、[（三）](https://cotin.tech/AI/UniMRCPTTS/) 对于构建过程的帮助，您可以在阅读本教程前，先行阅读这三篇文章，本次教程将基于此给出更多的操作细节，错误处理和其他技术讲解。

## 主要目的 

使用FreeSWITCH接受用户手机呼叫，通过UniMRCP Server集成讯飞开放平台（xfyun）插件将用户语音进行语音识别（ASR），并根据自定义业务逻辑调用语音合成（TTS），构建简单的端到端语音呼叫中心。

## 构建步骤

### 第一步 安装编译FreeSWITCH

本次示例在MacOS High Sierra 10.13.4版本中进行源码编译安装，并未使用软件包安装，具体安装步骤可见 [官网安装介绍](https://freeswitch.org/confluence/display/FREESWITCH/macOS+macFI+Installation) ，其他平台如Linux（Ubuntu、CentOS）应均能安装成功。

以下给出源码编译安装的步骤：


1.下载 [FreeSWITCH源码](https://freeswitch.org/stash/scm/fs/freeswitch.git)：

```
cd /usr/local/src
git clone -b v1.6 https://freeswitch.org/stash/scm/fs/freeswitch.git freeswitch
```

2.安装依赖库

```
brew install autoconf
brew install automake
brew install libtool
brew install pkg-config
brew install speexdsp
brew install speex
brew install libldns-dev
brew install OpenSSL
brew install pcre
brew install pkgconfig sqlite3
brew intall lua
brew install opus
brew install libsndfile
```

>注：其他系统平台请自行确认依赖库内容，可能的搜索结果：[Ubuntu/CentOS FreeSWITCH 安装依赖](https://blog.csdn.net/u012121105/article/details/74238595)。

3.编译安装

```
cd freeswitch/
./configure
make
make install
make cd-sounds-install
make cd-moh-install
```

4.运行

```
cd /usr/local/freeswitch/bin
./freeswitch
```

即可启动应用。

FreeSWITCH默认配置1000-1019（20个）用户，默认密码1234，您可以提前跳转到第四步“测试与验证”，登录并拨打5000，可以听到默认ivr的示例语音菜单指引。

### 第二步 配置编译UniMRCP Server

本次示例在CentOS 7中进行源码编译安装，感谢由Github用户cotinyang提供的已经写好的集成讯飞SDK的UniMRCP Server源码。

1.下载 [UniMRCP Server Plugin Demo 源码](https://github.com/cotinyang/MRCP-Plugin-Demo)：

```
cd /opt
git clone https://github.com/cotinyang/MRCP-Plugin-Demo.git MRCP-Plugin-Demo
```

2.编译准备环境

```
cd MRCP-Plugin-Demo/unimrcp-deps-1.5.0
./build-dep-libs.sh
```
>注：过程中需要输入两次y，并确认。

3.编译安装unimrcp

```
cd unimrcp-1.5.0
./bootstrap
./configure
make
make install
```
即可在/usr/local/中看到安装好的unimrcp。

### 第三步 集成讯飞开放平台SDK

实际上一步下载的MRCP-Plugin-Demo中已有SDK包，但从讯飞开放平台下载的SDK包和用户以及用户创建的应用相关联，因此需要将third-party/xfyun中的文件和文件夹全部删除，重新下载解压属于自己的SDK，目录与源代码基本一致。

1.需要你并注册登录 [讯飞开放平台](https://www.xfyun.cn/) ，进入控制台页面，并创建应用；

>注：创建应用页面中的应用平台选择“Linux”。

2.在“我的应用”界面获得你的APPID，并为该应用“添加新服务”，选择需要的“语音听写”和”在线语音合成“服务（本示例需要）；

3.点击右侧“SDK下载”，在跳转页面中确认“选择应用”已经选中了你创建的应用，“选择您需要的AI能力”选中上述两项服务，并点击“SDK下载”等待SDK生成与完成下载。

4.将下载的zip包，解压并替换MRCP-Plugin-Demo/unimrcp-1.5.0/plugins/third-party/xfyun/下的所有文件及文件夹。

5.重新编译安装unimrcp。

### 第四步 测试与验证

测试工具：Adore SIP Client

在App Store（其他手机系统请到对应应用市场）中搜索“Adore SIP Client”，并下载。

![image](https://github.com/wangkaisine/mrcp-plugin-with-freeswitch/blob/master/image/adoresipclient.png)

其中SIP IP是FreeSWITCH服务开启的IP与port，USER NAME如上所述可选1000-1019，PASSWORD默认为1234。点击Login，并拨打5001进行语言测试。

## 其他相关资料

FreeSWITCH主页：https://freeswitch.com/

Apache APR：https://apr.apache.org/

讯飞SDK包导入方式：https://doc.xfyun.cn/msc_linux/SDK%E5%8C%85%E5%AF%BC%E5%85%A5.html
