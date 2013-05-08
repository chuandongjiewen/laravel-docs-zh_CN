# 配置

- [介绍](#introduction)
- [环境配置](#environment-configuration)
- [维护模式](#maintenance-mode)

<a name="introduction"></a>
## 介绍

Laravel框架所有的配置文件存储在`app/config`文件夹中。所有配置文件中的选项都有注释，建议把所有的配置文件都过一遍，熟悉里面的各项配置。

有时候你可能需要在运行时获取配置，可以使用`Config`类：

**获取一个配置的值**

	Config::get('app.timezone');

注意 "." 语法用来访问不同配置文件中的值，你也可以在运行时设置配置的值：

**设置一个配置的值**

	Config::set('database.default', 'sqlite');

<a name="environment-configuration"></a>
## 环境配置

通常我们需要在程序运行的不同环境中使用不同的配置。例如，你可能希望在本地开发环境和生产环境中使用不同的缓存驱动器。通过基于环境的配置可以很容易地满足这个需求。

简单地在`config`文件夹中新建一个文件夹，取名为环境名，如`local`，然后，新建一个你希望覆盖的配置文件。例如，在local环境中设置不同的缓存驱动器配置，你应该在`app/config/local`文件夹中新建一个`cache.php`文件，内容如下：

	<?php

	return array(

		'driver' => 'file',

	);

> **注意：**不要使用'testing'作为环境名称，这个名称用于单元测试。

注意，你不必在新环境中设置所有在基础配置文件中的配置项，只需要设置你需要覆盖的配置项。环境配置的优先级高于基础配置文件。

接下来，我们需要告诉框架它所运行的环境。默认的环境是`production`环境，但你可以在`bootstrap/start.php`文件中设置其它环境。在这个文件中你可以看到一个`$app->detectEnvironment`的调用，传递到这个方法的数组是用来决定当前环境的，你可以在需要的时候添加其它环境和机器名到这个数组。

    <?php

    $env = $app->detectEnvironment(array(

        'local' => array('your-machine-name'),

    ));

你也可以传递一个`闭包`到`detectEnvironment`方法，它可以使用你设定的环境。

	$env = $app->detectEnvironment(function()
	{
		return $_SERVER['MY_LARAVEL_ENV'];
	});

你可以通过`environment`方法访问当前程序环境。

**获取当前程序环境**

	$environment = App::environment();

<a name="maintenance-mode"></a>
## 维护模式

当你的应用处于维护模式，所有的路由都会转发到一个自定义的页面，这让你很容易在应用升级的时候关闭应用。在`app/start/global.php`文件中已经存在一个对`App::down`方法的调用，在应用处于维护模式的时候这个方法会向用户发送一个响应。

开启维护模式，只需简单地执行`down`Artisan命令

	php artisan down

关闭维护模式，使用`up`命令

	php artisan up
