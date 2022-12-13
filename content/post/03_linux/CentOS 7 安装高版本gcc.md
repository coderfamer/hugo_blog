# CentOS 7 基于 devtoolset 安装 gcc

## 安装教程

### 1.   系统安装  repository

```shell
yum install cetnos-release-rh
```

### 2.  安装 development-tools 和 gcc

```shell
yum install devtoolset-8-gcc*
```

### 3. 使用

```shell
scl enable devtoolset-8 bash
gcc --version
```





