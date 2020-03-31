# babel-plugin-component

[![NPM version](https://img.shields.io/npm/v/babel-plugin-component.svg)](https://npmjs.org/package/babel-plugin-component)
[![Build Status](https://travis-ci.org/ElementUI/babel-plugin-component.svg?branch=master)](https://travis-ci.org/ElementUI/babel-plugin-component)
[![Coverage Status](https://coveralls.io/repos/github/QingWei-Li/babel-plugin-component/badge.svg?branch=master)](https://coveralls.io/github/QingWei-Li/babel-plugin-component?branch=master)

## 安装

```shell
npm i babel-plugin-elementui-demand -D

```

## 使用

通过 `.babelrc` 或 `babel.config.js` 或者 babel-loader 配置

### 例子

`/babel.config.js`

```javascript
module.exports = {
  "plugins": [
    [
      "component",
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-chalk",
        "ext": ".scss", // 此处指明加载的样式文件后缀为 scss
        "isElementUI": true // 指明为加载 element-ui 的样式
      }
    ]
  ]
}
```

`/vue.config.js`

```javascript
module.exports = {
    css: {
        loaderOptions: {
            scss: {
                // 加载 scss 文件前会引入的 scss 文件（此文件不要使用样式，仅用于存储变量及其他不会生成 css 文件的内容）
                // （此处就是用于修改 element-ui 的样式变量。（ src/assets 下新建一个 element-variables.scss 文件）
                // 文件内容可参见：https://github.com/ElementUI/theme-chalk/blob/master/src/common/var.scss
                prependData: `@import "~@/assets/element-variables.scss";`
            }
        }
    }
}
```

`/src/assets/element-variables.scss`

```scss
/* 改变主题色变量 */
$--color-primary: #f00;
```

`/src/main.js`

```javascript
// 将要使用的组件，增加到 main.js 或者其他地方

import { Button } from 'element-ui'

Vue.use(Button)

```


## 以下为原插件内容  

## 例子

插件会将下面的

```javascript
import { Button } from 'components'
```

转换为

```javascript
var button = require('components/lib/button')
require('components/lib/button/style.css')
```

## styleLibraryName 例子

插件会将下面的

```javascript
import Components from 'components'
import { Button } from 'components'
```

转换为

```javascript
require('components/lib/styleLibraryName/index.css')
var button = require('components/lib/styleLibraryName/button.css')
```

## 使用

通过 `.babelrc` 或者 babel-loader 配置

```javascript
{
  "plugins": [["component", options]]
}
```

## 多模块
```javascript
{
  "plugins": [xxx,
    ["component", {
      libraryName: "antd",
      style: true,
    }, "antd"],
    ["component", {
      libraryName: "test-module",
      style: true,
    }, "test-module"]
  ]
}
```

### 组件目录结构
```
- lib // 'libDir'
  - index.js // or custom 'root' relative path
  - style.css // or custom 'style' relative path
  - componentA
    - index.js
    - style.css
  - componentB
    - index.js
    - style.css
```

### 主题库目录结构
```
- lib
  - theme-default // 'styleLibraryName'
    - base.css // required
    - index.css // required
    - componentA.css
    - componentB.css
  - theme-material
    - ...
  - componentA
    - index.js
  - componentB
    - index.js
```
或者 
```
- lib
  - theme-custom // 'styleLibrary.name'
    - base.css // if styleLibrary.base true
    - index.css // required
    - componentA.css // default 
    - componentB.css
  - theme-material
    - componentA
      -index.css  // styleLibrary.path  [module]/index.css
    - componentB
      -index.css
  - componentA
    - index.js
  - componentB
    - index.js
```

### 选项

- `["component"]`: 导入 js 的模块
- `["component", { "libraryName": "component" }]`: 模块名称
- `["component", { "styleLibraryName": "theme_package" }]`: 样式模块名称
- `["component", { "styleLibraryName": "~independent_theme_package" }]`: 导入独立的主题包
- `["component", { "styleLibrary": {} }]`: 导入具有更多配置的独立主题包
  ```
  styleLibrary: {
    "name": "xxx", // 与 styleLibraryName 相同
    "base": true,  // 如果主题包中有 base.css 文件
    "path": "[module]/index.css",  // the style path. e.g. module Alert =>  alert/index.css
    "mixin": true  // 如果主题包中找不到 css 文件，则使用 [libraryName] 的 css 文件
  }
  ```
- `["component", { "style": true }]`: 从 'style.css' 中导入 js 和 css
- `["component", { "style": cssFilePath }]`: 从文件路径（filePath）导入 style css
- `["component", { "libDir": "lib" }]`: lib 目录
- `["component", { "root": "index" }]`: 主文件目录
- `["component", { "camel2Dash": false }]`: 是否将名称解析为破折号模式，默认为 `true` 



