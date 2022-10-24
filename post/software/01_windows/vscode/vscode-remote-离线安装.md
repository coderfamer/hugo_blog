---
title:  vscode remote 离线安装
date: 2022-06-06 15:07:32
categories: ['software', 'vscode']
tags: ['vscode']
draft: false

---

# vscode remote 离线安装

---

内网环境下，想要使用 vscode 并远程到 linux 开发机器，需要手动下载

## 安装

### 系统要求

- git 或者 ssh 客户端

### windows 安装 vscode
#### 安装 vscode

自行下载安装 [vscode](https://code.visualstudio.com/Download)

安装完毕之后，打开 vscode  **help->about** 查看版本信息，记录 commit

#### 安装 remote 插件

插件下载地址 [Extensions for Visual Studio family of products | Visual Studio Marketplace](https://marketplace.visualstudio.com/VSCode)

- ms-vscode-remote.remote-containers-0.238.1.visx
- ms-vscode-remote.remote-ssh-0.81.2022060215.vsix
- ms-vscode-remote.remote-ssh-edit-0.80.0.vsix
- ms-vscode-remote.remote-wsl-0.66.3.vsix
- ms-vscode-remote.vscode-remote-extensionpack-0.21.0.vsix

侧边栏 **Extensions图标->侧边栏三个点(···)->Install from VSIX** 本地离线安装插件

### linux 安装 vscode

windows 和 linux 对应的 commit 是一样的，根据 commit 下载，例如 1.67.2 的 commit 是 **c3511e6c69bb39013c4a4b7b9566ec1ca73fc4d5**

下载

```shell
wget https://update.code.visualstudio.com/commit:c3511e6c69bb39013c4a4b7b9566ec1ca73fc4d5/server-linux-x64/stable
tar -zxvf stable    # 解压后目录名为 vscode-server-linux-64 
mkdir -p ${HOME}/.vscode-server/bin && cd ${HOME}/.vscode-server/bin
mv vscode-server-linux-64 ${HOME}/.vscode-server/bin/c3511e6c69bb39013c4a4b7b9566ec1ca73fc4d5
```



拷贝 ssh 密钥到远程服务器，就可以远程连接了.其他插件自行安装





