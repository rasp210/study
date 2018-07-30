# Composer 包开发及常用命令

#### 一、包开发

假设已经安装了 composer。

##### 1. 在 [github](https://github.com/new) 上创建一个仓库

添加仓库名、描述，并勾选 `Initialize this repository with a README` ，然后点击 `Create repository` 即可。最后把远程仓库 clone 到本地。

##### 2. 创建 composer.json 文件

开发包之前最重要的一步是创建 `composer.json` ，它用来描述项目的依赖和其他元信息等。具体步骤如下：

---

$ `composer init`


  Welcome to the Composer config generator  
                                            


This command will guide you through creating your composer.json config.

Package name (<vendor>/<name>) [aaa/composer]: `test/xxx`
  
Description []: `Here is the description!`

Author [aaa <aaa@mail.com>, n to skip]: `author_test <author_test@email.com>` // *不允许下划线*

 Invalid author string.  Must be in the format: John Smith <john@example.com> 

Author [aaa <aaa@mail.com>, n to skip]: `testauthor <testauthor@email.com>`

Minimum Stability []: `dev` // *后面介绍*

Package Type (e.g. library, project, metapackage, composer-plugin) []: `library`

License []: `MIT`

Define your dependencies.

Would you like to define your dependencies (require) interactively [yes]? `yes`

Search for a package: `php`

Enter the version constraint to require (or leave blank to use the latest version): `7.1`

Search for a package: // *回车*

Would you like to define your dev dependencies (require-dev) interactively [yes]? `yes` // *输入yes,y,no,n中的一个*

Search for a package: `phpunit/phpunit`

Enter the version constraint to require (or leave blank to use the latest version): `7.0`

Search for a package:  // *回车*

{
    "name": "test/xxx",
    "description": "Here is the description!",
    "type": "library",
    "require": {
        "php": "7.1"
    },
    "require-dev": {
        "phpunit/phpunit": "7.0"
    },
    "license": "MIT",
    "authors": [
        {
            "name": "testauthor",
            "email": "testauthor@email.com"
        }
    ],
    "minimum-stability": "dev"
}

Do you confirm generation [yes]? `y`

---

通过以上步骤，将生成一个 `composer.json` 的文件，里面包括的 key 值有：require、require-dev、name、description、type、minimum-stability 等，下面一一介绍：

**require**

指定项目的依赖。require: {包名: 版本限制, ……}

```
"require": {
    "php": "^7.1",
    "monolog/monolog": "1.0.*"
}
```

**name**

包名。也是指 vendor 名和项目名，通常形式为以斜杠分隔的两个字符串，目的是避免命名冲突。

```
"name": "sunny-daisy/video",
```

**minimum-stability**

#### 二、常用命令

-Usage-

`command [options] [arguments]`

Options

`-h` or `--help` 使用帮助

`-q` or `--quiet` 不输出任何信息

`-V` or `--version` 展示版本

`--ansi` 强制 ANSI 输出

Available commands

`composer` or `composer list` 查看所有命令列表

`composer COMMAND --help` 列出 COMMAND 详细使用说明

#### 三、参考文献

[1.Github](https://github.com)

[2.Packagist 官网](https://packagist.org/)

[3.PHP 标准规范](https://psr.phphub.org/)







