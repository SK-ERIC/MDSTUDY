# Nodejs 之 Windows 下的安装与配置

## Windows 安装包 (.msi)

进入[Nodejs 官网下载页](https://nodejs.org/zh-cn/download/)， 选择长期支持版，选择msi 对应版本下载![image-20210515101844290](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20210515101844290.png)



下载完成后双击运行进行安装，安装过程 next 默认配置即可。（msi文件在安装过程中会直接添加path的系统变量，为了能在任意目录下执行cnpm、webpack等第三方包中带的命令，变量值是你的安装路径，例如“C:\Program Files\nodejs”）

## Windows 二进制文件 (.zip)



## windows 包管理器

### Scoop下载

[Scoop](https://github.com/lukesampson/scoop)下载地址（[https://github.com/lukesampso...](https://github.com/lukesampson/scoop)）

首先是安装Scoop的安装
使用命令

```shell
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
```

你可能需要配置Powershell的权限

```shell
Set-ExecutionPolicy RemoteSigned -scope CurrentUser
```

#### 安装Node.js版本管理

在项目中有时候会用到不同版本的 Node.js, 所有我们需要对其版本进行管理。

1. 安装 nvm

   ```shell
   scoop install nvm
   ```

2. 下载对应的node

   ```shell
   nvm install 10.1.0
   ```

3. 使用它

   ```shell
   nvm use 10.1.0
   ```

   











