title: RN环境配置
date: 2016-12-22 16:56:43
categories: ReactNative
tags: [环境配置]
---

<!--more-->
# 必需的软件

## Homebrew
Mac系统的包管理器
```shell
ruby -e -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## Node
```shell
brew install node
```

## react-native-cli
```shell
npm install -g react-native-cli
```

# 非必需软件
## Watchman
监视文件系统变更的工具
```shell
brew install watchman
```

## Flow
静态的JS类型检查工具
```shell
brew install flow
```

## 测试
```shell
react-native init FirstProject
cd FirstProject
react-native run-ios
```
