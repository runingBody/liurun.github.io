---
title: Hexo探究之路-快速搭建个人博客系统
tags:
  - Hexo
originContent: >
  ## 每日一言：“昨晚做了一个很奇怪的梦”

  ### 一、技术概括

  #### 1.Hexo [官网](https://hexo.io/zh-cn/)

  *是一个快速、简洁且高效的博客框架。Hexo 使用
  Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。何为Markdown,在我的理解是：框架自定义编写模式，并进行特定解析进而转化成静态的HTML文档结构。*

  *例如：标签元素写法*

  ```

  hexo写法：#这是文章H1标题 

  hexo解析成：<h1>这是文章H1标题</h1>

  ```

  *例如：常用字体样式操作*

  ```

  hexo写法：*文字斜体

  hexo解析成：<i>这是文章标题</i>(已废弃)

  ```

  *例如：代码块*

  ```

  hexo写法：` ` `code代码块被反引号闭合包装 ` ` `

  hexo解析成：解析为这种样式代码块

  ```

  ### 2.技术伙伴

  #### 1.git 

  Hexo 提供了快速方便的一键部署功能，让您只需一条命令就能将网站部署到服务器上.

  ```language

  $ hexo deploy

  ```

  所以前提是安装git(最新版本为佳），并在全局—config.yml文件中配置部署类型和正确的git地址。

  Ps: 如果git版本太低，会在验证git账户密码的时候报错，卸载重新安装最新版本git.


  #### 2.node 

  执行npm
  install安装package.json依赖，建议使用yarn命令安装。因为npm在安装依赖的时候可能会导致版本不确定性问题和安装效率问题，而yarn不存在这些问题。

  #### 3.hexo-client客户端

  这里首先感谢“一个爱折腾的码农”提供的快速实现文章发布、修改、部署的客户端应用 ***Hexo-client***.
  毕竟我们的重点是在于能时刻、方便的维护我们的博客，而不是去练习Markdown写法。

  hexo-client客户端使用到[electron](https://github.com/electron/electron)、[Vue.js](https://cn.vuejs.org/)、[electron-vue](https://github.com/SimulatedGREG/electron-vue)、[element-ui](http://element-cn.eleme.io/#/zh-CN)技术编写成的GUI桌面应用。有兴趣的可以研究一下。

  客户端下载地址：[click here](https://github.com/gaoyoubo/hexo-client)


  ### 一、Hexo建站及常用命令

  ### 1.建站

  安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

  ```language

  $ hexo init <folder>

  $ cd <folder>

  $ yarn

  ```

  新建完成后，生成目录及文件夹如下：

  ```language

  .

  ├── _config.yml

  ├── package.json

  ├── scaffolds

  ├── source

  |   ├── _drafts

  |   └── _posts

  └── themes


  ```

  **_config.yml**

  全局配置文件，配置网站基本信息，包括网址、标题、Mate、关键词、部署、主题等。

  **scaffolds**

  Hexo的模板是指在新建的markdown文件中默认填充的内容。

  **source**

  资源文件夹是存放用户资源的地方。除 _posts 文件夹之外（post文件夹存放的是发布的文章）

  **themes**

  Hexo 会根据主题来生成静态页面。


  ### 2. 文件部署和域名绑定

  - 第一步：注册域名，例如：[www.liurun.work](www.liurun.work)，推荐使用阿里云，便宜又好用。（首购只需1元/年）

  -
  第二部：在项目srouces目录下新建CNAME文件（不要带后缀名），写入注册好的域名，直接写[www.liurun.work](www.liurun.work)就好

  - 第三部：在项目目录下执行

  ```language

  $ hexo g 

  $ hexo d

  ```

  Ps: 如果访问域名无效，请确保是否存在一下问题

  1.git尽量安装最新版本，不然在deploy的时候会报验证错误。

  2.全局——config.yml配置文件中git地址配置正确。

  3.检查github确保文件已经push到git仓库（仓库命名规范：liurun.git.io)

  4.域名成功解析，且已成功进行身法验证。
categories:
  - 前端
toc: true
date: 2018-12-28 14:34:54
---

## 每日一言：“昨晚做了一个很奇怪的梦”
### 一、技术概括
#### 1.Hexo [官网](https://hexo.io/zh-cn/)
*是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。何为Markdown,在我的理解是：框架自定义编写模式，并进行特定解析进而转化成静态的HTML文档结构。*
*例如：标签元素写法*
```
hexo写法：#这是文章H1标题 
hexo解析成：<h1>这是文章H1标题</h1>
```
*例如：常用字体样式操作*
```
hexo写法：*文字斜体
hexo解析成：<i>这是文章标题</i>(已废弃)
```
*例如：代码块*
```
hexo写法：` ` `code代码块被反引号闭合包装 ` ` `
hexo解析成：解析为这种样式代码块
```
### 2.技术伙伴
#### 1.git 
Hexo 提供了快速方便的一键部署功能，让您只需一条命令就能将网站部署到服务器上.
```language
$ hexo deploy
```
所以前提是安装git(最新版本为佳），并在全局—config.yml文件中配置部署类型和正确的git地址。
Ps: 如果git版本太低，会在验证git账户密码的时候报错，卸载重新安装最新版本git.

#### 2.node 
执行npm install安装package.json依赖，建议使用yarn命令安装。因为npm在安装依赖的时候可能会导致版本不确定性问题和安装效率问题，而yarn不存在这些问题。
#### 3.hexo-client客户端
这里首先感谢“一个爱折腾的码农”提供的快速实现文章发布、修改、部署的客户端应用 ***Hexo-client***. 毕竟我们的重点是在于能时刻、方便的维护我们的博客，而不是去练习Markdown写法。
hexo-client客户端使用到[electron](https://github.com/electron/electron)、[Vue.js](https://cn.vuejs.org/)、[electron-vue](https://github.com/SimulatedGREG/electron-vue)、[element-ui](http://element-cn.eleme.io/#/zh-CN)技术编写成的GUI桌面应用。有兴趣的可以研究一下。
客户端下载地址：[click here](https://github.com/gaoyoubo/hexo-client)

### 一、Hexo建站及常用命令
### 1.建站
安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。
```language
$ hexo init <folder>
$ cd <folder>
$ yarn
```
新建完成后，生成目录及文件夹如下：
```language
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes

```
**_config.yml**
全局配置文件，配置网站基本信息，包括网址、标题、Mate、关键词、部署、主题等。
**scaffolds**
Hexo的模板是指在新建的markdown文件中默认填充的内容。
**source**
资源文件夹是存放用户资源的地方。除 _posts 文件夹之外（post文件夹存放的是发布的文章）
**themes**
Hexo 会根据主题来生成静态页面。

### 2. 文件部署和域名绑定
- 第一步：注册域名，例如：[www.liurun.work](www.liurun.work)，推荐使用阿里云，便宜又好用。（首购只需1元/年）
- 第二部：在项目srouces目录下新建CNAME文件（不要带后缀名），写入注册好的域名，直接写[www.liurun.work](www.liurun.work)就好
- 第三部：在项目目录下执行
```language
$ hexo g 
$ hexo d
```
Ps: 如果访问域名无效，请确保是否存在一下问题
1.git尽量安装最新版本，不然在deploy的时候会报验证错误。
2.全局——config.yml配置文件中git地址配置正确。
3.检查github确保文件已经push到git仓库（仓库命名规范：liurun.github.io)
4.域名成功解析，且已成功进行身法验证。
