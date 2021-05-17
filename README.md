# magento-2-dev-doc
miscellaneous dev task flows for magento 2 development

## List of contents
1. [Installation](#installation)
2. [Command line reference](#command-line-reference)
3. [Performance considerations](#performance-considerations)
4. [Module development](#module-development)
5. [Deployment](#deployment)

## Installation
Clone git / [Use install guide](https://devdocs.magento.com/guides/v2.4/install-gde/composer.html)
* run `bin/magento setup:install --base-url=... --base-url-secure=... --language=de_DE --currency=EUR --timezone=Europe/Berlin --admin-user=... --admin-firstname=... --admin-lastname=... --admin-email=... --admin-password=... --use-rewrites=0 --db-host=127.0.0.1 --db-user=... --db-name=one2buy --db-password=... --search-engine=elasticsearch7 --elasticsearch-host=127.0.0.1 --elasticsearch-port=9200`

## Command line reference
For a list of magento 2 commands [look here](https://devdocs.magento.com/guides/v2.4/config-guide/cli/config-cli-subcommands.html) or run `bin/magento list`

## Performance considerations
[See this link](https://www.atwix.com/magento-2/ways-to-make-theme-faster/)

## Module development
* Consider using [Mage2Gen module generator cli tool](https://pypi.org/project/Mage2Gen/) or [Silksoftware module creator online tool](https://modulecreator.silksoftware.com/magento-module-creator/magento2-module-creator.php)
* [Module development documentation](https://devdocs.magento.com/videos/fundamentals/create-a-new-module/)
### In webshop package
* Create custom `vendor/module` directory in `app/code` folder
### Git package within project
* Create custom directory in project root
  * f.e. `src/`  
**_NOTE: It's best to add `src/` path to shop project's `.gitignore` to prevent any accidential commits_**  
* Add custom module to `src/` directory
```
vendor/module
```
* Add module to `require` section
```
"require": {
   ...
   "vendor/module": "version"
   ...
}
```
* Add dev and git paths to `repository` section
```
"repositories": {
   ...
   "vendor.module": {
            "type": "package",
            "package": {
                "name": "vendor/module",
                "version": "x.x.x",
                "source": {
                    "type": "vcs|git",
                    "url": "{url}",
                    "reference": "x.x.x"
                },
                "dist": {
                    "type": "path",
                    "url": "src/vendor/module",
                    "options": {
                        "symlink": true
                    }
                }
            }
        },
   ...
}
```
[It might be possible to do this easier](https://laracasts.com/discuss/channels/general-discussion/switch-composer-package-from-vcs-to-path-and-back)  
Composer automatically preferences `dist` for packages, so in development it should always load from the path if it exists.  
On the other hand, if the module path does not exist, composer will then try to fetch from the `source`.  
If you run into issues, its best to run `composer require vendor/module` or `composer remove vendor/module` and `composer install vendor/module` again.

To install run `composer install vendor/module`

## Deployment
[See sample deploy.sh](https://github.com/Luc4G3r/magento-2-dev-doc/blob/main/SAMPLES/deploy.sh)
