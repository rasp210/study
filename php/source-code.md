# PHP 源码   
### 目录   
---
- appveyor   
- build 目录   
>与源码编译相关的文件。如环境检测脚本   
- ext 目录   
>官方扩展目录。包括绝大多数的PHP函数的定义和实现，如array系列、pdo系列、spl系列。个人写的扩展在测试时候也可以放到这个目录下，方便测试盒调试   
- main 目录   
>PHP核心代码文件目录。主要实现PHP的基本设施   
- pear 目录   
>PHP扩展与应用仓库。包含PEAR的核心文件   
- sapi 目录   
>包含各种服务器抽象层的代码，如apache的mod_php、cgi、fastcgi、fpm等接口   
- scripts   
- tests 目录   
>测试集目录   
- travis   
- TSRM 目录   
>TSRM(Thread Safe Resource Manager)线程安全资源管理器   
- win32   
- Zend   
- CODING_STANDARDS 文件   
>要写PHP扩展的话，需要读一读   

