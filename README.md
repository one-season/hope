# hope
  * 基于Vite4+TypeScript的Vue3组件库开发框架
  * hope是一个 Vue3 组件库开发环境框架，采用最新的 Vite4+TypeScript 为技术栈，支持按需加载、单元测试、自动打包与发布等功能，让我们能更专注于业务组件的开发。

## Monorepo项目搭建
  * Monorepo：就是一个代码库里包含很多的项目，而这些项目虽然是相关联的，但是在逻辑上是独立的，可以由不同人或者团队来维护；

### pnpm初始化项目
  * pnpm 对于包的管理是很方便的，尤其是对于一个 Monorepo 的项目。因为对于我们即将开发的组件库来说可能会存在多个 package(包)，而这些包在我们本地是需要相互关联测试的，刚好 pnpm 就对其天然的支持。其实像其它包管理工具，比如 yarn、lerna 等也能做到，但是相对来说比较繁琐。而 pnpm 现在已经很成熟了，像 Vant，ElementUI 这些明星组件库都在使用 pnpm，因此本项目也采用 pnpm 作为包管理工具。
#### 安装
  * npm i pnpm -g
#### 初始化
  * 在项目根目录执行`pnpm init`，会自动生成`package.json`文件；
    ```js
      {
        "name": "hope",
        "version": "1.0.0",
        "description": "",
        "main": "index.js",
        "scripts": {
          "test": "echo \"Error: no test specified\" && exit 1"
        },
        "keywords": [],
        "author": "",
        "license": "ISC"
      }
    ```
    

### 包管理
#### 新建 packages 文件夹用于存放我们的各种包；
  * 创建 first和second 两个文件夹（以下简称f、s）当作已有的两个包，用于测试pnpm的本地引用；
  * 分别在 f 和 s 目录下执行`pnpm init`初始化；
  * 更改 f/s 下面package.json文件里的包名（name：'frist/second' -> '@hope/frist / @hope/second'），表示这个包属于hope这个组织下的；
#### 验证本地包的关联；
  * 在 s 入口文件index.js 引入 f 的入口文件index.js 导出的函数，并调用；
  * 在根目录新建 pnpm-workspace.yaml 文件，这样就表示 packages 目录下的所有包都被关联了；
    ```js
      packages:
        - 'packages/**'
    ```
  * s文件里引入了f包，所以要在s目录下安装f包 `pnpm add @hope/first`，这时可以在s的package.json里看到依赖包；
    ```js
      "dependencies": {
        "@hope/first": "workspace:^1.0.0"
      }
    ```
  * 因为我们在文件里使用的是ESModule语法，所以两者入口文件里添加字段"type": "module"，最后执行node index.js，有打印f文件里的log就证明完成了本地包的关联；

