# Grav Static-PageCache Plugin

`Static-PageCache` is a powerful static page cache type plugin that caches the entire page output to the cache folder and allows http server to use this when the URL path is requested again.  This dramatically increases the performance of a Grav site.  Due to the static nature of this cache, if enabled, you will need to **manually** clear the cache if you make any page modifications.  This cache plugin (by default) will cache pages that have URLs that contain either **querystring or grav-paramater** style values, you may want to disable this behavior.

This plugin can provide dramatic performance boosts and is an ideal solution for sites with many pages and **only** static content.

You need to also modify your server's config. I.e. add this to top of grav/.htaccess for Apache:
```
## Static file cache:
RewriteCond %{DOCUMENT_ROOT}/cache/staticpagecache/%{HTTP_HOST}%{REQUEST_URI}/index.html -f
RewriteCond %{REQUEST_METHOD} GET
RewriteCond %{HTTP:Pragma} !no-cache
RewriteCond %{HTTP:Cache-Control} !no-cache
RewriteRule .* /cache/staticpagecache/%{HTTP_HOST}%{REQUEST_URI}/index.html [L]
RewriteRule ^(cache/staticpagecache/.*) $1 [L]
```

# Installation

Installing the Static-PageCache plugin can be done in one of two ways. Our GPM (Grav Package Manager) installation method enables you to quickly and easily install the plugin with a simple terminal command, while the manual method enables you to do so via a zip file.

## GPM Installation (Preferred)

The simplest way to install this plugin is via the [Grav Package Manager (GPM)](http://learn.getgrav.org/advanced/grav-gpm) through your system's Terminal (also called the command line).  From the root of your Grav install type:

    bin/gpm install static-pagecache

This will install the Static-PageCache plugin into your `/user/plugins` directory within Grav. Its files can be found under `/your/site/grav/user/plugins/static-pagecache`.

## Manual Installation

To install this plugin, just download the zip version of this repository and unzip it under `/your/site/grav/user/plugins`. Then, rename the folder to `static-pagecache`. You can find these files either on [GitHub](https://github.com/j000/grav-plugin-static-pagecache) or via [GetGrav.org](http://getgrav.org/downloads/plugins#extras).

You should now have all the plugin files under

    /your/site/grav/user/plugins/static-pagecache

# Usage

The default configuration provided in the `user/plugins/static-pagecache.yaml` file contains sensible defaults:

```
enabled: true                   # set to false to disable this plugin completely
enabled_with_params: true       # enable if there are params set on this URI (eg. /color:blue)
enabled_with_query: true        # enable if there are query options set on this URI (eg. ?color=blue)
whitelist: false                # set to array of enabled page paths to enable only when in whitelist
blacklist:                      # set to array and provide list of page paths to disable plugin for
  - /error
```

If a **whitelist** array is provided, **only** pages specifically listed will be cached.
If a **blacklist** array is provided, this plugin will cache all pages except those specifically listed.

## Important Notes

This plugin is intended for **production** scenarios where optimal performance is desired and more important than convenience. `Static-PageCache` is not intended to be used in a development environment or a rapidly changing one.

No plugin events will fire when a cached page is found becuase these are **not processed by Grav**, only the static page is returned.
