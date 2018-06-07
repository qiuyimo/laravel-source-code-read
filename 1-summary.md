## 框架的整体架构

我这里把 `/public/index.php` 和 `/bootstrap/app.php` 的源码整合到了一起. 去掉了与架构无关的内容, 方便理解和查看.

```php
require __DIR__.'/../vendor/autoload.php';

$app = new Illuminate\Foundation\Application(
    realpath(__DIR__.'/../')
);

$app->singleton(
    Illuminate\Contracts\Http\Kernel::class,
    App\Http\Kernel::class
);

$app->singleton(
    Illuminate\Contracts\Console\Kernel::class,
    App\Console\Kernel::class
);

$app->singleton(
    Illuminate\Contracts\Debug\ExceptionHandler::class,
    App\Exceptions\Handler::class
);

$kernel = $app->make(Illuminate\Contracts\Http\Kernel::class);

$request = Illuminate\Http\Request::capture();
$response = $kernel->handle($request);

$response->send();

$kernel->terminate($request, $response);

```


整体的结构就是这样, 先不用去管内部是怎么实现的. 先了解一下整体的架构.

1. composer 的自动加载, 现代化 php code 必备.
    * 怎么实现的自动加载?
2. 实例化了 `Illuminate\Foundation\Application`, 这个就是 `服务容器 (service container)`.
    * 什么是容器?
3. 绑定了 3 个 `interface` 到 `service container` 中. 分别是, 处理 http 请求接口, 处理命令行的接口, 异常处理接口.
    * 绑定是什么意思?
4. 通过 `service container`, 获取到实现 http 这个接口的类的实例. 注意, 第 3 步明明是把 http 这个接口绑定到了容器中.
    * 第 3 步绑定的是 `interface`, 为什么可以直接获取到实现了这个 `interface` 的类的实例呢?
5. 通过 `Illuminate\Http\Request::capture()` 来获取请求.
    * 如何实现的?
    * 返回的 `$request` 是什么结构?
6. 获取到了用于处理 http 请求的实例 `$kernel` 后, 调用 `handle` 方法处理 `http` 请求, 返回响应 `$response`.
    * 如何实现的?
    * 返回的响应 `$response` 是什么?
7. 通过字面意思, 就是发送请求啦.
    * 发送的是什么?
8. 结束处理.
    * 处理了那些东西?

带着这些问题. 接着了解源码就更有目的性了.
