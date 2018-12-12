# solidity 

1. 判断余额超过上限：

   1. > balances[addr] + _value < balances[addr] 
      >
      > // 原理是数字超过类型的上限时，进位后高位数据会被舍弃，剩余的有效位数字比之前更小。

2. 函数执行后，使用event进行记录

   1. > event属于solidity中的日志工具之一
      >
      > 可以在DAPP中进行监听

3. 使用安全计算函数

   1. > 其实就是在方法中增加回顾校验

4. solidity中，变量可以视为在同一个作用域中，所以局部变量/形参/全局变量（状态变量）不提倡重名

   1. > 通过为同名变量加前缀“_"是一种建议的用法

5. 判断一个地址是合约地址还是普通账户

   1. 使用web3.js

      > web3.eth.getCode()方法返回指定地址上代码的16进制字符串，由于普通账户地址处没有代码，因此将仅返回16进制前缀0x。利用这个我们可以进行判断，例如：

      ```web3
      var code = web3.eth.getCode("0xbfb2e296d9cf3e593e79981235aed29ab9984c0f")
      if(code === '0x') console.log('普通账户')
      else console.log('合约账户')
      ```

   2. 在solidity中实现

      > 在合约内，可以使用EVM汇编代码来获取指定地址处的代 码大小，显然，普通账户地址将返回0：

      ```solidity
      contract EzDemo {
          	function isContract(address addr) returns (bool) {
          		uint size;
          		assembly { size := extcodesize(addr) }
          		return size > 0;
        	}
      
      }
      ```

6. 

7. 

8. 

