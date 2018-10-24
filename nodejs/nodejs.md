# nodejs

## 1. 简介

> nodejs 不是一门独立的语言，而是一个让javascript运行在服务端的开发平台，不是类库，类似于jvm，就是一个平台。
>
> 是一个构建于chrome v8javascript引擎之上的一个javascript运行时。
>
> 专注于实时web应用
>
> nodejs中运行的代码都是js语句
>
> nodejs 生而异步，所有的代码执行默认都是异步执行

## 2. 语法

> nodejs语法上使用js语法，支持es6语法，区别是：
>
> 1. nodejs 中没有DOM/BOM
>
> 2. nodejs中没有css样式
> 3. 提供了一些标准包（http/fs等）
> 4. 还提供了一些全局API（服务端能力相关）
>    1. require
>       1. 引包，类似于import
>       2. 两个作用
>          1. 会执行被引用文件中的代码
>          2. 返回一个模块对象，使用该对象操作模块中的变量/方法
>             1. 要对外开放的变量，需要使用module.exports.valName1 = valName2    //valName1和valName2可以相同，也可以不同
>             2. nodejs中每个js文件相当于在文件末尾有一句 return module.exports // 返回模板对象，module是默认的模板对象，module有一个成员对象exports(是一个引用类型)，然后每个模块都隐式的return该成员
>             3. 实际使用时，module可以省略，使用exports == module.exports
>       3. require同目录下模块“./”不可以省略
>       4. 导出单一成员时：
>          1. modole.exports = 123     // 后面接的可以是变量，也可以是方法
>          2. exports = 123    // 这种写法是错误的，因为在模块中本身隐式的有一个exports = modole.exports 所以，这种写法不可取
>    2. Class: Buffer
>    3. process         --    (进程对象)
>
> node中没有全局作用域，只有模块作用域，即变量只在模块内有效

### 2.1 引包

> require关键字，类似于import
>
> const http = require("http")

### 2.2 文件读写

> const fs = require("fs")

### 2.2.1 读文件

1. fs.readFile(filepath, callbackFunc)

### 2.2.2 写文件

1. fs.writeFile(filepath, data, callbackFunc)

#### 2.2.3 API表格

> 这里的path建议使用path.join() 结合__dirname，避免路径出现问题

| API                                         | 作用              | 备注           |
| ------------------------------------------- | :---------------- | :------------- |
| fs.access(path, callback)                   | 判断路径是否存在  |                |
| fs.appendFile(file, data, callback)         | 向文件中追加内容  |                |
| fs.copyFile(src, callback)                  | 复制文件          |                |
| fs.mkdir(path, callback)                    | 创建目录          |                |
| fs.readDir(path, callback)                  | 读取目录列表      |                |
| fs.rename(oldPath, newPath, callback)       | 重命名文件/目录   |                |
| fs.rmdir(path, callback)                    | 删除目录          | 只能删除空目录 |
| fs.stat(path, callback)                     | 获取文件/目录信息 |                |
| fs.unlink(path, callback)                   | 删除文件          |                |
| fs.watch(filename[, options][, listener])   | 监视文件/目录     |                |
| fs.watchFile(filename[, options], listener) | 监视文件          |                |

### 2.3 数据加解密

> const crt = require("crypto")

### 2.4 http服务

> const http = require("http")

### 2.5 Sockt网络

### 2.6 全局成员



## 3. npm（Node Package Manager）

> npm 用于解决Node中第三方包共享问题，

### 3.1 两层含义

#### 3.1.1 npm网站

> npmjs.com ，提供了存放数据包的能力，不提供版本管理

#### 3.1.2 命令行工具

1. npm init  

   1. > 初始化一个package.json文件，表示用npm进行管理
      >
      > 可以使用npm init -y    跳过向导，快速生成

2. npm install

   1. >一次性安装dependencies中所有依赖项
      >
      >简写：npm i
      >
      >下载指定包：npm install pname
      >
      >下载指定版本：npm install pname@version
      >
      >下载全局包：npm install --global pname
      >
      >卸载指定包：npm uninstall pname 

3. npm view pname

   1. > 查看包信息

4. npm help/   npm 命令 --help

5. npm install --global  pname

   1. > 安装全局包：这类包不是require使用的，而是用来实现某种功能，类似git
      >
      > 简写 npm install -g pname

6. npm root

   1. > 查看npm的安装路径，
      >
      > npm root -g 查看 全局包的安装路径

7. 使用国内npm源

   1. > http://npm.taobao.org/
      >
      > eg：npm install jquery  --registry=https://registry.npm.taobao.org
      >
      > or   npm config set registry https://registry.npm.taobao.org
      >
      > 使用npm  config  list   查看配置信息

#### 3.2 依赖

> 保存代码时，一些第三方的类库，不需要进行保存
>
> nodejs项目中第三方包默认保存在项目路径的node_modules目录中
>
> 此外还有一个package.json 用于保存项目信息，其中有依赖的三方包信息（description字段），使用npm install可以下载对应的三方包，
>
> 此外还有package-lock.json，中存放所有包的信息（版本，下载地址），一方面方便下次下载，另一方面，防止npm 默认下载最新版本第三方包，造成依赖问题

> npm  install --save        // 会将信息保存到package-lock.json中的 dependencies依赖项中，--save是默认选项
>
> npm install --save-dev     // 会保存到devDependencies中
>
> npm  install --production  只安装dependencies依赖项中的包











