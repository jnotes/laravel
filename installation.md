# 安装

## 服务器要求

Laravel 对系统有一些要求。当然，所有这些要求 Laravel Homestead 虚拟机都能满足，因此强烈推荐你使用 Homestead 最为你的开发环境。

当然，假如你不使用 Homestead，请确保你的服务器满足以下要求：

- PHP >= 7.1.3
- OpenSSL PHP Extension
- PDO PHP Extension
- Mbstring PHP Extension
- Tokenizer PHP Extension
- XML PHP Extension
- Ctype PHP Extension
- JSON PHP Extension
- BCMath PHP Extension

## 创建Laravel项目

> `Laravel`利用`Composer`来管理其依赖,因此在使用`Laravel`之前，请确保在您的计算机中安装了`Composer`。

### 通过`Larvel Installer`创建项目

首页，使用`Composer`下载`Laravel Installer`

```bash
>$ composer global require laravel/installer
```
确保将`composer`的`vendor\bin`目录放置在你的系统环境变量`$PATH`中，以便系统可以找到`Laravel`的可执行文件。该目录根据你的操作系统存在不同的位置中；一些常见的配置包括 :

- macOS: `$HOME/.composer/vendor/bin`
- GNU / Linux 发行版: `$HOME/.config/composer/vendor/bin`
- Windows: `%USERPROFILE%\AppData\Roaming\Composer\vendor\bin`

安装完成后，`laravel new` 命令会在你指定的目录创建一个全新的 `Laravel` 项目。例如， `laravel new blog` 将会创建一个名为 `blog` 的目录，并已安装好 Laravel 所有的依赖项:

```bash
>$ laravel new blog
```

### 通过`Composer Create-Project`创建项目

你也可以在终端中运行 `create-project` 命令来安装 Laravel：

```bash
>$ composer create-project --prefer-dist laravel/laravel blog
```

## 本地开发服务器

如果你在本地安装了 PHP， 并且你想使用 PHP 内置的服务器来为你的应用程序提供服务，则可以使用 `Artisan` 命令 `serve`。该命令会在 `http://localhost:8000` 上启动开发服务器:

```bash
>$ php artisan serve
```

当然，最好还是选择 Homestead 和 Valet。

## 配置

### `public`目录

安装完 `Laravel` 之后，你应该配置你的 web 服务的文档目录指向 `public` 路径。该路径下的 `index.php` 文件作为进入应用的所有 HTTP 请求的前端控制器。

### 配置文件

`Laravel` 框架的所有配置文件存放在 `config` 目录下。每个选项都有文档标注，便于通过文件查看并熟悉对你有用的选项。

### 目录权限

在安装 `Laravel` 后，你可能需要配置一些权限。 `storage` 和 `bootstrap/cache` 目录在你的 web 服务下应该是可写的权限，否则 `Laravel` 将无法运行。如果你用的是 Homestead 虚拟机，这些权限应该已经设置好了。

### 应用密钥

安装好 `Laravel` 之后的下一步是设置你的应用秘钥为随机字符串。如果你通过 `composer` 或者 `Laravel` 安装器安装的，这个秘钥已经通过 `php artisan key:generate` 命令为你设置好了。

通常，这个字符串应该是 32 个字符长度。这个秘钥将会设置在环境变量文件 `.env` 中。如果你还没有将 `.env.example` 文件重命名为 `.env` 文件，你需要将 `.env.example` 文件重命名为 `.env` 文件。

> 如果应用秘钥还没有设置，你的用户会话和其他的加密数据将会不安全！

### 附加配置

`Laravel` 几乎不需要除上面所说的其他什么配置了。你可以随心所欲的开始开发了！然而，你可能会想要再次查看 `config/app.php` 文件和它的注释说明。它包含一些你可能希望根据你应用来更改的选项，诸如： `timezone` 和 `locale` 。

你还可能想要配置 Laravel 的其他的一些组件，例如：

- Cache
- Database
- Session

## Web服务器配置

### 优雅链接

#### Apache

`Laravel` 中包含了一个 `public/.htaccess` 文件通常用于在资源路径中隐藏 `index.php` 的前端控制器。在用 `Apache` 为 `Laravel` 提供服务之前，确保启用了 `mod_write` 模块，这样 `.htaccess` 文件才能被服务器解析。

如果 `Laravel` 附带的 `.htaccess` 文件不起作用，尝试下面的方法替代：

```
Options +FollowSymLinks -Indexes
RewriteEngine On

RewriteCond %{HTTP:Authorization} .
RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]

RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ index.php [L]
```

#### Nginx

如果你使用 `Nginx` ，在你的站点配置中加入以下配置，所有的请求将会引导至 `index.php` 前端控制器。

```
location / {
     try_files $uri $uri/ /index.php?$query_string;
}
```

当你使用 Homestead 或者 Valet 时，优雅链接将会自动配置好。
