# Composer 包开发及常用命令

## 一、包开发

假设已经安装了 composer。

### 1. 在 [github](https://github.com/new) 上创建一个仓库

添加仓库名、描述，并勾选 `Initialize this repository with a README` ，然后点击 `Create repository` 即可。最后把远程仓库 clone 到本地。

### 2. 创建 composer.json 文件

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

### 3. composer.json 内容说明
通过以上步骤，将生成一个 `composer.json` 的文件，里面包括的 key 值有：require、require-dev、name、description、type、minimum-stability 等，下面一一介绍：

**name**

包名。也是指 vendor 名和项目名，通常形式为以斜杠分隔的两个字符串，目的是避免命名冲突。

```
"name": "sunny-daisy/video",
```

- **description**
描述

- **type**
类型，可选值有：

- **license**

- **authors**

- **require**

指定项目的依赖。require: {包名: 版本限制, ……}

```
"require": {
    "php": "^7.1",
    "monolog/monolog": "1.0.*"
}
```

**minimum-stability**

## 二、常用命令
### Usage
`composer command [options] [arguments]`

### composer | composer list
查看所有命令列表

### composer show
列出包的信息，包括包名、版本、描述
```
justinrainbow/json-schema 5.2.7  A library to validate a json schema.
mk-j/php_xlsxwriter       0.37   PHP Library to write XLSX files
```

### composer <COMMAND> --help
列出 COMMAND 详细使用说明

## 三、参考文献

[1.Github](https://github.com)

[2.Packagist 官网](https://packagist.org/)

[3.PHP 标准规范](https://psr.phphub.org/)

## 四、附录
```
   ______
  / ____/___  ____ ___  ____  ____  ________  _____
 / /   / __ \/ __ `__ \/ __ \/ __ \/ ___/ _ \/ ___/
/ /___/ /_/ / / / / / / /_/ / /_/ (__  )  __/ /
\____/\____/_/ /_/ /_/ .___/\____/____/\___/_/
                    /_/
Composer version 1.6.5 2018-05-04 11:44:59

Usage:
  command [options] [arguments]

Options:
  -h, --help                     Display this help message
  -q, --quiet                    Do not output any message
  -V, --version                  Display this application version
      --ansi                     Force ANSI output
      --no-ansi                  Disable ANSI output
  -n, --no-interaction           Do not ask any interactive question
      --profile                  Display timing and memory usage information
      --no-plugins               Whether to disable plugins.
  -d, --working-dir=WORKING-DIR  If specified, use the given directory as working directory.
  -v|vv|vvv, --verbose           Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Available commands:
  about                Shows the short information about Composer.
  archive              Creates an archive of this composer package.
  browse               Opens the package's repository URL or homepage in your browser.
  check-platform-reqs  Check that platform requirements are satisfied.
  clear-cache          Clears composer's internal package cache.
  clearcache           Clears composer's internal package cache.
  config               Sets config options.
  create-project       Creates new project from a package into given directory.
  depends              Shows which packages cause the given package to be installed.
  diagnose             Diagnoses the system to identify common errors.
  dump-autoload        Dumps the autoloader.
  dumpautoload         Dumps the autoloader.
  exec                 Executes a vendored binary/script.
  global               Allows running commands in the global composer dir ($COMPOSER_HOME).
  help                 Displays help for a command
  home                 Opens the package's repository URL or homepage in your browser.
  info                 Shows information about packages.
  init                 Creates a basic composer.json file in current directory.
  install              Installs the project dependencies from the composer.lock file if present, or falls back on the composer.json.
  licenses             Shows information about licenses of dependencies.
  list                 Lists commands
  outdated             Shows a list of installed packages that have updates available, including their latest version.
  prohibits            Shows which packages prevent the given package from being installed.
  remove               Removes a package from the require or require-dev.
  require              Adds required packages to your composer.json and installs them.
  run-script           Runs the scripts defined in composer.json.
  search               Searches for packages.
  self-update          Updates composer.phar to the latest version.
  selfupdate           Updates composer.phar to the latest version.
  show                 Shows information about packages.
  status               Shows a list of locally modified packages, for packages installed from source.
  suggests             Shows package suggestions.
  update               Upgrades your dependencies to the latest version according to composer.json, and updates the composer.lock file.
  upgrade              Upgrades your dependencies to the latest version according to composer.json, and updates the composer.lock file.
  validate             Validates a composer.json and composer.lock.
  why                  Shows which packages cause the given package to be installed.
  why-not              Shows which packages prevent the given package from being installed.

