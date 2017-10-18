---
layout: default
title: react-native
---

# create-react-native-app
## 概述
> create-react-native-app(React Native 项目) 是业界最优秀的 React 相关应用开发工具之一

> Exponent是一套开发环境，还带有一个已上架的空应用容器。这样你可以在没有原生开发平台（Xcode或是Android Studio）的情况下直接编写React Native应用（当然这样你只能写js部分代码而没法写原生代码）。

## 安装和初始化
- $ npm install -g create-react-native-app   使用npm全局安装create-react-native-app
- $ create-react-native-app antm-demo  初始化react-native 项目
- $ npm start 
- $ npm run eject 转到自定义配置，单向操作

## 使用手机调试
在手机上安装'Expo'app,输入本机ip地址和端口号访问即可访问页面。

## DEMO
[react-native-example](https://github.com/Bubblex/react-native-example)

- dva
- fetch
- antd-mobile

# react-native

## 搭建开发环境
与 create-react-native-app 不同的是需要分别搭建Android和IOS开发环境，Xcode和Android Studio。

具体搭建方法参考官方文档 [搭建开发环境](https://reactnative.cn/docs/0.49/getting-started.html#content)

## 安装
- $ react-native init myapp 初始化react-native项目
- $ react-native start 开启本地服务


## 开发者菜单
> Developer Menu是React Native给开发者定制的一个开发者菜单，来帮助开发者调试React Native应用。

> 模拟器中Ctrl+M 打开。

> 真机测试摇晃手机开启。
### Reload
- Reload js即将你项目中js代码部分重新生成bundle，然后传输给模拟器或手机。
- 安卓模拟器中RR，IOS模拟器中COM+R
### Debug JS Remotely
- JS远程调试功能
### Enable Live Reload
- React Native动态加载功能。当js代码发生变化后，React Native会自动生成bundle然后传输到模拟器或手机上。
### Enable Hot Reloading
- 当你每次保存代码时Hot Reloading功能便会生成此次修改代码的增量包，然后传输到手机或模拟器上以实现热加载。

## DEMO

[official-react-native-demo](https://github.com/Bubblex/official-react-native-demo)

- fetch
- dva-core
- antd-mobile

# 参考文档
- [react-native 文档](https://reactnative.cn/docs/0.49/getting-started.html#content)
- [react-native-dva-starter](https://github.com/nihgwu/react-native-dva-starter)
- [React Navigation
](https://reactnavigation.org/docs/intro/)
- [React Native 调试技巧与心得](https://github.com/crazycodeboy/RNStudyNotes/blob/master/React%20Native%E8%B0%83%E8%AF%95%E6%8A%80%E5%B7%A7%E4%B8%8E%E5%BF%83%E5%BE%97/React%20Native%E8%B0%83%E8%AF%95%E6%8A%80%E5%B7%A7%E4%B8%8E%E5%BF%83%E5%BE%97.md)


