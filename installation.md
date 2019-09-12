# installation
* [Requirements](#requirements)
* [Installing Dependencies](#installing-dependencies)
* [Larakits Installer](#installer)

## [Requirements](#requirements)
Larakits has a few requirements. The requirements are listed below:

* Composer
* Laravel Installer
* Laravel Framework 6.0+
* Laravel Mix
* Bootstrap 4
* ReactJS 
* NodeJS & NPM

Larakits uses ReactJS for the settings and lighthouse pages to show interactive UI. You need ReactJS for those pages (it’s a must). For rest of the pages of your application, you can use whatever JavaScript framework or library you want.

## [Installing Dependencies](#installing-dependencies)

### Installing Laravel Installer
You can install Laravel installer via [Composer](https://getcomposer.org).  First [download](https://getcomposer.org/download) and install composer on your machine. Then run the command on your terminal to install `Laravel Installer`:

```
composer global require laravel/installer
```

### Installing NodeJS
Larakits uses `Laravel Mix` for compiling sass and javascript assets. To install `Laravel Mix` you may need `NodeJS` on your machine.
You can download NodeJS from [nodejs.org](https://nodejs.org).


## [Larakits Installer](#installer)
### Installing Larakits Installer 
The Larakits installer will create a new Laravel application and install Larakits inside your Laravel application in the directory of your choice. To install Larakits installer you must install Composer on your machine. After that you have to install Larakits installer globally by using Composer:

```
composer global require larakits/installer
```

### Registering API Token
Before starting new project you must register your API token with the installer. You can create your API token from [API settings page](https://larakits.com/settings/api). After that you have to register your API token using `register` command of Larakits installer:

```
larakits regiser YOUR_API_TOKEN
```

### Creating New Project
If you have completed your API token registering process, you can use `new` command for creating new project with Larakits installer:

```
larakits new YOUR_PROJECT_NAME
```

The Larakits new command will create a fresh Laravel application with Larakits package inside `YOUR_PROJECT_NAME` directory. `YOUR_PROJECT_NAME` will be your project name or website name.

> After new project creation is done. You have to use your own `database` credential on `.env` file. Then you have to run `php artisan migrate` to migrate all migration files that comes with Larakits.  

If everything goes well, the next step is [configuring billing](/docs/{version}/billing) for your application.