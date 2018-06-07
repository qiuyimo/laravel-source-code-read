## 服务容器

接着上篇文章, 来看一下.

```php
$app = new Illuminate\Foundation\Application(
    realpath(__DIR__.'/../')
);
```

这个类的位置. 可以看到, 这个是核心框架的类. 代码太长, 这里就不展示了. 

`/laravel/vendor/laravel/framework/src/Illuminate/Foundation/Application.php`

这个就是实例化了 `service container`. 

```php
<?php

namespace Illuminate\Foundation;

// ...
use Symfony\Component\HttpKernel\HttpKernelInterface;
use Illuminate\Contracts\Foundation\Application as ApplicationContract;

class Application extends Container implements ApplicationContract, HttpKernelInterface
{
    // ...
}
```

通过这个简略版的代码, 可以看到, 这个类有父类, 并且实现了 2 个接口. 这里整理一下, 方便查阅. 



