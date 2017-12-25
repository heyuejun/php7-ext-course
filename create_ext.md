## 新建新扩展
这里新建一个名字叫test扩展

### PHP7代码项目目录结构
```
TSRM
Zend
appveyor
build
ext
main
pear
sapi
scripts
tests
travis
win32
...
```

### 创建扩展工具
```
# cd ext
# ./ext_skel
./ext_skel --extname=module [--proto=file] [--stubs=file] [--xml[=file]]
           [--skel=dir] [--full-xml] [--no-help]

  --extname=module   module is the name of your extension
  --proto=file       file contains prototypes of functions to create
  --stubs=file       generate only function stubs in file
  --xml              generate xml documentation to be added to phpdoc-svn
  --skel=dir         path to the skeleton directory
  --full-xml         generate xml documentation for a self-contained extension
                     (not yet implemented)
  --no-help          don't try to be nice and create comments in the code
                     and helper functions to test if the module compiled
```

### 创建扩展目录
```
# ./ext_skel --extname=test
// 创建之后在ext目录下生成一个项目目录结构
CREDITS
EXPERIMENTAL
test.c
test.php
README.md
config.m4
config.w32
php_test.h
tests
```

### 修改config.m4
```
del PHP_ARG_ENABLE(test, whether to enable test support,
del Make sure that the comment is aligned:
del [  --enable-test           Enable test support])

改为

PHP_ARG_ENABLE(test, whether to enable test support,
[  --enable-test           Enable test support])

注意: del 是config.m4文件的注释符号，就好比C语言中的//注释符号
```

### 修改 test.c
```
PHP_FUNCTION(confirm_php_hello_compiled)
{
	php_printf("hello world!\n");
	return;
}
```

### 编译扩展
```
cd ext/test
/usr/local/php/bin/phpize
./configure --with-php-config=/usr/local/php/bin/php-config
make
make install  
```

### php.ini配置文件中加载扩展的动态链接库
```
vim php.ini
添加: extension=test.so
```

### 检测扩展是否加载成功
```
# php -m | grep test
```

### 运行测试
```
# php -r 'confirm_php_hello_compiled()'
```