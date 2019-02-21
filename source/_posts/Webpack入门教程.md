---
title: Webpack入门教程
tags:
  - Webpack
originContent: >+
  ### 配合一个完整的Demo快速入门、学习Webpack

  ### 快速了解的几个基本概念

  ### mode 开发模式

  webpack 提供 mode 配置选项，配置 webpack 相应模式的内置优化。

  ```language

  // webpack.production.config.js

  module.exports = {

  +  mode: 'production',

  }

  ```

  ### 入口文件(entry)

  入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，webpack
  会找出有哪些模块和库是入口起点（直接和间接）依赖的。


  可以在 webpack 的配置文件中配置入口，配置节点为： entry,当然可以配置一个入口，也可以配置多个。


  ### 输出(output)

  output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件。

  ```language

  const path = require('path');


  module.exports = {
    entry: './path/to/my/entry/file.js',
    output: {
      path: path.resolve(__dirname, 'dist'),
      filename: 'my-first-webpack.bundle.js'
    }
  };

  ```

  ### 加载器Loader

  loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader
  可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。


  ### 插件(plugins)

  loader
  被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。


  ### 本地安装 webpack

  ```language

  $ npm install --save-dev webpack


  # 如果你使用 webpack 4+ 版本，你还需要安装 CLI。

  npm install --save-dev webpack-cli

  ```

  注意：不推荐全局安装 webpack。这会将你项目中的 webpack 锁定到指定版本，并且在使用不同的 webpack 版本的项目中，可能会导致构建失败。


  安装完成后，可以添加npm的script脚本

  ```language

  // package.json

  "scripts": {
      "start": "webpack --config webpack.config.js"
  }

  ```

  ### 一、Demo配解

  #### 1. 初始化项目

  ```language

  mkdir webpack-demo && cd webpack-demo

  npm init -y

  npm install webpack webpack-cli --save-dev

  ```

  项目结构

  ```language
    webpack-demo
  + |- package.json

  + |- /dist

  +   |- index.html

  + |- /src

  +   |- index.js

  ```

  #### 2.安装 loadash 依赖和编写 js 文件

  ```

  npm install --save lodash

  ```

  webpack.config.js 配置文件

  ```language

  const path = require('path');


  module.exports = {
    mode: 'development',
    entry: './src/index.js',
    output: {
      filename: 'main.js',
      path: path.resolve(__dirname, './dist')
    }
  };

  ```

  #### 3. 加载非Js文件

  #### 1> 加载Css文件

  #####  安装 css 和 style 模块解析的依赖 style-loader 和 css-loader

  ```language

  npm install --save-dev style-loader css-loader

  ```

  #####  添加 css 解析的 loader

  ```language

  const path = require('path');


  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.resolve(__dirname, 'dist')
    },
    module: {
      rules: [
        {
          test: /\.css$/,
          use: ['style-loader', 'css-loader']
        }
      ]
    }
  };

  ```

  *PS:

  css-loader： 辅助解析 js 中的 import './main.css'

  style-loader: 把 js 中引入的 css 内容 注入到 html 标签中，并添加 style 标签.依赖 css-loader*


  **你可以在依赖于此样式的 js 文件中 导入样式文件，比如：import './style.css'。现在，当该 js 模块运行时，含有 CSS 字符串的
  style 标签，将被插入到 html 文件的 <head>中。**


  ### 二、Module配置补充

  模块(module): 这些选项决定了如何处理项目中的不同类型的模块。

  webpack 模块可以支持如下:

  - ES2015 import 语句

  - CommonJS require() 语句

  - AMD define 和 require 语句

  - css/sass/less 文件中的 @import 语句。

  - 样式(> url(...))或 HTML 文件(> <img src=...>)中的图片链接(image url)

  #### module.noParse

  防止 webpack 解析那些任何与给定正则表达式相匹配的文件。忽略的文件中不应该含有 import, require, define
  的调用，或任何其他导入机制。忽略大型的 library 可以提高构建性能。

  ```language

  module.exports = {
    mode: 'devleopment',
    entry: './src/index.js',
    ...
    module: {
      noParse: /jquery|lodash/,
      // 从 webpack 3.0.0 开始,可以使用函数，如下所示
      // noParse: function(content) {
      //   return /jquery|lodash/.test(content);
      // }
    }
    ...
  };

  ```

  #### module.rules

  创建模块时，匹配请求的规则数组。这些规则能够修改模块的创建方式。这些规则能够对模块(module)应用 loader，或者修改解析器(parser)。

  ```language

  module.exports = {
    ...
    module: {
      noParse: /jquery|lodash/,
      rules: [
        {
          test: /\.css$/,
          use: ['style-loader', 'css-loader']
        }
      ]
    }
    ...
  };

  ```

  #### module Rule条件详解

  - 字符串：匹配输入必须以提供的字符串开始。是的。目录绝对路径或文件绝对路径。

  - 正则表达式：test 输入值。

  - 函数：调用输入的函数，必须返回一个真值(truthy value)以匹配。

  - 条件数组：至少一个匹配条件。

  - 对象：匹配所有属性。每个属性都有一个定义行为。

  #### Rule.test

  { test: Condition }：匹配特定条件。一般是提供一个正则表达式或正则表达式的数组，但这不是强制的。

  ```language

  module.exports = {
    ...
    module: {
      rules: [
        {
          test: /\.css$/,
          use: ['style-loader', 'css-loader']
        }
      ]
    }
    ...
  };

  ```

  其它条件，如：

  - { include: Condition }:匹配特定条件。一般是提供一个字符串或者字符串数组，但这不是强制的。

  - { exclude: Condition }:排除特定条件。一般是提供一个字符串或字符串数组，但这不是强制的。

  - { and: [Condition] }:必须匹配数组中的所有条件

  - { or: [Condition] }:匹配数组中任何一个条件

  - { not: [Condition] }:必须排除这个条件

  ```language

  module.exports = {
    ...
    module: {
      rules: [
        {
          test: /\.css$/,
          include: [
            path.resolve(__dirname, "app/styles"),
            path.resolve(__dirname, "vendor/styles")
          ],
          use: ['style-loader', 'css-loader']
        }
      ]
    }
    ...
  };

  ```

  ### Rule.use

  应用于模块指定使用一个 loader。

  **PS:加载器可以链式传递，从右向左进行应用到模块上。**

  ```language

  use: [
    'style-loader',
    {
      loader: 'css-loader'
    },
    {
      loader: 'less-loader',
      options: {
        noIeCompat: true
      }
    }
  ];

  ```

  *传递字符串（如：use: [ "style-loader" ]）是 loader 属性的简写方式（如：use: [ { loader:
  "style-loader "} ]）。*


  ### 加载 Sass 文件

  ```

  安装

  npm install sass-loader node-sass webpack --save-dev


  ```

  ```language

  // webpack.config.js

  module.exports = {
    ...
    module: {
      rules: [{
        test: /\.scss$/,
        use: [{
          loader: "style-loader"
        }, {
          loader: "css-loader"
        }, {
          loader: "sass-loader"
        }]
      }]
    }
  };

  ```

  ### 创建 Source Map


  css-loader和sass-loader都可以通过该 options 设置启用 sourcemap。


  ```language

  // webpack.config.js

  module.exports = {
    ...
    module: {
      rules: [{
        test: /\.scss$/,
        use: [{
          loader: "style-loader"
        }, {
          loader: "css-loader",
          options: {
            sourceMap: true
          }
        }, {
          loader: "sass-loader",
          options: {
            sourceMap: true
          }
        }]
      }]
    }
  };

  ```

  ### PostCSS 处理 loader（附带：添加 css3 前缀）

  PostCSS是一个 CSS 的预处理工具，可以帮助我们：给 CSS3 的属性添加前缀，样式格式校验（stylelint），提前使用 css
  的新特性比如：表格布局，更重要的是可以实现 CSS 的模块化，防止 CSS 样式冲突。（配合prefixer使用)


  ```language

  const path = require('path');

  const MiniCssExtractPlugin = require('mini-css-extract-plugin');

  module.exports = {
    mode: 'development',
    entry: './src/index.js',
    output: {
      filename: 'main.js',
      path: path.resolve(__dirname, './dist')
    },
    module: {
      rules: [
        {
          test: /\.(sa|sc|c)ss$/,
          use: [
            'style-loader',
            {
              loader: 'css-loader',
              options: {
                sourceMap: true
              }
            },
            {
              loader: 'postcss-loader',
              options: {
                ident: 'postcss',
                sourceMap: true,
                plugins: loader => [
                  require('autoprefixer')({ browsers: ['> 0.15% in CN'] }) // 添加前缀
                ]
              }
            },
            {
              loader: 'sass-loader',
              options: {
                sourceMap: true
              }
            }
          ]
        }
      ]
    }
  };

  ```

  ### 样式表抽离成专门的单独文件并且设置版本号


  Ps:*抽取了样式，就不能再用 style-loader注入到 html 中了。*


  ```language

  npm install --save-dev mini-css-extract-plugin


  const path = require('path');

  const MiniCssExtractPlugin = require('mini-css-extract-plugin');

  const devMode = process.env.NODE_ENV !== 'production'; // 判断当前环境是开发环境还是
  部署环境，主要是 mode属性的设置值。


  module.exports = {
    mode: 'development',
    entry: './src/index.js',
    output: {
      filename: 'main.js',
      path: path.resolve(__dirname, './dist')
    },
    module: {
      rules: [
        {
          test: /\.(sa|sc|c)ss$/,
          use: [
            MiniCssExtractPlugin.loader,
            'css-loader',
            'postcss-loader',
            'sass-loader'
          ]
        }
      ]
    },
    plugins: [
      new MiniCssExtractPlugin({
        filename: devMode ? '[name].css' : '[name].[hash].css', // 设置最终输出的文件名
        chunkFilename: devMode ? '[id].css' : '[id].[hash].css'
      })
    ]
  };

  ```

  再次运行打包：


  在 dist 目录中已经把 css 抽取到单独的一个 css 文件中了。修改 html，引入此 css 就能看到结果了。


  ### 开发相关辅助


  ### 合并两个webpack的js配置文件

  开发环境(development)和生产环境(production)配置文件有很多不同点，但是也有一部分是相同的配置内容，如果在两个配置文件中都添加相同的配置节点，
  就非常不爽。


  webpack-merge 的工具可以实现两个配置文件进合并，这样我们就可以把 开发环境和生产环境的公共配置抽取到一个公共的配置文件中。

  ```language

  npm install --save-dev webpack-merge

  ```

  project

  ```language
    webpack-demo
    |- package.json
  - |- webpack.config.js

  + |- webpack.common.js

  + |- webpack.dev.js

  + |- webpack.prod.js
    |- /dist
    |- /src
      |- index.js
      |- math.js
    |- /node_modules
  ```

  webpack.common.js

  ```language

  + const path = require('path');

  + const CleanWebpackPlugin = require('clean-webpack-plugin');

  + const HtmlWebpackPlugin = require('html-webpack-plugin');

  +

  + module.exports = {

  +   entry: {

  +     app: './src/index.js'

  +   },

  +   plugins: [

  +     new CleanWebpackPlugin(['dist']),

  +     new HtmlWebpackPlugin({

  +       title: 'Production'

  +     })

  +   ],

  +   output: {

  +     filename: '[name].bundle.js',

  +     path: path.resolve(__dirname, 'dist')

  +   }

  + };

  ```

  webpack.dev.js

  ```language

  + const merge = require('webpack-merge');

  + const common = require('./webpack.common.js');

  +

  + module.exports = merge(common, {

  +   devtool: 'inline-source-map',

  +   devServer: {

  +     contentBase: './dist'

  +   }

  + });

  ```

  webpack.prod.js

  ```language

  + const merge = require('webpack-merge');

  + const UglifyJSPlugin = require('uglifyjs-webpack-plugin');

  + const common = require('./webpack.common.js');

  +

  + module.exports = merge(common, {

  +   plugins: [

  +     new UglifyJSPlugin()

  +   ]

  + });

  ```

  ## 相关的loader列表

  ### 文件

  raw-loader 加载文件原始内容（utf-8）

  val-loader 将代码作为模块执行，并将 exports 转为 JS 代码

  url-loader 像 file loader 一样工作，但如果文件小于限制，可以返回 data URL

  file-loader 将文件发送到输出文件夹，并返回（相对）URL

  ### JSON

  json-loader 加载 JSON 文件（默认包含）

  json5-loader 加载和转译 JSON 5 文件

  cson-loader 加载和转译 CSON 文件

  ### 转换编译(Transpiling)

  script-loader 在全局上下文中执行一次 JavaScript 文件（如在 script 标签），不需要解析

  babel-loader 加载 ES2015+ 代码，然后使用 Babel 转译为 ES5

  buble-loader 使用 Bublé 加载 ES2015+ 代码，并且将代码转译为 ES5

  traceur-loader 加载 ES2015+ 代码，然后使用 Traceur 转译为 ES5

  ts-loader 或 awesome-typescript-loader 像 JavaScript 一样加载 TypeScript 2.0+

  coffee-loader 像 JavaScript 一样加载 CoffeeScript

  ### 模板(Templating)

  html-loader 导出 HTML 为字符串，需要引用静态资源

  pug-loader 加载 Pug 模板并返回一个函数

  jade-loader 加载 Jade 模板并返回一个函数

  markdown-loader 将 Markdown 转译为 HTML

  react-markdown-loader 使用 markdown-parse parser(解析器) 将 Markdown 编译为 React 组件

  posthtml-loader 使用 PostHTML 加载并转换 HTML 文件

  handlebars-loader 将 Handlebars 转移为 HTML

  markup-inline-loader 将内联的 SVG/MathML 文件转换为 HTML。在应用于图标字体，或将 CSS 动画应用于 SVG
  时非常有用。

  ### 样式

  style-loader 将模块的导出作为样式添加到 DOM 中

  css-loader 解析 CSS 文件后，使用 import 加载，并且返回 CSS 代码

  less-loader 加载和转译 LESS 文件

  sass-loader 加载和转译 SASS/SCSS 文件

  postcss-loader 使用 PostCSS 加载和转译 CSS/SSS 文件

  stylus-loader 加载和转译 Stylus 文件

  ### 清理和测试(Linting && Testing)

  mocha-loader 使用 mocha 测试（浏览器/NodeJS）

  eslint-loader PreLoader，使用 ESLint 清理代码

  jshint-loader PreLoader，使用 JSHint 清理代码

  jscs-loader PreLoader，使用 JSCS 检查代码样式

  coverjs-loader PreLoader，使用 CoverJS 确定测试覆盖率

  ### 框架(Frameworks)

  vue-loader 加载和转译 Vue 组件

  polymer-loader 使用选择预处理器(preprocessor)处理，并且 require() 类似一等模块(first-class)的 Web
  组件

  angular2-template-loader 加载和转译 Angular 组件

  Awesome 更多第三方 loader，查看 awesome-webpack 列表。

  ### 打包分析优化

  webpack-bundle-analyzer插件可以帮助我们分析打包后的图形化的报表.













categories:
  - 前端
toc: true
date: 2018-12-28 16:39:23
---

### 配合一个完整的Demo快速入门、学习Webpack
### 快速了解的几个基本概念
### mode 开发模式
webpack 提供 mode 配置选项，配置 webpack 相应模式的内置优化。
```language
// webpack.production.config.js
module.exports = {
+  mode: 'production',
}
```
### 入口文件(entry)
入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

可以在 webpack 的配置文件中配置入口，配置节点为： entry,当然可以配置一个入口，也可以配置多个。

### 输出(output)
output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件。
```language
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  }
};
```
### 加载器Loader
loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。

### 插件(plugins)
loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。

### 本地安装 webpack
```language
$ npm install --save-dev webpack

# 如果你使用 webpack 4+ 版本，你还需要安装 CLI。
npm install --save-dev webpack-cli
```
注意：不推荐全局安装 webpack。这会将你项目中的 webpack 锁定到指定版本，并且在使用不同的 webpack 版本的项目中，可能会导致构建失败。

安装完成后，可以添加npm的script脚本
```language
// package.json
"scripts": {
    "start": "webpack --config webpack.config.js"
}
```
### 一、Demo配解
#### 1. 初始化项目
```language
mkdir webpack-demo && cd webpack-demo
npm init -y
npm install webpack webpack-cli --save-dev
```
项目结构
```language
  webpack-demo
+ |- package.json
+ |- /dist
+   |- index.html
+ |- /src
+   |- index.js
```
#### 2.安装 loadash 依赖和编写 js 文件
```
npm install --save lodash
```
webpack.config.js 配置文件
```language
const path = require('path');

module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, './dist')
  }
};
```
#### 3. 加载非Js文件
#### 1> 加载Css文件
#####  安装 css 和 style 模块解析的依赖 style-loader 和 css-loader
```language
npm install --save-dev style-loader css-loader
```
#####  添加 css 解析的 loader
```language
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  }
};
```
*PS:
css-loader： 辅助解析 js 中的 import './main.css'
style-loader: 把 js 中引入的 css 内容 注入到 html 标签中，并添加 style 标签.依赖 css-loader*

**你可以在依赖于此样式的 js 文件中 导入样式文件，比如：import './style.css'。现在，当该 js 模块运行时，含有 CSS 字符串的 style 标签，将被插入到 html 文件的 <head>中。**

### 二、Module配置补充
模块(module): 这些选项决定了如何处理项目中的不同类型的模块。
webpack 模块可以支持如下:
- ES2015 import 语句
- CommonJS require() 语句
- AMD define 和 require 语句
- css/sass/less 文件中的 @import 语句。
- 样式(> url(...))或 HTML 文件(> <img src=...>)中的图片链接(image url)
#### module.noParse
防止 webpack 解析那些任何与给定正则表达式相匹配的文件。忽略的文件中不应该含有 import, require, define 的调用，或任何其他导入机制。忽略大型的 library 可以提高构建性能。
```language
module.exports = {
  mode: 'devleopment',
  entry: './src/index.js',
  ...
  module: {
    noParse: /jquery|lodash/,
    // 从 webpack 3.0.0 开始,可以使用函数，如下所示
    // noParse: function(content) {
    //   return /jquery|lodash/.test(content);
    // }
  }
  ...
};
```
#### module.rules
创建模块时，匹配请求的规则数组。这些规则能够修改模块的创建方式。这些规则能够对模块(module)应用 loader，或者修改解析器(parser)。
```language
module.exports = {
  ...
  module: {
    noParse: /jquery|lodash/,
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  }
  ...
};
```
#### module Rule条件详解
- 字符串：匹配输入必须以提供的字符串开始。是的。目录绝对路径或文件绝对路径。
- 正则表达式：test 输入值。
- 函数：调用输入的函数，必须返回一个真值(truthy value)以匹配。
- 条件数组：至少一个匹配条件。
- 对象：匹配所有属性。每个属性都有一个定义行为。
#### Rule.test
{ test: Condition }：匹配特定条件。一般是提供一个正则表达式或正则表达式的数组，但这不是强制的。
```language
module.exports = {
  ...
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  }
  ...
};
```
其它条件，如：
- { include: Condition }:匹配特定条件。一般是提供一个字符串或者字符串数组，但这不是强制的。
- { exclude: Condition }:排除特定条件。一般是提供一个字符串或字符串数组，但这不是强制的。
- { and: [Condition] }:必须匹配数组中的所有条件
- { or: [Condition] }:匹配数组中任何一个条件
- { not: [Condition] }:必须排除这个条件
```language
module.exports = {
  ...
  module: {
    rules: [
      {
        test: /\.css$/,
        include: [
          path.resolve(__dirname, "app/styles"),
          path.resolve(__dirname, "vendor/styles")
        ],
        use: ['style-loader', 'css-loader']
      }
    ]
  }
  ...
};
```
### Rule.use
应用于模块指定使用一个 loader。
**PS:加载器可以链式传递，从右向左进行应用到模块上。**
```language
use: [
  'style-loader',
  {
    loader: 'css-loader'
  },
  {
    loader: 'less-loader',
    options: {
      noIeCompat: true
    }
  }
];
```
*传递字符串（如：use: [ "style-loader" ]）是 loader 属性的简写方式（如：use: [ { loader: "style-loader "} ]）。*

### 加载 Sass 文件
```
安装
npm install sass-loader node-sass webpack --save-dev

```
```language
// webpack.config.js
module.exports = {
  ...
  module: {
    rules: [{
      test: /\.scss$/,
      use: [{
        loader: "style-loader"
      }, {
        loader: "css-loader"
      }, {
        loader: "sass-loader"
      }]
    }]
  }
};
```
### 创建 Source Map

css-loader和sass-loader都可以通过该 options 设置启用 sourcemap。

```language
// webpack.config.js
module.exports = {
  ...
  module: {
    rules: [{
      test: /\.scss$/,
      use: [{
        loader: "style-loader"
      }, {
        loader: "css-loader",
        options: {
          sourceMap: true
        }
      }, {
        loader: "sass-loader",
        options: {
          sourceMap: true
        }
      }]
    }]
  }
};
```
### PostCSS 处理 loader（附带：添加 css3 前缀）
PostCSS是一个 CSS 的预处理工具，可以帮助我们：给 CSS3 的属性添加前缀，样式格式校验（stylelint），提前使用 css 的新特性比如：表格布局，更重要的是可以实现 CSS 的模块化，防止 CSS 样式冲突。（配合prefixer使用)

```language
const path = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, './dist')
  },
  module: {
    rules: [
      {
        test: /\.(sa|sc|c)ss$/,
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              sourceMap: true
            }
          },
          {
            loader: 'postcss-loader',
            options: {
              ident: 'postcss',
              sourceMap: true,
              plugins: loader => [
                require('autoprefixer')({ browsers: ['> 0.15% in CN'] }) // 添加前缀
              ]
            }
          },
          {
            loader: 'sass-loader',
            options: {
              sourceMap: true
            }
          }
        ]
      }
    ]
  }
};
```
### 样式表抽离成专门的单独文件并且设置版本号

Ps:*抽取了样式，就不能再用 style-loader注入到 html 中了。*

```language
npm install --save-dev mini-css-extract-plugin

const path = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const devMode = process.env.NODE_ENV !== 'production'; // 判断当前环境是开发环境还是 部署环境，主要是 mode属性的设置值。

module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, './dist')
  },
  module: {
    rules: [
      {
        test: /\.(sa|sc|c)ss$/,
        use: [
          MiniCssExtractPlugin.loader,
          'css-loader',
          'postcss-loader',
          'sass-loader'
        ]
      }
    ]
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename: devMode ? '[name].css' : '[name].[hash].css', // 设置最终输出的文件名
      chunkFilename: devMode ? '[id].css' : '[id].[hash].css'
    })
  ]
};
```
再次运行打包：

在 dist 目录中已经把 css 抽取到单独的一个 css 文件中了。修改 html，引入此 css 就能看到结果了。

### 开发相关辅助

### 合并两个webpack的js配置文件
开发环境(development)和生产环境(production)配置文件有很多不同点，但是也有一部分是相同的配置内容，如果在两个配置文件中都添加相同的配置节点， 就非常不爽。

webpack-merge 的工具可以实现两个配置文件进合并，这样我们就可以把 开发环境和生产环境的公共配置抽取到一个公共的配置文件中。
```language
npm install --save-dev webpack-merge
```
project
```language
  webpack-demo
  |- package.json
- |- webpack.config.js
+ |- webpack.common.js
+ |- webpack.dev.js
+ |- webpack.prod.js
  |- /dist
  |- /src
    |- index.js
    |- math.js
  |- /node_modules
```
webpack.common.js
```language
+ const path = require('path');
+ const CleanWebpackPlugin = require('clean-webpack-plugin');
+ const HtmlWebpackPlugin = require('html-webpack-plugin');
+
+ module.exports = {
+   entry: {
+     app: './src/index.js'
+   },
+   plugins: [
+     new CleanWebpackPlugin(['dist']),
+     new HtmlWebpackPlugin({
+       title: 'Production'
+     })
+   ],
+   output: {
+     filename: '[name].bundle.js',
+     path: path.resolve(__dirname, 'dist')
+   }
+ };
```
webpack.dev.js
```language
+ const merge = require('webpack-merge');
+ const common = require('./webpack.common.js');
+
+ module.exports = merge(common, {
+   devtool: 'inline-source-map',
+   devServer: {
+     contentBase: './dist'
+   }
+ });
```
webpack.prod.js
```language
+ const merge = require('webpack-merge');
+ const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
+ const common = require('./webpack.common.js');
+
+ module.exports = merge(common, {
+   plugins: [
+     new UglifyJSPlugin()
+   ]
+ });
```
### 解析(resolve)
配置模块如何解析。比如： import _ from 'lodash' ,其实是加载解析了lodash.js文件。此配置就是设置加载和解析的方式。

#### resolve.alias

创建 import 或 require 的别名，来确保模块引入变得更简单。例如，一些位于 src/ 文件夹下的常用模块：
```language
// webpack.config.js
module.exports = {
  mode: 'production',
  entry: './src/index.js',
  output: {
    filename: 'main.[hash].js',
    path: path.resolve(__dirname, './dist')
  },
+ resolve: {
+   alias: {
+     vue$: path.resolve(__dirname, 'src/lib/vue/dist/vue.esm.js'),
+     '@': path.resolve(__dirname, 'src/')
+   }
+ }
  ...
}

// index.js
// 在我们的index.js文件中，就可以直接import
import vue from 'vue';
// 等价于
import vue from  'src/lib/vue/dist/vue.esm.js';
```
#### resolve.extensions
自动解析确定的扩展。
```language
// webpack.config.js
module.exports = {
  mode: 'production',
  entry: './src/index.js',
  output: {
    filename: 'main.[hash].js',
    path: path.resolve(__dirname, './dist')
  },
  resolve: {
    alias: {
      vue$: path.resolve(__dirname, 'src/lib/vue/dist/vue.esm.js'),
      '@': path.resolve(__dirname, 'src/')
    },
+   extensions: [".js", ".vue",".json"]   // 默认值: [".js",".json"]
  }
  ...
}
```
Ps:给定对象的键后的末尾添加 $，以表示精准匹配

## 相关的loader列表
### 文件
raw-loader 加载文件原始内容（utf-8）
val-loader 将代码作为模块执行，并将 exports 转为 JS 代码
url-loader 像 file loader 一样工作，但如果文件小于限制，可以返回 data URL
file-loader 将文件发送到输出文件夹，并返回（相对）URL
### JSON
json-loader 加载 JSON 文件（默认包含）
json5-loader 加载和转译 JSON 5 文件
cson-loader 加载和转译 CSON 文件
### 转换编译(Transpiling)
script-loader 在全局上下文中执行一次 JavaScript 文件（如在 script 标签），不需要解析
babel-loader 加载 ES2015+ 代码，然后使用 Babel 转译为 ES5
buble-loader 使用 Bublé 加载 ES2015+ 代码，并且将代码转译为 ES5
traceur-loader 加载 ES2015+ 代码，然后使用 Traceur 转译为 ES5
ts-loader 或 awesome-typescript-loader 像 JavaScript 一样加载 TypeScript 2.0+
coffee-loader 像 JavaScript 一样加载 CoffeeScript
### 模板(Templating)
html-loader 导出 HTML 为字符串，需要引用静态资源
pug-loader 加载 Pug 模板并返回一个函数
jade-loader 加载 Jade 模板并返回一个函数
markdown-loader 将 Markdown 转译为 HTML
react-markdown-loader 使用 markdown-parse parser(解析器) 将 Markdown 编译为 React 组件
posthtml-loader 使用 PostHTML 加载并转换 HTML 文件
handlebars-loader 将 Handlebars 转移为 HTML
markup-inline-loader 将内联的 SVG/MathML 文件转换为 HTML。在应用于图标字体，或将 CSS 动画应用于 SVG 时非常有用。
### 样式
style-loader 将模块的导出作为样式添加到 DOM 中
css-loader 解析 CSS 文件后，使用 import 加载，并且返回 CSS 代码
less-loader 加载和转译 LESS 文件
sass-loader 加载和转译 SASS/SCSS 文件
postcss-loader 使用 PostCSS 加载和转译 CSS/SSS 文件
stylus-loader 加载和转译 Stylus 文件
### 清理和测试(Linting && Testing)
mocha-loader 使用 mocha 测试（浏览器/NodeJS）
eslint-loader PreLoader，使用 ESLint 清理代码
jshint-loader PreLoader，使用 JSHint 清理代码
jscs-loader PreLoader，使用 JSCS 检查代码样式
coverjs-loader PreLoader，使用 CoverJS 确定测试覆盖率
### 框架(Frameworks)
vue-loader 加载和转译 Vue 组件
polymer-loader 使用选择预处理器(preprocessor)处理，并且 require() 类似一等模块(first-class)的 Web 组件
angular2-template-loader 加载和转译 Angular 组件
Awesome 更多第三方 loader，查看 awesome-webpack 列表。
### 打包分析优化
webpack-bundle-analyzer插件可以帮助我们分析打包后的图形化的报表.













