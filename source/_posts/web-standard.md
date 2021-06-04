---
title: 前端规范
---

# 一、前端规范

## 1.1 项目结构

    api/                            请求api目录
    |-- auth.js                         认证请求文件
    assets/                         静态资源目录
    |-- styles/                         样式目录
    |--- common/                            公共样式
    |----- base.less                            自行添加全局less
    |----- flex.less                            flex封装类
    |----- font.less                            字体封装类
    |----- main.less                            公共样式入口
    |----- simple.less                          less类名简写封装
    |----- variables.less                       全局less变量
    |--- page.less                          基础后台页面表单样式
    |--- transition.less                    过渡样式
    |--- darkmodal.less                     大屏暗色系modal样式
    |-- fonts                           字体目录
    |-- img                             图片目录
    |-- icon                            图标目录
    components/                     组件目录
    |-- base/                          后台CRUD基础组件
    |-- blocks/                        分块组件目录(适用于大屏分解等)
    |-- echarts/                       echarts组件
    |-- map/                           map组件
    |-- ui/                            ui组件
    config                          全局配置(与环境配置相关放env文件)
    |-- index.js
    theme                           主题配置
    |-- index.js                        自定义变量与antd变量配置
    directives                      全局自定义指令
    |-- index.js
    filters                         全局过滤器
    |-- index.js
    layouts                         布局(块)目录
    |-- backstage                     后台layout布局
    |--- index.vue                       后台视图入口
    |--- section/                        后台布局分块
    |---- AppHeader.vue                      头部
    |---- Navbar.vue                         导航面包屑
    |---- Sidebar.vue                        侧边栏菜单
    |---- SidebarItem.vue                    侧边栏菜单内子组件
    |---- AppMain.vue                        主要内容区
    |-- screen                        大屏layout布局(视情况用)
    |---- index.vue                      视图入口
    |---- DataHeader.vue                 头部
    |---- DataContent.vue                内容区
    mixins                          全局mixin
    |-- index.js
    plugins                         插件目录
    |-- ant-design-vue.js               按需引入antd-vue组件
    |-- vue-echarts.js                  按需引入vue-echarts组件
    router                          路由配置
    |-- index.js                        路由入口
    |-- constant-router.js               静态路由(一般：登录+错误页+大屏等)
    |-- permission.js                   权限处理、进度条、标题等处理
    |-- async-router.js                  异步路由(一般用于后台异步权限过滤 *已弃用！)
    |-- modules/                        模块路由(路由过多时拆解 *已弃用！)
    store                           状态管理
    |-- index.js                        vuex入口
    |-- getters.js                      全局getters
    |-- mutation-types.js               mutation名称
    |-- modules/                        store模块
    mock                            mockjs
    |-- index.js                        mock入口
    |-- modules/                        各个mock模块
    utils                           工具类
    |-- http.js                         请求封装
    |-- message.js                      消息提示工具类
    |-- preload.js                      预加载转场动画工具类(搭配index.html)
    views                           视图目录
    |-- backstage/                      后台views
    |-- screen/                         大屏views
    |-- login/                          登录页
    |-- error/                          错误转发页
    main.js                         入口文件


## 1.2 命名规范

命名严禁使用拼音与英文混合的方式，更不允许直接使用中文的方式。 
说明：正确的英文拼写和语法可以让阅读者易于理解，避免歧义。
注意，即使纯拼音缩写命名方式也要避免采用 (除表单中数据与后端接口绑定值为拼音简写时可使用)

### 1.2.1 项目命名

采用小写方式，以下划线间隔。

例如：

```
mall_management_system (√)

mall-management-system (×)

mallManagementSystem (×)
```

若同项目名有不同端展示可追加中划线。

例如：

```
mall_management_system-pc (√)

mall_management_system-app (√)

mall_management_system-mini (√)
```

### 1.2.2 目录命名

全部采用小写方式，以中划线分隔，有复数结构时，要采用复数命名法，缩写不用复数。

例如：

```
styles / components / utils / demo-styles / demo-scripts / img / doc (√)

style / component / util / demoStyles / demo_scripts / imgs / docs (×)
```

### 1.2.3 文件命名（JS、CSS、LESS、HTML、PNG）

全部采用小写方式，以中划线分隔。

```
render-dom.js / user-management.html (√)

renderDom.js / UserManagement.html  (×)
```

特殊情况：当图片文件包含与主题相关联的不同套个数时可追加下划线及主题名

例如：

```
theme-switch_light.png  (√) //light主题下的theme-switch

theme-switch_dark.png  (√)  //dark主题
```

### 1.2.4 vue文件命名

`views` 下视图`.vue`文件，以中划线分隔。

例如：

```
system-settings.vue  (√)

systemSettings.vue  (×)

SystemSettings.vue  (×)
```

注：`views`下视图文件一般以文件夹为模块主体命名，其中`vue`视图文件一般以列表：`list.vue`，详情：`detail.vue`，新增：`add.vue`等形式命名。

`components`下组件`vue`文件按大驼峰命名。

例如：

```
AppHeader (√)

appHeader (×)

app-header (×)
```

### 1.2.5 JS变量命名

命名方式：小驼峰

命名规范：前缀名词

命名建议：语义化

例如：

```javascript
let isLogin = true; //(√)

let tableTitle = LoginTable; //(√)

let isFlag = true; //(×)

let getTitle = LoginTable; //(×)
```

### 1.2.6 JS常量命名

命名方式：全部大写

命名规范：使用大写字母和下划线来组合命名，下划线用以分割单词

命名建议：语义化

例如：

```javascript
const MAX_COUNT = 10; //(√)

const BASE_URL = 'http://www.foreverz.com'; //(√)
```

### 1.2.7 JS函数命名

命名方式：小驼峰式命名法。

命名规范：前缀应当为动词。

命名建议：语义化（动词 + 名词）

可以参考如下的动作：

| **动词** | **含义**                     | **返回值**                                            |
| -------- | ---------------------------- | ----------------------------------------------------- |
| can      | 判断是否可执行某个动作(权限) | 函数返回一个布尔值。true：可执行；false：不可执行     |
| has      | 判断是否含有某个值           | 函数返回一个布尔值。true：含有此值；false：不含有此值 |
| is       | 判断是否为某个值             | 函数返回一个布尔值。true：为某个值；false：不为某个值 |
| get      | 获取某个值                   | 函数返回一个非布尔值                                  |
| set      | 设置某个值                   | 无返回值、返回是否设置成功或者返回链式对象            |
| load     | 加载某些数据                 | 无返回值或者返回是否加载完成的结果                    |

例如：

```javascript
function canRead() //(√)

function getUserList() //(√)
```



### 1.2.8 特殊情况命名

#### 1.2.8.1 api文件

文件名：一般与`views`下视图文件名字对应

例如：

```
//api文件目录
api/user.js
//views下视图vue文件
views/backstage/user/list.vue
```

增删改查方法命名及注释。

例如：

```javascript
/**
 * 查询用户列表
 * @param {Integer} pageNumber 
 * @param {Integer} pageSize 
 * @param {Object} formObj 
 */
export const queryUserList = (pageNumber, pageSize, formObj) => {
  ...
}

/**
 * 查询用户信息
 * @param {Integer} userId 
 */
export const getUserInfo = (userId) => {
  ...
}

/**
 * 新增用户
 * @param {Object} user 
 */
export const addUser = (user) => {
  ...
}

/**
 * 修改用户信息
 * @param {Object} user 
 */
export const updateUser = ({ id, ...user }) => {
  ...
}

/**
 * 删除用户
 * @param {Integer} userId 
 */
export const deleteUser = (userId) => {
  ...
}
```



#### 1.2.8.2 config配置文件

说明：配置最高级模块采用全大写命名，子级采用下划线分隔命名
例如：

```javascript
module.exports = {
  OAUTH: {
      //oauth中请求头内需加密的client_id
      client_id: 'client',
      //oauth中请求头内需加密的client_secret
      client_secret: '123456'
  },
  ...
}
```

#### 1.2.8.3 theme主题文件

说明：主题文件内自定义变量命名以`h-`开头与`antdv`变量区分开
例如：

```javascript
light: {
  'h-background': '#f0f2f5',  //自定义背景底色（非antd内变量）
  'h-action-bar-bg': '#FAFBFD',     //自定义action bar 背景色
  'body-background': '#fff',  //背景
  'text-color': '#5A6275', // 主文本色
},
dark: {
  'h-background': '#1B2431',  //自定义背景底色
  'h-action-bar-bg': '#323D4E',     //自定义action bar 背景色
  'body-background': '#273142',  //背景
  'text-color': '#CCD0D9', // 主文本色
}
```



### 1.2.9 CSS命名

> 前言:  ID 和 class 的名称总是使用可以反应元素目的和用途的名称，或其他通用的名称，代替表象和晦涩难懂的名称。

> BEM说明：class命名使用BEM其实是块（block）、元素（element）、修饰符（modifier）的缩写，利用不同的区块，功能以及样式来给元素命名。这三个部分使用-与_连接。

#### 1.2.9.1 中划线可以用来表示层级关系

```html
<div class="box">
    <ul class="box-list">
        <li class="box-list-item"></li>
        <li class="box-list-item"></li>
        <li class="box-list-item"></li>
    </ul>
</div>
```

#### 1.2.9.2 下划线可以用来表示不同的状态

```html
<div class="box">
    <button class="box-btn box-btn_default" type="button"></button>
    <button class="box-btn" type="button"></button>
</div>
```

#### 1.2.9.3 尽量避免使用标签名

从结构、表现、行为分离的原则来看，应该尽量避免 css 中出现 HTML 标签，并且在 css 选择器中出现标签名会存在潜在的问题。

#### 1.2.9.4 需要匹配到 DOM 末端的选择器， 总是考虑直接子选择器

```css
/* 推荐(√) */
.content > .title {
    font-size: 16px;
}

/* 不推荐(×) */
.content .title { 
    font-size: 16px; 
}

```

#### 1.2.9.5 尽量使用缩写属性

```css
/* 推荐(√) */
div{ 
    border-top: 0;
    font: 100%/1.6 palatino, georgia, serif;
    padding: 0 1px 2px;
} 

/* 不推荐(×) */
div{
    border-top-style: none;
    font-family: palatino, georgia, serif;
    font-size: 100%;
    line-height: 1.6;
    padding-bottom: 2px;
    padding-left: 1px;
    padding-right: 1px;
    padding-top: 0;
}

```

#### 1.2.9.6 省略 0 后面的单位

```css
/* 推荐(√) */
div{
    padding-bottom: 0;
    margin: 0;
}

/* 不推荐(×) */
div{
    padding-bottom: 0px;
    margin: 0px;
}

```

#### 1.2.9.7 避免使用 ID选择器及全局标签选择器防止污染全局样式

```css
/* 推荐(√) */
.header{
    ...
}

/* 不推荐(×) */
#header{
    ...
}
```

