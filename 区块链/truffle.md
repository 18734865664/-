# truffle

> solidity开发框架

## 1. 环境初始化

> ```text
> truffle init     // 必须在一个空目录中执行
> 
> // 流程：
> // init --> compile --> migrate
> ```

执行初始化后，有下面文件存在：

- app/ - 你的应用文件运行的默认目录。这里面包括推荐的javascript文件和css样式文件目录，但你可以完全决定如何使用这些目录。
- contract/ - Truffle默认的合约文件存放地址。
- migrations/ - 存放发布脚本文件
- test/ - 用来测试应用和合约的测试文件
- truffle.js - Truffle的配置文件

## 2. 编译合约

> truffle compile      // 在项目根目录执行
>
> truffle  compile  --compile-all     // 默认只编译上次编译后变更的文件，加参数后强制全部重新编译
>
> 编译获得的json文件位于  build/contracts/

