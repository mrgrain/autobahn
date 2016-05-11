# Autobahn

Autobahn is the Ueberholspur for your Composer-based WordPress stack. German Engineering.

## Quick start
_This requires [Composer](https://getcomposer.org/) and [Vagrant](https://www.vagrantup.com/) to be properly set up on your machine. If not, please skip to the documentation._

Create a new project by running:
```
composer create-project mrgrain/autobahn
```

and start your local Vagrant environment with:
```
vagrant up
```

Open your browser and navigate to [autobahn.jumpstart.rocks](http://autobahn.jumpstart.rocks). That's all!


## License
The mrgrain/autobahn package is open-sourced software licensed under the [GPL-3.0](LICENSE) license.

## Documentation

### Installation

Use composer to create a new project
```
composer create-project mrgrain/autobahn
```
... or clone the git repository and run composer install.
```
git clone https://github.com/mrgrain/autobahn.git autobahn
cd autobahn
composer install
```

#### Vagrant setup
Provisioning your Vagrant virtual machine will execute all required steps: 
```
vagrant up
```

#### Manual setup
After installing the composer dependencies, set up a basic configuration by copying `.env.example` to `.env` and adjust it to your needs:
```
cp .env.example .env
vim .env
```
This most likely required you to change the database connection details and set an appropriate `WP_HOME` URL.

Next generate the WordPress security keys using Autobahn CLI:
```
./vendor/bin/autobahn keys:generate
```

Finally, run the WordPress install via wp-cli (change URL to an appropriate value):
```
./vendor/bin/wp core install --url=http://localhost
```

### Basic usage

#### .env file
Sensitive environment configuration should be stored in the `.env` file based in your root directory.

#### autobahn.json
WordPress configuration has to be set in the `autobahn.json` file in based in your root directory.

#### Files setup
```
autobahn
|-- public              # web server root
|   |-- app             # renamed `wp-content` directory
|   |   |-- mu-plugins
|   |   |-- plugins
|   |   |-- themes
|   |   `-- uploads
|   |
|   |-- wp              # WordPress core files
|   |
|   |-- index.php       # main scipt entry point
|   `-- wp-config.php   # adjusted `wp-config.php` using `.env` & `autobahn.json`
|
|-- vendor              # composer dependencies
|-- .env                # environment configuration
`-- autobahn.json       # Autobahn WordPress configuration
```

### Settings

#### Autobahn constants
`WP_ENV` - Set the environment, defaults to `development`.
`WPMU_LOADER` - Path to a mu-plugins loader, see [advanced configuration](#mu-plugins-loader) section.

#### Path constants
`PUBLIC_DIR` - Absolute or relative (project's root directory) path to the public web server document root directory, defaults to `public/`.
`CONTENT_DIR` - Relative path (from `PUBLIC_DIR`) to the WordPress content directory, defaults to `app/`.
`WORDPRESS_DIR` - Relative path( from `PUBLIC_DIR`) to the WordPress core, defaults to `wpp/`.

#### WordPress constants
All default WordPress constants are available via `.env` or `autobahn.json`.

#### WordPress options
Any WordPress option can be forced to a value using the `options` section in the `autobahn.json` file. 

#### PHP configuration

### Environments

### Advanced Configuration
#### mu-plugins loader

#### Change public server directory
The easiest way to achieve this, is setting a symlink to the existing `public` directory.
```
# Create a symlink named "web" to the existing "public" directory
ln -s public web
```
However, if you need to move the files, you can do this by changing a few configuration values.
First, update the composer installer-paths for plugins, themes and the WordPress install directory in your `composer.json` file:
```
{
  "extra": {
    "installer-paths": {
      "public/app/mu-plugins/{$name}/": [
        "type:wordpress-muplugin"
      ],
      "public/app/plugins/{$name}/": [
        "type:wordpress-plugin"
      ],
      "public/app/themes/{$name}/": [
        "type:wordpress-theme"
      ]
    },
    "wordpress-install-dir": "public/wp"
  }
}
```
Run `composer update` to move all files to the new location.

Next, add `PUBLIC_DIR` as well as `WORDPRESS_DIR` and `CONTENT_DIR` to your `autobahn.json` configuration.
```
{
  "config": {
    "PUBLIC_DIR": "public/",
    "WORDPRESS_DIR": "wp/",
    "CONTENT_DIR": "app/"
  }
}
```
`PUBLIC_DIR` can be an absolute path or relative to the project's root directory. `WORDPRESS_DIR` and `CONTENT_DIR` are relative to the `PUBLIC_DIR`. Both are also added to the end of the website URL to access the WordPress admin area and plugin/themes files.

As last we'll need to change the wp-cli configuration to keep it working. Therefore update the `wp-cli.yml` file:
```
path: public/wp
```

##### wp-content location
By default the `wp-conent` folder is moved to `public/app`. If you want to change the location of it in your project, please follow the steps described in **Change public server directory**, only changing the `app` related values.

##### WordPress location
By default WordPress is installed to `public/wp`. If you want to change the location of WordPress in your project, please follow the steps described in **Change public server directory**, only changing the `wp` related values.


#### wp-cli.yml
You can set any configuration for wp-cli to its `wp-cli.yml` file. For example you might want to globally set the `url` value and thus skip passing it on to every command:
```yml
url: http://autobahn.jumpstart.rocks
```
Please be aware that changing the `path` will require further adjustments to keep everything working.
