# 安装

- [安装Composer](#install-composer)
- [安装Laravel](#install-laravel)
- [服务器要求](#server-requirements)
- [配置](#configuration)
- [友好的URL](#pretty-urls)

<a name="install-composer"></a>
## 安装Composer

Laravel使用[Composer](http://getcomposer.org)来管理它的依赖。首先，下载`composer.phar`。得到PHAR文件后，你可以将它放在你的项目文件夹中，你也可以将它移动到`usr/local/bin`，这样它就可以在你系统里全局使用。Windows下，你可以使用Composer的[Windows安装程序](https://getcomposer.org/Composer-Setup.exe)

<a name="install-laravel"></a>
## 安装Laravel

Composer安装后，下载Laravel框架的[最新版本](https://github.com/laravel/laravel/archive/develop.zip)，解压到你服务器的一个文件夹。然后，在你的Laravel应用的根目录，执行`php composer.phar install`命令安装所有框架的依赖。你的电脑需要安装有Git来完成这个安装过程。

<a name="server-requirements"></a>
## 服务器要求

Laravel框架有一些系统要求：

- PHP >= 5.3.7
- MCrypt PHP扩展

<a name="configuration"></a>
## 配置

laravel基本不用配置，可以很轻松地开始开发。但是，你最好看一遍`app/config/app.php`文件，包括里面的注释，它包含了一些根据你的项目你可能会改变的一些配置，如`timezone`和`locale`。

> **注意：**在`app/config/app.php`中你应该保证设置好的配置是`key`，这个配置应该被设置为32位长度的随机字符串。key用来对数据加密，如果key没有设置，所有被加密的数据都是不安全的。你可以通过`php artisan key:generate`快速地设置这个值。

<a name="permissions"></a>
### 权限
Laravel要求一些权限的配置 - 在app/storage里面的文件夹要有被服务器写的权限。

<a name="paths"></a>
### 路径

框架中一些文件夹的路径可以在`boostrap/paths.php`文件中配置。

> **注意：**Laravel设计来保护你的程序代码，只有必须公开访问的文件才被放在public文件夹中。推荐你让public文件夹作为你网站根目录，Laravel其它文件放在网站根目录以外。

<a name="pretty-urls"></a>
## 友好的URL

框架中包含`public/.htaccess`文件，用来让URL去掉`index.php`。如果你的服务器是Apache，请确保开启了`mod_rewrite`模块。

如果`.htaccess`文件在你的Apache下不生效，可以试下改成：

	Options +FollowSymLinks
	RewriteEngine On

	RewriteCond %{REQUEST_FILENAME} !-d
	RewriteRule ^(.+)/$ http://%{HTTP_HOST}/$1 [R=301,L]

	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteRule ^ index.php [L]
