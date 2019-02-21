---
title: React- Native 小小鸟觅食之路
tags:
  - 前端
  - react
originContent: >-
  Welcome to my personal blog! I hope that what I have written can help you.
  Summarize the experience of the predecessors and the summary of their own
  projects, the technology is not guilty, and share the sentiment.


  ## 与其败笔，宁愿一死。历经苦难而不厌，此乃阿修罗之道。


  ### 一. 技术要点


  ``` bash

  1.create-react-app


  2.create-react-native-app

  ```


  ### 二.技术概要（技术比较、简单使用规范、示例）


  #### 1.yarn


  ``` bash

  compare:
  相比与npm，yarn（package.josn）在下载项目依赖时更优的版本确定性的和它的并行加载方式让下载速度有的质的提升。更神奇的是，它可以提供离线本地缓存的方

  式来引入包。


  command:


  yarn / npm install


  yarn add global <package> / npm install -g <package>


  yarn remove <package> / npm uninstall <package>


  yarn add <package> / npm i <package> --save


  yarn add <package> --dev / npm i <package> --save-dev


  ```

  #### 2.npx


  ``` bash


  ```


  ### 三.开发流程


  1.国内外技术交流，建议使用翻墙软件查看国外优秀的技术文章（鉴于国内相关技术文档太过陈旧==。）


  2.推荐相关优秀的技术连接（如遇链接无法打开的情况，请先翻墙，谢谢配合）


  https://reactjs.org/tutorial/tutorial.html


  https://github.com/facebook/create-react-app#create-react-app-


  ### 四.相关附件：

  1.鉴于一些小伙伴的翻墙之痛，我已分享一些翻墙软件（psiphon.exe），双击运行应用以后，就会自动配置代理服务，一键翻墙。


  psiphon.exe


  链接：https://pan.baidu.com/s/1u-h7iInqLWFbDVyRIkc1Cg 密码：3dym


  2.开发工具包


  yarn


  链接：https://pan.baidu.com/s/1r1MqRuFUqUgwDc6_X5lsgw 密码：7t6p


  ### 五.修行之路


  一.正式进入开发（踩坑阶段）...


  按照[https://github.com/facebook/create-react-app#create-react-app-](https://github.com/facebook/create-react-app#create-react-app-)的官方示例，不禁感叹一声：


  ”卧槽 ，原来这么简单，什么webpack、loader、plug和config 都去见鬼吧。“


  其实按照文章说明：


  ```bash

  ”You don’t need to install or configure tools like Webpack or Babel.


  They are preconfigured and hidden so that you can focus on the code.


  Just create a project, and you’re good to go.“

  ```


  至此，我们不在需要配置繁杂的配置文件，just focus on code! 多么幸福发事儿啊！


  step1: 安装好 yarn ,


  (什么？这是什么东西！来，请看这里http://geek.csdn.net/news/detail/197339 ）


  step2: 运行yarn add global npx


  (什么？这是什么东西！来，请看如上链接，或自行google ）


  step3: 运行npx create-react-app myApp,


  见证奇迹的时刻到了:


  下图是我的运行结果：(开心...嘤嘤嘤）









  在之后，我用编辑器，尝试跟新内部代码，热更新正常！









  至此，示例演示完成，各位看官也可以自己去试试啦！


  少公仔也要去继续参研Tutorial: Intro To React.



  文章来自本人知乎: 附属链接[Click
  here](https://zhuanlan.zhihu.com/p/35602360?utm_source=wechat_session&utm_medium=social&from=timeline)
categories:
  - react-native
toc: true
date: 2018-12-27 14:27:56
---

### 每日一言：与其败笔，宁愿一死。历经苦难而不厌，此乃阿修罗之道。

### 一. 技术要点

``` bash
1.create-react-app

2.create-react-native-app
```

### 二.技术概要（技术比较、简单使用规范、示例）

#### 1.yarn

``` bash
compare: 相比与npm，yarn（package.josn）在下载项目依赖时更优的版本确定性的和它的并行加载方式让下载速度有的质的提升。更神奇的是，它可以提供离线本地缓存的方
式来引入包。

command:

yarn / npm install

yarn add global <package> / npm install -g <package>

yarn remove <package> / npm uninstall <package>

yarn add <package> / npm i <package> --save

yarn add <package> --dev / npm i <package> --save-dev

```
#### 2.npx

``` bash

```

### 三.开发流程

1.国内外技术交流，建议使用翻墙软件查看国外优秀的技术文章（鉴于国内相关技术文档太过陈旧==。）

2.推荐相关优秀的技术连接（如遇链接无法打开的情况，请先翻墙，谢谢配合）

https://reactjs.org/tutorial/tutorial.html

https://github.com/facebook/create-react-app#create-react-app-

### 四.相关附件：
1.鉴于一些小伙伴的翻墙之痛，我已分享一些翻墙软件（psiphon.exe），双击运行应用以后，就会自动配置代理服务，一键翻墙。

psiphon.exe

链接：https://pan.baidu.com/s/1u-h7iInqLWFbDVyRIkc1Cg 密码：3dym

2.开发工具包

yarn

链接：https://pan.baidu.com/s/1r1MqRuFUqUgwDc6_X5lsgw 密码：7t6p

### 五.修行之路

一.正式进入开发（踩坑阶段）...

按照[https://github.com/facebook/create-react-app#create-react-app-](https://github.com/facebook/create-react-app#create-react-app-)的官方示例，不禁感叹一声：

”卧槽 ，原来这么简单，什么webpack、loader、plug和config 都去见鬼吧。“

其实按照文章说明：

```bash
”You don’t need to install or configure tools like Webpack or Babel.

They are preconfigured and hidden so that you can focus on the code.

Just create a project, and you’re good to go.“
```

至此，我们不在需要配置繁杂的配置文件，just focus on code! 多么幸福发事儿啊！

step1: 安装好 yarn ,

(什么？这是什么东西！来，请看这里http://geek.csdn.net/news/detail/197339 ）

step2: 运行yarn add global npx

(什么？这是什么东西！来，请看如上链接，或自行google ）

step3: 运行npx create-react-app myApp,

见证奇迹的时刻到了:

下图是我的运行结果：(开心...嘤嘤嘤）








在之后，我用编辑器，尝试跟新内部代码，热更新正常！








至此，示例演示完成，各位看官也可以自己去试试啦！

少公仔也要去继续参研Tutorial: Intro To React.


文章来自本人知乎: 附属链接[Click here](https://zhuanlan.zhihu.com/p/35602360?utm_source=wechat_session&utm_medium=social&from=timeline)