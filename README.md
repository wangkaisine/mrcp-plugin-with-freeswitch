# mrcp-plugin-with-freeswitch 

这将是我的第一个github工程。

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

2.编译安装

```
cd freeswitch/
./configure
make
make install
```

3.安装声音文件

```
make cd-sounds-install
make cd-moh-install
```

4.运行

```
cd /usr/local/freeswitch/bin
./freeswitch
```

即可启动应用。

### 第二步 配置编译UniMRCP Server

下载代码：https://github.com/cotinyang/MRCP-Plugin-Demo

### 第三步 集成讯飞开放平台SDK

下载讯飞开发平台SDK包，上一步下载的MRCP-Plugin-Demo中已有SDK包，但实际上，讯飞的SDK包和用户以及用户创建的应用相关联，因此需要将third-party/xfyun中的文件删除，重新下载属于自己的SDK，目录与源代码基本一致。

### 第四步 测试与验证

测试工具：Adore SIP Client

在App Store（其他手机系统请到对应应用市场）中搜索“Adore SIP Client”，并下载。

## 其他相关资料

FreeSWITCH主页：https://freeswitch.com/

讯飞SDK包导入方式：https://doc.xfyun.cn/msc_linux/SDK%E5%8C%85%E5%AF%BC%E5%85%A5.html
