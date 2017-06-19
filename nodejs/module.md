# MODULE

## commonjs规范

### 定义

[commonjs规范](http://wiki.commonjs.org/wiki/CommonJS)

先来说说commonjs规范，虽然它的名字后面带一个js的标识，但是它并不是一个js框架(库)文件。它是一种规范。它的出现主要是为了弥补ECMASCRIPT、BOM、DOM等规范缺失与不足的部分。
比如:

* 文件系统(fs)
* 网络传输(tcp, http)
* IO协议(Stream)
* Buffer
* Module

......

这些很大程度上补充了js先天性的不足。

### 模块规范

commonjs中对模块的查找定义了一套规则

* 查找目录下的package.json，如果有解析其中的main字段对应的文件
* 查找目录下的index.js -> index.node -> index.json

## nodejs的模块实现


### 导出

```js
// a.js
module.exports = 1
```

### 导入

```js
const a = require('a')
console.log(a) // 1
```

> 规范中定义使用require作为关键字来导入已定义的模块。传入参数是string类型的路径

#### nodejs中的实现规则

* 路径分析
* 文件定位
* 编译执行

> 这几个步骤是在第一次加载模块时会执行的步骤，加载过的模块会被缓存，再次被require时，是直接从缓存中读取。


#### 路径分析

* 核心模块

```txt
node源代码编译过程中已经编译成二进制代码的模块
```

* 相对/绝对路径模块

```txt
这种类型的模块就是加载一个文件模块来处理，require()方法会将其中的路径转成真实的路径的去索引，找到对应的模块编译执行。
```

* 自定义的模块

```txt
这类模块一般为第三方模块，通过npm安装在node_modules下
```

路径分析的规则如下:

1. 当前目录下的node_modules
1. 父目录的node_modules
1. 父目录的父目录的node_modules
1. ......
1. 根目录的node_modules

#### 文件定位

* 文件扩展名分析

```txt
require()在文件定位时，如果字符串中没有携带后缀名的文件，nodejs会会按照.js -> .node -> .json顺序自动添加后缀名尝试读取文件。
```

> 如果是其他扩展名，会被等同于.js文件导入

> 正常非.js文件，在require时会显式把后缀名带上，明确了文件的特殊性增加程序可读性，同时减少程序为了补齐后缀名尝试的损耗

* 目录分析和包

```txt
在添加所有扩展名都没有找到文件之后，匹配到对应的目录。此时nodejs会把这个模块当做一个包来出来。(按照commonjs规范来查找)
```

#### 模块编译

由于nodejs支持.js, .json, .node的加载模式，那么在编译时也是对应不同的编译方法


##### .js

根据规范的设计，让每个模块完全解耦，我们在编译.js文件时把代码头尾包裹起来

```js
(function(exports, require, module, __filename, __dirname){
    // your code
})
```

上面的包装完代码会被native的runInThisContext()方法执行抛出实际的模块


##### .node

这类模块都是由c/c++代码编写编译后产生的，nodejs会调用process.dlopen方法直接加载执行。

##### .json

.json文件，nodejs利用fs模块同步读取，之后调用JSON.parse去解析的。正常在导入一个.json文件时，会直接使用require来导入。

## nodejs模块调用解析

### javascript编写的核心模块

这种使用js编写的核心模块，其实最后也是会被变成C/C++来执行。这类模块在node源码中存放在/lib下

### C/C++编写的核心模块(内建模块)
nodejs虽然是可以通过javascript来编写，但是js语法上的特性决定了它无法想C/C++那样处理更加底层的系统调度逻辑，所以nodejs中有大量的底层模块是C/C++编写的。通常这部分我们叫做内建模块。源码存放在/src下

### 调度链路

![调用](http://studzw.github.io/nodejs/module.jpeg)

> 一般不推荐直接调用内建模块




















