# Vue课程

## 第一部分 预备知识 —— git

1. 安装（略）

2. 配置

   1. 配置name和email

      ``` bash
      git config --global user.name "名称"
      git config --global user.email "QQ邮箱"
      ```

3. 使用git：

   - 查看当前仓库的状态

   git status

    
On branch master
nothing to commit, working tree clean

   - 初始化仓库

    
     git init


   - 文件状态：

     1. 未跟踪  === U
     2. 已跟踪  === 
     3. 暂存
     4. 未修改
     5. 已修改  === M

   - 未跟踪 → 暂存

   
     git add 文件名称    
      / 将文件切换到暂存的状态

     git add *           
     将所有 已修改/未跟踪 的文件都可以暂存
     ```

   - 暂存 → 未修改

     //提交给git仓库
    1.将暂存的文 件存储到仓库中
     git commit -m "文件名称" 

    2. 提交所有已修改的文件（未跟踪的文件不会提交）
     git commit -a -m "文件名称" 
    
     ```

   - 未修改 → 修改

     - 修改代码后，文件会变为修改状态

4. 常用的命令

   1. 重置文件

   ```bash
   git restore 文件名称 # 恢复文件
   git restore * # 恢复所有文件
   git restore --staged 文件名称 # 取消暂存状态
   ```

   2. 删除文件	

   ```bash
   git rm 文件名称 # 删除文件
   git rm 文件名称 -f # 强制删除
   ```

   3. 移动文件

   ```bash
   git mv from to # 移动文件 重命名文件
   ```

   ### 分支

   git在存储文件时，每一次代码代码的提交都会创建一个与之对应的节点，git就是通过一个一个的节点来记录代码的状态的。

   节点会构成一个树状结构，树状结构就意味着这个树会存在分支，默认情况下仓库只有一个分支，命名为master。
   
   在使用git时，可以创建多个分支，分支与分支之间相互独立，在一个分支上修改代码不会影响其他的分支。

   ```bash
   git branch # 查看当前分支
   git branch <branch name> # 创建新的分支
   git branch -d <branch name> # 删除分支
   git switch <branch name> # 切换分支
   git switch -c <branch name> # 创建并切换分支
   git merge <branch name> # 和并分支
   ```

   在开发中，都是在自己的分支上编写代码，代码编写完成后，在将自己的分支合并到主分支中。
   
   ### 变基（rebase）
   
   在开发中除了通过merge来合并分支外，还可以通过变基来完成分支的合并。
   
   我们通过merge合并分支时，在提交记录中会将所有的分支创建和分支合并的过程全部都显示出来，这样当项目比较复杂，开发过程比较波折时，我必须要反复的创建、合并、删除分支。这样一来将会使得我们代码的提交记录变得极为混乱。
   
   原理（变基时发生了什么）：
   
   1. 当我们发起变基时，git会首先找到两条分支的最近的共同祖先
   2. 对比当前分支相对于祖先的历史提交，并且将它们提取出来存储到一个临时文件中
   3. 将当前部分指向目标的基底
   4. 以当前基底开始，重新执行历史操作
   
   变基和merge对于合并分支来说最终的结果是一样的！但是变基会使得代码的提交记录更整洁更清晰！注意！大部分情况下合并和变基是可以互换的，但是如果分支已经提交给了远程仓库，那么这时尽量不要变基。
   
   ### 远程仓库（remote）
   
   目前我对于git所有操作都是在本地进行的。在开发中显然不能这样的，这时我们就需要一个远程的git仓库。远程的git仓库和本地的本质没有什么区别，不同点在于远程的仓库可以被多人同时访问使用，方便我们协同开发。在实际工作中，git的服务器通常由公司搭建内部使用或是购买一些公共的私有git服务器。我们学习阶段，直接使用一些开放的公共git仓库。目前我们常用的库有两个：GitHub和Gitee（码云）
   
   将本地库上传git：
   
   ```bash
   git remote add origin https://github.com/lilichao/git-demo.git
   # git remote add <remote name> <url>
   
   git branch -M main
   # 修改分支的名字的为main
   
   git push -u origin main
   # git push 将代码上传服务器上
   ```
   
   将本地库上传gitee：
   
   ```bash
   git remote add gitee https://gitee.com/ymhold/vue-course.git
   git push -u gitee main
   ```
   
   ### 远程库的操作的命令
   
   ```bash
   git remote # 列出当前的关联的远程库
   git remote add <远程库名> <url> # 关联远程仓库
   git remote remove <远程库名>  # 删除远程库
   git push -u <远程库名> <分支名> # 向远程库推送代码，并和当前分支关联
   git push <远程库> <本地分支>:<远程分支>
   git clone <url> # 从远程库下载代码
   
   git push # 如果本地的版本低于远程库，push默认是推不上去
   git fetch # 要想推送成功，必须先确保本地库和远程库的版本一致，fetch它会从远程仓库下载所有代码，但是它不会将代码和当前分支自动合并
   		 # 使用fetch拉取代码后，必须要手动对代码进行合并	
   git pull  # 从服务器上拉取代码并自动合并 
   
   ```
   
   注意：推送代码之前，一定要先从远程库中拉取最新的代码

	   ### 		tag 标签

- 当头指针没有执行某个分支的头部时，这种状态我们称为分离头指针（HEAD detached），分离头指针的状态下也可以操作操作代码，但是这些操作不会出现在任何的分支上，所以注意不要再分离头指针的状态下来操作仓库。

- 如果非得要回到后边的节点对代码进行操作，则可以选择创建分支后再操作

  ```bash
  git switch -c <分支名> <提交id>
  ```

- 可以为提交记录设置标签，设置标签以后，可以通过标签快速的识别出不同的开发节点：

  ```bash
  git tag
  git tag 版本
  git tag 版本 提交id
  git push 远程仓库 标签名
  git push 远程仓库 --tags
  git tag -d 标签名 # 删除标签
  git push 远程仓库 --delete 标签名 # 删除远程标签
  ```

  ### gitignore

- 默认情况下，git会监视项目中所有内容，但是有些内容比如node_modules目录中的内容，我们不希望它被git所管理。我们可以在项目目录中添加一个`.gitignore`文件，来设置那些需要git忽略的文件。

### 	github的静态页面

- 在github中，可以将自己的静态页面之间部署到github中，它会给我们提供一个地址使得我们的页面变成一个真正的网站，可以供用户访问。
- 要求：
  - 静态页面的分支必须叫做：gh-pages
  - 如果希望页面可以通过xxx.github.io访问，则需要将库的名字配置为xxx.github.io 

### 	docusaurus

- facebook推出的开源的静态的内容管理系统，通过它可以快速的部署一个静态网站

- 使用：

  - 网址：

    - https://docusaurus.io/

  - 安装

    - `npx create-docusaurus@latest my-website classic`

  - 启动项目

    - `npm start`或`yarn start`

  - 构建项目

    - `npm run build`或`yarn build`
    - 

  - 配置项目：

    - docusaurus.config.js 项目的配置文件

  - 添加页面：

    - 在docusaurus框架中，页面分成三种：1.page，2.blog，3.doc

  - 案例地址：

    - https://github.com/lilichao/lilichao.github.io

    

## 构建工具

- 当我们习惯了在node中编写代码的方式后，在回到前端编写html、css、js这些东西会感觉到各种的不便。比如：不能放心的使用模块化规范（浏览器兼容性问题）、即使可以使用模块化规范也会面临模块过多时的加载问题。
- 我们就迫切的希望有一款工具可以对代码进行打包，将多个模块打包成一个文件。这样一来即解决了兼容性问题，又解决了模块过多的问题。
- 构建工具就起到这样一个作用，通过构建工具可以将使用ESM规范编写的代码转换为旧的JS语法，这样可以使得所有的浏览器都可以支持代码。

### Webpack

- 使用步骤：
  1. 初始化项目`yarn init -y`
  2. 安装依赖`webpack`、`webpack-cli`
  3. 在项目中创建`src`目录，然后编写代码（index.js）
  4. 执行`yarn webpack`来对代码进行打包（打包后观察dist目录）

- 配置文件（webpack.config.js）

  ```javascript
  const path = require("path")
  module.exports = {
      mode: "production", 
      entry: "./src/index.js",
      output: {
      }, 
      module: {
          rules: [
              {
                  test: /\.css$/i,
                  use: ["style-loader", "css-loader"]
              }
          ]
      }
  }
  
  ```

- 在编写js代码时，经常需要使用一些js中的新特性，而新特性在旧的浏览器中兼容性并不好。此时就导致我们无法使用一些新的特性。

- 但是我们现在希望能够使用新的特性，我们可以采用折中的方案。依然使用新特性编写代码，但是代码编写完成时我们可以通过一些工具将新代码转换为旧代码。

- babel就是这样一个工具，可以将新的js语法转换为旧的js，以提高代码的兼容性。

- 我们如果希望在webpack支持babel，则需要向webpack中引入babel的loader

- 使用步骤

  1. 安装 `npm install -D babel-loader @babel/core @babel/preset-env`

  2. 配置：

     ```javascript
     module: {
       rules: [
         {
           test: /\.m?js$/,
           exclude: /(node_modules|bower_components)/,
           use: {
             loader: 'babel-loader',
             options: {
               presets: ['@babel/preset-env']
             }
           }
         }
       ]
     }
     ```

  3. 在package.json中设置兼容列表

     ```json
     "browserslist": [
             "defaults"
      ]
     ```

     https://github.com/browserslist/browserslist

- 插件（plugin）

  - 插件用来为webpack来扩展功能

  - html-webpack-plugin

    - 这个插件可以在打包代码后，自动在打包目录生成html页面

    - 使用步骤：

      1. 安装依赖
      2. 配置插件

      ```javascript
      plugins: [
              new HTMLPlugin({
                  // title: "Hello Webpack",
                  template: "./src/index.html"
              })
          ]
      ```

- 开发服务器（webpack-dev-server）

  - 安装：
    - `yarn add  -D webpack-dev-server`
    - 启动：`yarn webpack serve --open`

- `devtool:"inline-source-map"`配置源码的映射

## Vite

- Vite也是前端的构建工具

- 相较于webpack，vite采用了不同的运行方式：

  - 开发时，并不对代码打包，而是直接采用ESM的方式来运行项目
  - 在项目部署时，在对项目进行打包

- 除了速度外，vite使用起来也更加方便

- 基本使用：

  1. 安装开发依赖 vite

  2. vite的源码目录就是项目根目录

  3. 开发命令：

     vite 启动开发服务器

     vite build 打包代码

     vite preview 预览打包后代码

- 使用命令构建

  ```bash
  npm create vite@latest
  yarn create vite
  pnpm create vite
  ```

- 配置文件：`vite.config.js`

- 格式：

  ```javascript
  import { defineConfig } from "vite"
  import legacy from "@vitejs/plugin-legacy"
  
  export default defineConfig({
      plugins: [
          legacy({
              targets: ["defaults"]
          })
      ]
  })
  ```

  
