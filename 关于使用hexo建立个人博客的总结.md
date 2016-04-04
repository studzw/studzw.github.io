# 关于使用hexo建立个人博客的总结

[hexo](http://hexo.io/)是一个基于Node.js的静态博客程序，可以将其托管到github,heroku等上面。其中可以在线上找到很多不错的theme,当然也可以自己定制开发一套。

## 1、准备工作

1. 安装Node.js和NPM
2. 安装Git
3. 配置Github
4. 安装Hexo
5. 部署到github
6. 整个过程中的问题

## 2、Node.js和NPM安装
 
### 2.1、直接在Node.js[官网](https://nodejs.org/download/)下载.pkg直接安装。

安装完成可通过命令查看是否安装成功

``` bash
$ node --version
v0.12.2
$ npm --version
v2.7.0
```

### 2.2、通过n或者nvm来安装管理Nodej.s

由于Node.js版本更新较快，且io.js的出现针对不同版本的管理很重要。所以流行使用nvm或n来管理。前者是Node的一个模块后者是通过shell脚本实现的，2两者具体区别可以参照[这里](http://it.taocms.org/03/3079.htm)

* 通过n安装管理Node.js

*PS: 这里很有意思的是：安装n一般都是通过npm直接安装，但是npm的安装又是要基于node的运行，那么如果想通过n来安装管理node,还是要先按照 __2.1__ 方法安装node和npm后再安装n*

``` bash
$ sudo npm install -g n
// 查看已安装版本和正在使用的版本
$ n
//安装指定版本，建议先去node官网查找对应的版本列表(http://nodejs.org/dist)
$ n 0.10.11
```

* 通过nvm安装管理Node.js

使用mac OSX系统建议先安装[Homebrew](http://brew.sh/)，通过brew来管理nvm。

``` bash
// 安装brew
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
// 安装nvm
$ brew install nvm
// 安装node
$ nvm install node 0.12.2
// 运行指定的版本
$ nvm use 0.12.2
-> v0.12.2
// 安装npm，注意npm安装需要依赖node的运行
$ brew install npm
```

> * 如果想直接安装nvm和npm。可以参照各自在github上面的介绍 》[这是nvm地址](https://github.com/creationix/nvm)、[这是npm地址](https://github.com/npm/npm)
 * Homebrew是Mac OSX一个比较好的脚本插件管理器，也是最常用的管理器

## 3、Git的安装

### 3.1、直接通过[Git官网](http://git-scm.com/)下载包安装

### 3.2、通过brew安装

```bash
$ brew install git
```

*PS: 关于git的使用以及原理介绍可以通过一些资料查找。这里提供几个[git原理](http://www-cs-students.stanford.edu/~blynn/gitmagic/intl/zh_cn/)、[git常用方法](http://www.bootcss.com/p/git-guide/)*

## 4、配置GitHub

使用Github Page搭建博客, 需要遵循一定的规则, 需要在github建立仓库,仓库名为 __username.github.io__, 更多详情请参考[官方文档](https://pages.github.com/)

## 5、Hexo安装

``` bash
$ npm install hexo-cli -g
```

Hexo使用命令：

``` bash
// 在本地创建blog(项目目录)
$ hexo init blog
// 开启本地服务，访问地址：localhost:port
$ hexo server
// 自动根据当前目录下文件,生成静态网页
$ hexo generate
```

添加文章:

``` bash
$ hexo new "artName"
```

*PS：关于Hexo具体功能与操作详见[官方API](http://hexo.io/api/)*

## 6、部署到GitHub

``` bash
// 修改根目录下./_config.yml
$ vi _config.yml
deploy:
  type: git
  repository: git@github.com:username/username.github.io.git  #部署的仓库的SSH
  branch: master   #部署分支,一般使用master主分支
// 开始部署
$ hexo deploy
```

*PS：也可以使用https方式repository。<code>github.com/username/username.github.io.git</code>*

## 7、部署过程中遇到的问题

* Deployer not found: undefined

	* 请检查你_config.yml文件编码，需要修改成UTF-8
	* 注意_config.yml文件中deploy的type格式，冒号后面需要有一个空格
* Deployer not found: git或者是github

	* 由于hexo更新到3.0之后，deploy的type值需要改成git

	```bash
	// 修改完文件之后执行
	$ npm install hexo-deployer-git --save 
	```
