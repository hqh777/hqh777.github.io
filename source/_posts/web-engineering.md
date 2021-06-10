---
title: 前端工程化
date: 2021-06-10 15:46:33
---

# 一、工程化概念

## 1、工程化定义
   遵循一定标准和规范，通过工具去提高效率降低成本的一种手段
## 2、主要解决的问题：
• 传统语言或语法的弊端
• 无法使用模块化/组件化
• 重复的机械式工作
• 代码风格统一、质量保证
• 依赖后端服务接口支持
• 整体依赖后端项目
## 3、工程化表现
一切重复的工作都应该被自动化
• 创建项目——使用脚手架工具自动完成基础结构搭建
• 编码——借助工程化工具，格式化代码、校验代码风格、编译/构建/打包
• 预览/测试——Web Server/Mock、Live Reloading/HMR、Source Map
• 提交——Git Hooks、Lint-staged、持续集成
• 部署——CI/CD、自动发布
# 二、脚手架工具
## 1、概要
本质作用：创建项目基础结构、提供项目规范和约定
• 相同的组织结构
• 相同的开发模式
• 相同的模块依赖
• 相同的工具配置
• 相同的基础代码
## 2、常用的脚手架工具
• React——create-react-app
• Vue——vue-cli
• Angular——angular-cli
• Yeoman——通用
• Plop——创建特定类型的文件
## 3、Yeoman
（1）基本使用
安装 Yeoman 运行 generator
yarn global add yo
yarn global add generator-node
mkdir my-module
cd my-module\
yo node
（2）Sub Generator
yo node:cli
yarn link // 链接全局范围
my-midule --help
（3）使用步骤
• 明确需求
• 找到合适的 Generator 
• 全局范围安装找到的 Generator
• 通过 Yo 运行对应的 Generator
• 通过命令行交互填写选项
• 声称你所需要的项目结构
（4）自定义 Generator
基于 Yeoman 搭建自己的脚手架
mkdir generator-sample
cd generator-sample\
yarn init
yarn add yeoman-generator
```
// index.js 
// 写入文件功能
const Generator = require('yeoman-generator')
module.export = class extends Generator {
    writing () {
    // Yeoman 自动在生成文件阶段调用此方法
    // 我们这里尝试往项目目录中写入文件
    this.fs.write(
        this.destinationPath('temp.txt'),
      Math.random().toString()
    )
  }
}
```
// 模板创建文件
// template/foo.txt
```
const Generator = require('yeoman-generator')
module.export = class extends Generator {
  prompting () {
    // Yeoman 在询问用户环节会自动调用此方法
    // 在此方法中可以调用父类的 prompt() 方法发出对用户命令行询问
    return this.prompt([
      {
        type: 'input',
        name: 'name',
        message: 'Your project name',
        default: this.appname // appname 为项目生成目录名称
      }
    ])
    .then(answer => {
        // answer => { name: 'user input value' }
      this.answers = answer
    })
  }
    writing () {
    // 通过模板方式写入文件到目标目录
    // 模板文件路径
    const tmpl = this.templatePath('foo.txt')
    // 输出目标路径
    const output = this.destinationPath('foo.txt')
    // 模板数据上下文
    const context = this.answers
    this.fs.copyTpl(tmpl, output, context)
  }
}
```
（5）Vue Generator 案例
（6）发布 Generator
```
echo node_modules > .gitignore // 创建 .gitignore 文件忽略 node_modules
git init // 初始化本地空仓库
git status
git add .
git commit
git remote add origin 远端仓库地址
git push -u origin master
npm publish // 必须是官方镜像
```
## 4、Plop
（1）简介
主要用于创建项目中特定类型文件的小工具，集成到项目中自动化创建同类型的文件。
（2）基本使用
yarn add plop --dev
```
// 根目录新建 polpfile.js 文件
// Plop 入口文件，需要导出一个函数
// 此函数接收一个 Plop 对象，用于创建生成器任务
module.exports = plop => {
    plop.setCenerator('component', {
    description: 'creatr a component',
    prompts: [
      {
        type: 'input', // 指定输入方式
        name: 'name', // 指定返回值的键
        meaasge: 'component name', // 屏幕上给出的提示
        defaults: 'MyComponent' // 默认答案
      }
    ],
    action: [
      {
        type: 'add', // 指定动作类型，添加文件
        path: 'src/components/{{name}}/{{name}}.js', // 添加文件的路径
        templateFile: 'plop-templates/component.hbs' // 模板文件
      }
    ]
  })
}
yarn plop component
```
# 总结：
• 将 plop 模块作为项目开发依赖安装
• 在项目根目录下创建一个 plopfile.js 文件
• 在 plopfile.js 文件中定义脚手架任务
• 编写用于生成特定类型文件的模板
• 通过 Plop 提供的 CLI 运行脚手架任务
## 5、脚手架工作原理
```
// package.json 文件添加 bin 字段，用于指定 cli 应用的入口文件 cli.js
// cli.js cli 入口文件

#!/user/bin/env node
// Node CLI 应用入口文件必须要有这样的文件头
// 如果是Linux 或者 macOS 系统下还需要修改此文件的读写权限为 755
// 具体就是通过 chmod 755 cli.js 实现修改
// 脚手架的工作过程：
// 1.通过命令行交互询问用户问题 yarn add inquirer
// 2.根据用户回答的结果生成文件
const fs = require('fs')
const path = require('path')
const inquirer = require('inquirer')
const ejs = require('ejs') // yarn add ejs
inquirer.prompt([
  {
    type: 'input',
    name: 'name',
    message: 'project name?'
  }
])
.then(anwsers => {
    // console.log(anwsers)
  // 根据用户回答的结果生成文件
  
  // 模板目录
  const tmplDir = path.join(_dirname, 'templates')
  // 目标目录
  const destDir = process.cwd()
  
  // 将模板下的文件全部转换到目标目录
  fs.readdir(tmplDir,(err, files) => {
    if (err) throw err
    files.forEach(file => {
        //通过模板引擎渲染文件
      ejs.renderFile(path.join(tmplDir, file), anwsers, (err, result) => {
        if (err) throw err
        
        // 将结果写入目标文件路径
        fs.writeFileSync(path.join(destDir, file), result)
      })
    })
  })
})
```
# 三、自动化构建
## 1、简介
开发阶段写出来的源代码自动化转换成生产环节可以运行的代码
作用：脱离运行环境兼容带来的问题
## 2、常用的自动化构建工具
• Grunt——基于临时文件实现，磁盘读写操作，构建速度慢
• Gulp——基于内存实现的，支持同时执行多个任务
• FIS——适用于初学者
注：严格来说 webpack 是模块打包工具
## 3、Grunt
（1）
## 4、Gulp
（1）基本使用
```
code gulpfile.js // gulp 入口文件
exports.foo = done => {
    done() // 标识任务完成
}
exports.default = done => {
    done() // 标识任务完成
}
// gulp 4.0 以前
const gulp = require('gulp')
gulp.task('bar', done => {
    done()
})
```
（2）组合任务
```
exports.foo = series(task1,task2, task3) // 串行
exports.bar = parallel(task1,task2, task3) // 并行
```
（3）异步任务
```
exports.promise = () => {
  console.log('promise task~')
    return Promise.resolve()
}
const timeout = time => {
    return new promise(resolve => {
    setTimeout(resolve, time)
  })
}
exports.async = async () => {
  await timeout(1000)
    return Promise.resolve()
}
exports.stream = () => {
    const readStream = fs.createReadStream('package.json')
  const writeSteam = fs.createWriteStream('temp.txt')
  readStream.pipe(writeStream)
  return readStream
}
exports.stream = () => {
    const readStream = fs.createReadStream('package.json')
  const writeSteam = fs.createWriteStream('temp.txt')
  readStream.pipe(writeStream)
  readStream.on('end', () => {
    done()
  })
}
```
（4）核心工作原理
```
const fs = require('fs')
const { Transform } = require('stream')
exports.default = () => {
    // 文件读取流
  const read = fs.createReadStream('normalize.css')
  // 文件写入流
  const write = fs.createWriteStream('normalize.min.css')
  // 文件转换流
  const = transform = new Transform({
    transform: (chunk, encoding, callback) => {
        // 核心转换过程实现
      // chunk => 读取流中读取到的内容 （Buffer）
      const input = chunk.toString()
      const output = input.replace(/\s+/g, '').replace(/\/\*.+?\*\//g, '')
      callback(null, output)
    }
  })
  // 把读取出来的文件流导入写入流
  read.pipe(write)
  return read
}
```
（5）文件操作 API
```
const { src, dest } = require('gulp')
const cleanCss = require('gulp-clean-css')
const rename = require('gulp-rename')
exports.default = () => {
    return src('src/*.css') // 读取流 src 目录下所有的 css 文件
    .pipe(cleanCss()) // css 转换流（压缩代码） yarn add gulp-clean-css --dev
    .pipe(rename({ extname: '.min.css' })) // 重命名 yarn add gulp-rename --dev
    .pipe(dest('dist')) // 写入流
}
```
（6）
```
const { src, dest, parallel, series, watch }  = require('gulp')
const loadPlugins = require('gulp-load-plugins') // yarn add gulp-load-plugins --dev
const plugins = loadPlugins()
const sass = require('gulp-sass')
const babel = require('gulp-babel')
const swig = require('gulp-swig')
const imagemin = require('gulp-imagemin')
const del = require('del')
const browserSync = require('browser-sync')
// 创建开发服务器
const bs = browserSync.create()
// 样式编译
// yarn add gulp-sass --dev
const style = () => {
    return src('src/assets/styles/*.css', { base: 'src' })
  .pipe(sass({ outputStyle: 'expanded' }))
  .pipe(dest('temp'))
}
// 脚本编译 
// yarn add gulp-babel --dev
// yarn add @babel/core @babel/preset-env
const script = () => {
    return src('src/assets/scripts/*.js', { base: 'src' })
  .pipe(babel({ presets: ['@babel/preset-env'] }))
  .pipe(dest('temp'))
}
// 页面模板编译
// yarn add gulp-swig --dev
const page = () => {
    return src('src/*.html', { base: 'src' })
  .pipe(swig({ data }))
  .pipe(dest('temp'))
}
// 图片和字体文件转换
// yarn add gulp-imagemin --dev
const image = () => {
    return src('src/assets/images/**', { base: 'src' })
  .pipe(imagemin())
  .pipe(dest('dist'))
}
const font = () => {
    return src('src/assets/fonts/**', { base: 'src' })
  .pipe(imagemin())
  .pipe(dest('dist'))
}
// 其他文件及文件清除
const extra = () => {
    return src('public/**', { base: 'piblic' })
  .pipe(dest('dist'))
}
// yarn add del --dev
const clean = () => {
    return del(['dist', 'temp'])
}
// 开发热更新服务器
// yarn add browser-sync --dev
const serve = () => {
  watch('src/assets/styles/*.css', style)
  watch('src/assets/scripts/*.js', script)
  watch('src/*.html', page)
  // watch('src/assets/images/**', image)
  // watch('src/assets/fonts/**', font)
  // watch('public/**', extra)
  watch(['src/assets/images/**',
        'src/assets/fonts/**',
         'public/**'
        ], bs.reload)
  
  // 可能会因为 swig 模板引擎缓存的机制导致页面不会变化
  // 此时需要额外将 swig 选项中的 cache 设置为false
  
    bs.init({
    notify: false, // 页面提示
    port: 2080, // 端口号
    open: false, // 取消默认打开浏览器
    files:'dist/**', // 热更新修改的文件
    server: {
        baseDir: ['temp', 'src', 'public'],
      routes: {
        '/node_modules': 'node_modules'
      }
    }
  })
}
// useref 文件引用处理
// yarn add gulp-useref
// yarn add gulp-htmlmin gulp-uglify gulp-clean-css --dev
// yarn add gulp-if --dev
const useref = () => {
    return src('temp/*.html', { base: 'temp' })
  .pipe(plugins.useref({searchPath: ['temp', '.']}))
  // html js css
  .pipe(plugins.if(/\.js$/, plugins.uglify()))
  .pipe(plugins.if(/\.css$/, plugins.cleanCss()))
  .pipe(plugins.if(/\.html$/, plugins.htmlmin({
    collapseWhitespace: true,
    minifyCSS: true,
    minifyJS: true
  })))
  .pipe(dest('dist'))
}
const compile = parallel(style, script, page)
// 上线之前执行的任务
const build = series(clean, parallel(series(compile, useref), image, font, extra))
const develop = series(compile, serve)
module.exports = {
  clean,
  compile,
    build,
  develop,
  useref
}
```