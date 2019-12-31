laminas-development-mode
===================

This Laminas "development mode" module allows you to specify configuration and
modules that should only be enabled when in development, and not when in
production.

Requirements
------------
  
Please see the [composer.json](composer.json) file.

Installation with Composer
--------------------------

1. Add `"laminas/laminas-development-mode": "2.*"` to the `"require"` section your
   `composer.json` file and run `php composer.phar update`.
1. Copy `development.config.php.dist` to your application's `config/` directory,
   without renaming the file, and edit as required. Commit this file to your
   version control system. Optionally, if you want to override application config
   as well, copy `development.local.php.dist` file into `config/autoload/` and
   update it.
1. Add any development modules to the `"require-dev"` section of your
   application's `composer.json`. e.g:
   
   ```javascript
        "laminas/laminas-developer-tools": "dev-master",
        "laminas/laminastool": "dev-master"
   ```
        
   and run `composer.update`.
1. If you're using Laminas Developer Tools, Copy
   `./vendor/laminas/laminas-developer-tools/config/laminas-developer-tools.local.php.dist`
   to `./config/autoload/laminas-developer-tools.local.php`. Change any settings in
   it according to your needs.
1. Add `'Laminas\DevelopmentMode'` to the list of Modules in your
   application's `config/application.config.php` file.
1. In your application's `public/index.php`, replace these lines:

   ```php
        // Run the application!
        Laminas\Mvc\Application::init(require 'config/application.config.php')->run();
   ```

   with

   ```php
        // Config
        $appConfig = include 'config/application.config.php';

        if (file_exists('config/development.config.php')) {
            $appConfig = Laminas\Stdlib\ArrayUtils::merge($appConfig, include 'config/development.config.php');
        }

        // Run the application!
        Laminas\Mvc\Application::init($appConfig)->run();
   ```


To enable development mode
--------------------------

```sh
cd path/to/install
php public/index.php development enable
```

Note: enabling development mode will also clear your module configuation cache,
to allow safely updating dependencies and ensuring any new configuration is
picked up by your application.

To disable development mode
---------------------------

```sh
cd path/to/install
php public/index.php development disable
```

**Note:** Don't run development mode on your production server.
