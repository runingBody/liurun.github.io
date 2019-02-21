---
title: JS/CSS体积减少了67%，我们是如何做到的？
tags:
  - react
  - Webpack
  - Javascript
originContent: ''
categories:
  - 前端
toc: false
date: 2019-01-22 11:21:44
---

Fider（Fider 是一款开源的产品反馈搜集平台）团队一直致力于优化应用程序的体验。作为一个使用 React 构建的 web 应用程序，他们主要关注如何减少 JS 和 CSS 代码的体积。在这篇文章中，Fider 团队分享了他们的学习体会。通过掌握这些概念和建议，你也可以针对自己的 Web 应用做出类似的优化。

Fider 的前端由 React 和 Webpack 构建，所以下面的主题非常适用于使用相同技术栈的前端团队，但是这些概念也可以应用于其他技术栈。Fider 本身是一个开源项目，所以你实际上可以在项目地址上提交 PR 或者查看源码。

### 1. Bundle 可视化分析器

webpack-bundle-analyzer 是一个 Webpack 插件，它可以将你所使用的 Bundle 生成为一个可交互、可缩放 Treemap。通过这个插件，你可以看到 Bundle 中哪个 Module 占用体积最大。

如果你不知道产生问题的根本原因，你怎么能解决它呢?

webpack-bundle-analyzer 检查结果配图

###  2. 长期缓存
Long term caching 是指告知浏览器长时间缓存一个文件，比如 3 个月甚至 1 年。它之所以非常重要，是因为这项特性可以确保再次返回同一页面浏览的用户，不需要重复下载相同的 JS/CSS 文件。

浏览器是根据文件的完整路径名来缓存文件的。因此，假如你需要强制用户下载新版本的 Bundle，首先必须重命名这个 Bundle。幸运的是，Webpack 提供了一个功能，可以根据一定规则来更新 Bundle 的名称，从而解决这个问题。

长久以来，Fider 团队一直把 Chunkhash 作为 Webpack 编译输出的配置。但是在 99% 的需要 Long Term Caching 的场景中，最好的选择 Contenthash。它将根据 JS/CSS 的内容生成散列，因此只有内容发生改变，Bundle 的命名才会变化，从而尽可能的利用 Long Term Caching 的优势。

尽管这种技术不会减少 Bundle 的大小，但它确实有助于减少用户下载 Bundle 的时间。如果某个 Bundle 没有更改，用户就无需再次下载它。

更多相关信息可以从 Webpack 官方文档中获取：
[https://webpack.js.org/guides/caching/]()

### 3. 公共 Bundle

长久以来，许多团队在实践中，都会将所有的 NPM Package 构建成一个单独的 Bundle。这与 Long Term Caching 相结合时非常有用。

相比我们应用自身的代码，NPM Package 的改动往往要小得多。因此我们可以把这些包单独打包成一个 Bundle，从而避免用户重复下载这些 NPM Package。这些由 Npm Package 构建成的 Bundle 通常称为 Vendor Bundle。

但是我们可以做的更好。如果你自己也有一部分很少更改的代码呢？也许你已经有了一些在一段时间之前创建，并且迄今为止很少修改的组件，如 Button，Grid，Toggle 等，那么这些组件就可以作为 Common Bundle 的候选。

你可以看看 PR #636：

[https://github.com/getfider/fider/pull/636]()

在这个 PR 中，我们就是把一些特定文件夹中的 Module 单独构建成了一个 Common Bundle。这将确保，除非我们更改这些公共组件，否则用户就不需要重新下载它们。

### 4. 路由级别代码分割

Code Splitting 是当前的热门话题。代码分割在过去不是一件简单的事，但是随着工具和框架的长足发展，现在进行 Code Splitting 相对而言简单得多。

一种常见的情况是，即使用户只是需要查看主页，应用程序也会推送一个包含所有 JS/CSS 的 Bundle。这些 JS/CSS 可以用来渲染应用程序内的所有页面。因为我们已经推送了所有的 JS/CSS 代码，所以不用去关心用户是否会去访问站点内的其他页面，比如设置页面。Fider 这么做已经很久了，但是现在我们已经做出了改变。

Code Splitting 的思想是生成一个 MainBundle 以及多个更小的 Bundle（通常一个路由对应一个）。我们唯一分发给所有用户的是 Main Bundle，然后由 Main Bundle 异步下载当前页面渲染所需的 Bundle。

这看起来很复杂，但是多亏了 React 和 Webpack 这对组合，Code Splitting 已经不是什么难事了。对于 React <= 16.5 的用户，我们推荐 react-loadable（https://github.com/jamiebuilds/react-loadable） 来做 Code Splitting。如果你已经在使用 React 16.6，那么你可以使用 response .lazy()，它是这个版本新增的特性。

### 5. 按需载入外部依赖

通过使用 Webpack Bundle Analyzer，我们注意到，在 Vendor Bundle 中引入了 response-toastify。response-toastify 是我们用于弹出消息提示的 toaster 库。在 Fider 中，我们很少弹出消息，那么把 response-toastify 直接引入 Bundle 就不是非常合理。假如用户并不需要显示弹出消息，那为什么要向他们推送这 30kB 的 JavaScript 代码呢?

这个问题和第四节的问题很相似，但是这里我们讨论不再是路由，这是在多个路由中使用的某个特性。你能在 feature 级别上进行 Code Splitting 吗？

是的，完全可以!

简而言之，你需要做的就是从静态导入切换到动态导入。

```language
// before
import { toast } from "./toastify";
toast("Hello World");

// after
import("./toastify").then(module => {
  module.toast("Hello World");
});
```
这样，Webpack 会将 toastify 模块及其 NPM 依赖项单独打包。之后浏览器就会在需要 toast 的时候下载对应的 Bundle。如果您已经配置了 Long term caching，那么在第二个 toaster 调用时就无需再次下载。下面这个动图就显示了这个过程。

你可以去 PR #645 上了解更多关于此功能的实现细节：
[https://github.com/getfider/fider/pull/645]()

### 6.Font Awesome 和“摇树”

Tree Shaking 是只指从 Module 中导入你需要的东西，然后丢弃其余部分的过程。Webpack 在生产模式下时默认启用这个功能。

通常使用 Font Awesome 的方法是导入一个外部字体文件和一个 CSS，然后将字体上的每个字符或者图标映射形成一个 CSS 类。带来的结果是，即使我们只是使用图标 A、B 和 C，用户也要下载这个外部字体和一个包含 600 多个图标定义的 CSS。

幸运的是，我们找到了 response-icons，这是一个 NPM Pakage，包含了 SVG 格式的 Font Awsome 和其他图标包！通过它，我们可以将这些图标导出成一个 ES Module 格式的 React 组件。

然后你可以只导入你需要的图标，Webpack 会从 Bundle 中删除其他图标。结果呢？不仅我们的 CSS 文件小了近 68kB，同时我们也不需要下载外部字体了。这是 Fider 团队在减少 CSS 体积上的最大贡献。

可以在这个 PR 中看到更多实现细节 PR #631：
[https://github.com/getfider/fider/pull/631]()

### 7. 使用更小的 NPM 包

*“NPM 就像一个乐高商店，里面装满了积木，你可以随意挑选你喜欢的。你不需要为安装 Package 付费。但是这些 Package 会占用应用程序的字节大小，你的用户会为此买单。所以请做出明智的选择。”*

在使用 Bundle Analyzer 时，我们发现，单单 markdown-it 就占用了 Vendor Bundle 体积的 40%。于是我们就想在 NPM 上寻找一个可替代的 Markdown 解析器，它必须更小、更好维护且能满足我们的需要。

在安装任意 NPM Package 之前，我们使用 bundlephobia.com 来分析它的大小。我们只需要修改少量 API，就能从 markdown-it 切换到 markit。这最终帮助 Fider 减少了大约 63KB 的 Vendor Bundle 的体积。

可以在这个 PR 中看到更多实现细节 PR #643：
[https://github.com/getfider/fider/pull/643]()

在添加大体积的 Package 之前，请三思。你真的需要它吗？您的团队有更简单的实现吗？如果没有，你能否找到另一个 Package，能用更少的代码完成相同的工作？ 如果还是不行，你可以像第 5 节使用 react-toastify 一样，首先添加这个 NPM Package，然后根据需要进行异步加载。

### 8. Main Bundle 优化至关重要

假如你用路由级别的 Code Splitting 来创建一个应用，并且已经部署在生产环境上供用户使用了。假如现在你对 Dashboard 路由组件做出了更改，那么 Webpack 只会重新生成 Dashboard 路由对应的那一份 Bundle？是这样吗？

事实上并非如此。

如果应用程序代码发生某些改变，Webpack 总是会重新生成 Main Bundle。原因是 Main Bundle 是指向其他 Bundle 的指针。如果某个 Bundle 的散列值发生更改，那么 Main Bundle 也必须发生改变，以指向拥有新散列值的 Dashboard Bundle。

因此，如果您的 Main Bundle 不仅包含这些指针，还包含许多常见组件，如按钮、切换、网格和选项卡。那么由于 Main Bundle 发生了变化，就会导致用户重复下载一些完全没有更改的东西。

因此，你首先需要使用 Webpack Bundle Analyzer 来看看你的 Main Bundle 中到底有些什么。然后再应用我们前面提到的一些技术来减少 Main Bundle 的体积。因为 Main Bundle 的优化至关重要。

### 9. TSLib（只针对 TypeScript）

将 TypeScript 代码编译成 ES5 时，TypeScript 编译器会向最终输出的 JavaScript 文件添加一些辅助函数。这个过程确保，即使你的 TypeScript 中使用了某些旧浏览器不支持类（Classes ） 和生成器（Generators）等 ES6 特性，最后生成的代码也可与之相兼容。

在这种情况下，一旦在某个 TypeScript 文件中使用非 ES5 特性的语法，那么最后生成的文件就会包含这些 Helper 函数。虽然单独来看，这些 Helper 函数体积非常小，但是假如这些文件数量很多，那么这个空间消耗就不能被忽略了。由于 Webpack 无法对这些函数进行 Tree Shaking，因此最终会生成一个略大的、包含多份相同的代码的 Bundle。

幸运的是，一个名为 tslib 的 NPM Package 可以解决这个问题，它包含了所有 TypeScript 所需的 Helper 函数。在 npm install tslib -save 安装之后，我们通过在 tsconfig.json 上设置 importHelpers: true，来让编译器从 tslib 包导入 Helper 函数，而不是重复添加。

对于一个 React 应用程序来说，如果大部分组件是由类构成，那么体积缩减就会非常明显。

### 小 结 
你准备好迎接后十亿用户（https://developers.google.com/web/billions/ ）了吗？考虑一下你的应用程序的所有潜在用户，可能他们目前正试图用低成本设备、较慢的网络访问它。减少 Bundle 的大小将直接影响我们应用程序的性能，并让用户更好地访问它。

 原文链接
[https://dev.to/goenning/how-we-reduced-our-initial-jscss-size-by-67-3ac0]()







