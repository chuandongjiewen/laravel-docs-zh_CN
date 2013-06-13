# 路由

- [基础](#basic-routing)
- [路由参数](#route-parameters)
- [路由过滤](#route-filters)
- [Named Routes](#named-routes)
- [Route Groups](#route-groups)
- [Sub-Domain Routing](#sub-domain-routing)
- [Route Prefixing](#route-prefixing)
- [Route Model Binding](#route-model-binding)
- [Throwing 404 Errors](#throwing-404-errors)
- [Resource Controllers](#resource-controllers)

<a name="basic-routing"></a>
## Basic Routing

应用程序的大部分路由是在 `app/routes.php` 文件里定义的。 而最简单的路由则是由一
个URI和回调函数组成的。

**GET请求方式的简单路由**

	Route::get('/', function()
	{
		return 'Hello World';
	});

**POST请求方式的简单路由**

	Route::post('foo/bar', function()
	{
		return 'Hello World';
	});

**为所有HTTP请求注册一个路由响应**

	Route::any('foo', function()
	{
		return 'Hello World';
	});

**使路由强制运行在HTTPS请求下**

	Route::get('foo', array('https', function()
	{
		return 'Must be over HTTPS';
	}));

通常，如果你需要为路由生成URI，则可以使用 `URL::to` 方法：

	$url = URL::to('foo');

<a name="route-parameters"></a>
## 路由参数

	Route::get('user/{id}', function($id)
	{
		return 'User '.$id;
	});

**可选的路由参数**

	Route::get('user/{name?}', function($name = null)
	{
		return $name;
	});

**带默认值的可选路由参数**

	Route::get('user/{name?}', function($name = 'John')
	{
		return $name;
	});

**用正则表达式约束路由**

	Route::get('user/{name}', function($name)
	{
		//
	})
	->where('name', '[A-Za-z]+');

	Route::get('user/{id}', function($id)
	{
		//
	})
	->where('id', '[0-9]+');

<a name="route-filters"></a>
## 路由过滤器

路由过滤器提供了一个路由访问限制的简单方法，这对我们网站中需要权限认证的网页很有
用。 在Laravel中，有几个过滤器，包括 `auth`过滤器，`auth.basic`过滤器，`guest`过
滤器，`crsf`过滤器。可以在 `app/filters` 文件中找到。

**定义路由过滤器**

	Route::filter('old', function()
	{
		if (Input::get('age') < 200)
		{
			return Redirect::to('home');
		}
	});

万一从一个过滤器中返回一个响应，那么这个响应将会被当成当前请求的响应，而这个路由
则不会被执行，任何在这个路由上的`after`过滤器也会被全部取消。

**为路由添加一个过滤器**

	Route::get('user', array('before' => 'old', function()
	{
		return 'You are over 200 years old!';
	}));

**为路由添加多个过滤器**

	Route::get('user', array('before' => 'auth|old', function()
	{
		return 'You are authenticated and over 200 years old!';
	}));

**指定路由参数**

	Route::filter('age', function($route, $request, $value)
	{
		//
	});

	Route::get('user', array('before' => 'age:200', function()
	{
		return 'Hello World';
	}));

After filters receive a `$response` as the third argument passed to the filter:

	Route::filter('log', function($route, $request, $response, $value)
	{
		//
	});

**基于模式的过滤器**

You may also specify that a filter applies to an entire set of routes based on their URI.
你也可以指定一个过滤器，应用在符合模式的URI的路由集。

	Route::filter('admin', function()
	{
		//
	});

	Route::when('admin/*', 'admin');

In the example above, the `admin` filter would be applied to all routes beginning with `admin/`. The asterisk is used as a wildcard, and will match any combination of characters.
在上面的例子中，`admin`过滤器将会被应用在所有以`admin`开头的路由上。 *号作为通配
符使用，匹配所有字符的组合。

**过滤器类**

For advanced filtering, you may wish to use a class instead of a Closure. Since filter classes are resolved out of the application [IoC container](/docs/ioc), you will be able to utilize dependency injection in these filters for greater testability.

**Defining A Filter Class**

	class FooFilter {

		public function filter()
		{
			// Filter logic...
		}

	}

**Registering A Class Based Filter**

	Route::filter('foo', 'FooFilter');

<a name="named-routes"></a>
## Named Routes

Named routes make referring to routes when generating redirects or URLs more convenient. You may specify a name for a route like so:

	Route::get('user/profile', array('as' => 'profile', function()
	{
		//
	}));

You may also specify route names for controller actions:

	Route::get('user/profile', array('as' => 'profile', 'uses' => 'UserController@showProfile'));

Now, you may use the route's name when generating URLs or redirects:

	$url = URL::route('profile');

	$redirect = Redirect::route('profile');

You may access the name of a route that is running via the `currentRouteName` method:

	$name = Route::currentRouteName();

<a name="route-groups"></a>
## Route Groups

Sometimes you may need to apply filters to a group of routes. Instead of specifying the filter on each route, you may use a route group:

	Route::group(array('before' => 'auth'), function()
	{
		Route::get('/', function()
		{
			// Has Auth Filter
		});

		Route::get('user/profile', function()
		{
			// Has Auth Filter
		});
	});

<a name="sub-domain-routing"></a>
## Sub-Domain Routing

Laravel routes are also able to handle wildcard sub-domains, and pass you wildcard parameters from the domain:

**Registering Sub-Domain Routes**

	Route::group(array('domain' => '{account}.myapp.com'), function()
	{

		Route::get('user/{id}', function($account, $id)
		{
			//
		});

	});
<a name="route-prefixing"></a>
## Route Prefixing

A group of routes may be prefixed by using the `prefix` option in the attributes array of a group:

**Prefixing Grouped Routes**

	Route::group(array('prefix' => 'admin'), function()
	{

		Route::get('user', function()
		{
			//
		});

	});

<a name="route-model-binding"></a>
## Route Model Binding

Model binding provides a convenient way to inject model instances into your routes. For example, instead of injecting a user's ID, you can inject the entire User model instance that matches the given ID. First, use the `Route::model` method to specify the model that should be used for a given parameter:

**Binding A Parameter To A Model**

	Route::model('user', 'User');

Next, define a route that contains a `{user}` parameter:

	Route::get('profile/{user}', function(User $user)
	{
		//
	});

Since we have bound the `{user}` parameter to the `User` model, a `User` instance will be injected into the route. So, for example, a request to `profile/1` will inject the `User` instance which has an ID of 1.

> **Note:** If a matching model instance is not found in the database, a 404 error will be thrown.

If you wish to specify your own "not found" behavior, you may pass a Closure as the third argument to the `model` method:

	Route::model('user', 'User', function()
	{
		throw new NotFoundException;
	});

Sometimes you may wish to use your own resolver for route parameters. Simply use the `Route::bind` method:

	Route::bind('user', function($value, $route)
	{
		return User::where('name', $value)->first();
	});

<a name="throwing-404-errors"></a>
## Throwing 404 Errors

There are two ways to manually trigger a 404 error from a route. First, you may use the `App::abort` method:

	App::abort(404);

Second, you may throw an instance of `Symfony\Component\HttpKernel\Exception\NotFoundHttpException`.

More information on handling 404 exceptions and using custom responses for these errors may be found in the [errors](/docs/errors#handling-404-errors) section of the documentation.

<a name="resource-controllers"></a>
## Resource Controllers

Resource controllers make it easier to build RESTful controllers around resources. 

See [Controllers](/docs/controllers#resource-controllers) documentation for more information.
