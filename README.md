
# ste-vue-inset-loader
#### 编译阶段在sfc模板指定位置插入自定义内容，适用于webpack构建的vue应用，常用于小程序需要全局引入组件的场景。（由于小程序没有开放根标签，没有办法在根标签下追加全局标签，所以要使用组件必须在当前页面引入组件标签）
该项目基于 [vue-inset-loader](https://github.com/1977474741/vue-inset-loader)，做了如下修改：
1. 支持h5，app等多端。
2. 修复了获取不到页面内容就报错的问题。

### 第一步 安装

    npm install ste-vue-inset-loader --save-dev

### 第二步 vue.config.js注入loader

const path = require('path');
module.exports = {
    chainWebpack: (config) => {
        config.module
            .rule('vue')
            .test(/\.vue$/)
            .use()
            .loader(path.resolve(__dirname, './node_modules/ste-vue-inset-loader'))
            .end();
    },
};


### 第三步 pages.json配置文件中添加insetLoader

    "insetLoader": {
        "config":{
            "confirm": "<BaseConfirm ref='confirm'></BaseConfirm>",
            "abc": "<BaseAbc ref='BaseAbc'></BaseAbc>"
        },
        // 全局配置
        "label":["confirm","abc"],
        "rootEle":".*"
    },
    "pages": [
        {
            "path": "pages/tabbar/index/index",
            "style": {
                "navigationBarTitleText": "测试页面",
                // 单独配置，用法跟全局配置一致，优先级高于全局
                "label": ["confirm","abc"],
                "rootEle":"div"
            }
        },
    ]

###  配置说明

 - `config` (default: `{}`)
    定义标签名称和内容的键值对
 - `label`(default: `[]`)
    需要全局引入的标签，打包后会在所有页面引入此标签
 - `rootEle`(default: `"div"`)
    根元素的标签类型，缺省值为div，支持正则，比如匹配任意标签 ".*"

 ✔ `label` 和 `rootEle` 支持在单独页面的style里配置，优先级高于全局配置