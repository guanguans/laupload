<p align="center">
    <a href="https://packagist.org/packages/guanguans/laupload"><img src="https://img.shields.io/packagist/v/guanguans/laupload.svg" alt="Version"></a>
    <a href="https://packagist.org/packages/guanguans/laupload"><img src="https://img.shields.io/packagist/l/guanguans/laupload.svg" alt="License"></a>
    <a href="https://packagist.org/packages/guanguans/laupload"><img src="https://img.shields.io/packagist/php-v/guanguans/laupload.svg" alt="PHP version"></a>
    <a href="https://github.com/guanguans/laupload/tags"><img src="https://img.shields.io/github/tag/guanguans/laupload.svg" alt="GitHub tag"></a>
</p>

# 一个 laravel 的上传组件

<div align="center"><img src="./docs/demo.gif"/></div>

## 安装

``` sh
composer global require fxp/composer-asset-plugin
composer require guanguans/laupload
```

## 使用

### 注册服务

`config/app.php` 中：

``` php
'providers' => [
    ...
    guanguans\laupload\LauploadServiceProvider::class,
],
```

或者 `app/Providers/AppServiceProvider.php` 中：

``` php
...
public function register()
{
    $this->app->register('guanguans\laupload\LauploadServiceProvider');
}
```

``` sh
php artisan vendor:publish --provider="guanguans\laupload\LauploadServiceProvider"
```

### 配置

``` php
return [
    ...
    'uploadStore'  => 'uploadStore', // 上传文件路由
    'uploadFileList' => 'uploadFileList', // 获取文件列表路由
];
```

### 视图

``` html
<div class="col-md-4">
    <!----------文件视图----------->
    {!! laupload_widget('file') !!}
    <!----------单图视图----------->
    {!! laupload_widget('oneImage') !!}
    <!----------多图视图----------->
    {!! laupload_widget('muiImage') !!}
    <!----------移动端视图--------->
    {!! laupload_widget('app') !!}
</div>
```

### 后台

``` php
use guanguans\laupload\Laupload;
...
public function store()
{
    $upload = new Laupload();
    $upload->savePath = storage_path('app/public/laupload') . '/'; // 上传根目录
    if ($upload->upload()) {
        // 成功时返回数据 message 为文件地址
        $data = ['valid' => 1, 'message' => asset('storage/app/public/laupload/'.$upload->getUploadFileInfo()[0]['savename'])];
    } else {
        // 失败时返回数据 message 为失败原因
        $data = ['valid' => 0, 'message' => "后台提示:" . $upload->getErrorMsg()];
    }

    return $data;
}
```

## License

[MIT](./LICENSE)

