composer-patcher
================

Plugin to patch composer downloads post-hoc.

Arguably you wouldn't want to do this, if you could avoid it. But it's the
way that a lot of existing drupal.org patch workflow happens (especially
via Drush make) and so this provides a useful transition technology.

Minimum `composer.json`
-----------------------

The package is now registered on Packagist:

https://packagist.org/packages/jpstacey/composer-patcher

so you should only require the following minimum JSON:

```json
{
    "require": {
        "jpstacey/composer-patcher": "*"
    },
    "scripts": {
        "post-package-install": "Composer\\Patcher\\PatcherPlugin::postPackageInstall"
    }
}
```

The "scripts" is required in your root `composer.json` as it will not run in
a subsidiary `composer.json` for security reasons.

Example `composer.json`
-----------------------

Downloads and patches a Drupal module:

```json
{
    "repositories": [
        {
            "type": "composer",
            "url": "http://packagist.drupal-composer.org"
        }
    ],
    "require": {
        "jpstacey/composer-patcher": "*",
        "drupal/xmlsitemap": "2.0-rc2"
    },
    "extra": {
        "patches": {
            "drupal/xmlsitemap": [
                {
                    "title": "Call to undefined function xmlsitemap_link_frontpage_settings() https://www.drupal.org/node/1392710",
                    "url": "https://www.drupal.org/files/include_inc_file-1392710.patch"
                }
            ]
        }
    }
    "scripts": {
        "post-package-install": "Composer\\Patcher\\PatcherPlugin::postPackageInstall"
    }
}
```
