---
title: 基于 Node.js 的前端代码检查工具-FECS
tags:
  - 前端
originContent: >-
  精选一语： I'm looking forward to the rest of my life every time at the thought of
  spending the rest life with you.



  为了统一团队的代码规范，除了一纸规范说明之外，还需要引入工具进行限制。虽说工具并不能完全实现规范中的规则，但至少能够在一定程度上缓解代码不统一的局面。


  相对于后端，前端代码规范的质量检查涉及到HTML, CSS，Javascript ，如今还涉及到SCSS，ES5，JSX, 
  React，Vue，Angular等，更是复杂。


  ```language

  All code in any code-base should look like a single person typed it, no matter
  how many people contributed. — idiomatic.js


  在任一个代码库中，不管是多少人协同开发，所有的代码都应该看起来像是一个人写的。- idiomatic.js

  ```


  今天要讲的不是代码规范，关于代码规范网上已经有了非常好的实践和各大公司公开的实践指南，大家可自行查询。 例如: [code-guide
  chinese](https://zoomzhao.github.io/code-guide/)


  ## FECS 


  fecs[在线体验](http://fecs.baidu.com/demo)，代码检查/格式化，从未如此简单！


  FECS 是以百度前端代码规范为目标的基于 Node.js 的前端代码风格检测工具，套件内包括了 htmlcs、csshint、lesslint 和
  jformatter 等工具。


  因此，fecs 不仅能检查 HTML/CSS/LESS/JavaScript 代码的规范问题，而且还能修复代码的规范问题。


  需要说明的是，FECS 目前只支持 HTML/CSS/LESS/JavaScript 四种文件和语法的检测。


  ### Javascript

  Javascript 方面 FECS 采用了 eslint 检测引擎，因此你在自定义规则的时候可以参考
  [esling](https://github.com/AlloyTeam/eslint-config-alloy) 的文档进行相关配置。FECS 只是在
  eslint 基础之上针对百度的代码规范作了新的规则实现或调整。详细内容见
  [FECS](https://github.com/ecomfe/fecs/wiki/FECSRules) 自有规则


  这里推荐一个比较齐全的自定义规则的链接：[自定义EsLint校验规程](https://www.cnblogs.com/imwtr/p/9189414.html)


  ### CSS/LESS/HTML

  CSS 的 linter 是使用了内部优化过的 csshint。LESS 和 HTML 方面则分别使用了 lesslint 和 htmlcs。


  ## FECS 的安装及其使用

  ### Install

  ```language

  $ [sudo] npm install fecs -g

  ```

  ### Use

  使用方式可使用以下命令查看:


  ```language

  $ fecs --help

  $ fecs check --help

  $ fecs format --help

  ```

  详情请查看官方文档: [FECS 命令参数](https://github.com/ecomfe/fecs/wiki/CLI)


  ### Tips:

  FECS 的错误报告默认为英文格式，由各 linter 直接提供。FECS 根据百度前端代码规范，作了一次影射转换，通过指定 reporter 为
  baidu 可以看到中文的报告输出效果，对于某些比较抽象的描述，会同时在括号内提供英文原文补充说明。


  例如，你可以这样用:

  ```language

  fecs check --reporter=baidu

  ```


  或者，更直接一点，直接添加一个 alias，使其在执行 fecs check 命令时默认为中文输出。

  ```language

  alias fecs='fecs --reporter=baidu'

  ```
categories:
  - 专栏
toc: true
date: 2019-03-12 22:58:58
---

精选一语： I'm looking forward to the rest of my life every time at the thought of spending the rest life with you.


为了统一团队的代码规范，除了一纸规范说明之外，还需要引入工具进行限制。虽说工具并不能完全实现规范中的规则，但至少能够在一定程度上缓解代码不统一的局面。

相对于后端，前端代码规范的质量检查涉及到HTML, CSS，Javascript ，如今还涉及到SCSS，ES5，JSX,  React，Vue，Angular等，更是复杂。

```language
All code in any code-base should look like a single person typed it, no matter how many people contributed. — idiomatic.js

在任一个代码库中，不管是多少人协同开发，所有的代码都应该看起来像是一个人写的。- idiomatic.js
```

今天要讲的不是代码规范，关于代码规范网上已经有了非常好的实践和各大公司公开的实践指南，大家可自行查询。 例如: [code-guide chinese](https://zoomzhao.github.io/code-guide/)

## FECS 

fecs[在线体验](http://fecs.baidu.com/demo)，代码检查/格式化，从未如此简单！

FECS 是以百度前端代码规范为目标的基于 Node.js 的前端代码风格检测工具，套件内包括了 htmlcs、csshint、lesslint 和 jformatter 等工具。

因此，fecs 不仅能检查 HTML/CSS/LESS/JavaScript 代码的规范问题，而且还能修复代码的规范问题。

需要说明的是，FECS 目前只支持 HTML/CSS/LESS/JavaScript 四种文件和语法的检测。

### Javascript
Javascript 方面 FECS 采用了 eslint 检测引擎，因此你在自定义规则的时候可以参考 [esling](https://github.com/AlloyTeam/eslint-config-alloy) 的文档进行相关配置。FECS 只是在 eslint 基础之上针对百度的代码规范作了新的规则实现或调整。详细内容见 [FECS](https://github.com/ecomfe/fecs/wiki/FECSRules) 自有规则

这里推荐一个比较齐全的自定义规则的链接：[自定义EsLint校验规程](https://www.cnblogs.com/imwtr/p/9189414.html)

### CSS/LESS/HTML
CSS 的 linter 是使用了内部优化过的 csshint。LESS 和 HTML 方面则分别使用了 lesslint 和 htmlcs。

## FECS 的安装及其使用
### Install
```language
$ [sudo] npm install fecs -g
```
### Use
使用方式可使用以下命令查看:

```language
$ fecs --help
$ fecs check --help
$ fecs format --help
```
详情请查看官方文档: [FECS 命令参数](https://github.com/ecomfe/fecs/wiki/CLI)

### Tips:
FECS 的错误报告默认为英文格式，由各 linter 直接提供。FECS 根据百度前端代码规范，作了一次影射转换，通过指定 reporter 为 baidu 可以看到中文的报告输出效果，对于某些比较抽象的描述，会同时在括号内提供英文原文补充说明。

例如，你可以这样用:
```language
fecs check --reporter=baidu
```

或者，更直接一点，直接添加一个 alias，使其在执行 fecs check 命令时默认为中文输出。
```language
alias fecs='fecs --reporter=baidu'
```