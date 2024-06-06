# Silverstripe CDN Rewrite Plus

Provides a simple method of rewriting the URLs of assets and resources to allow the use of a subdomain or external CDN service

See the configuration notes below for an example of how to set this module up.

# Requirements
* Silverstripe 4.x
* Silverstripe 5.x

# Installation
* Install the code with `composer require swordfox/silverstripe-cdnrewrite-plus "^1"`
* Run a `dev/build?flush` to update your project

# Usage

The module won't make any changes to your site unless you do a bit of configuration.  There are a few options you can set, done in a yml file:


```yaml
---
Name: cdnconfig
---

Swordfox\CDNRewritePlus\CDNMiddleware:
  cdn_rewrite: true
  cdn_domain: 'https://cdn.example.com'
  add_debug_headers: true
  enable_in_dev: true
  subdirectory: ''
  add_prefetch: true
  rewritelinks: true
  rewritebgattr: true
  assetprefix: 'site-'
  rewrites:
    - 'assets'
    - 'site-resources'
```

The options are hopefully fairly self explanatory:

* `cdn_rewrite` - globally enables and disables the module (default false - disabled)
* `cdn_domain` - the full domain name of the CDN (required to enable module)
* `add_debug_headers` - if enabled, adds extra HTML headers to show the various operations being applied (default false)
* `enable_in_dev` - enable the CDN in dev mode (default false)
* `subdirectory` - set this if your site is in a subdirectory (eg. for http://www.example.com/silverstripe - set this to 'silverstripe')
* `add_prefetch` - set this to true if you want the module to automatically add a `<link rel="dns-prefetch">` tag to your html head to improve performance
* `rewritelinks` - set this to false if your CDN does not support all file types e.g. PDFs
* `rewritebgattr` - rewrites background-image: url(<url>) paths
* `assetprefix` - adds a prefix to the assets directory for use on shared CDNS e.g. `https://cdn.example.com/site-assets/image.jpg`
* `rewrites` - this is a list of the prefixes you wish to rewrite.  By default, CMS 4 exposes content in a _resources directory in the public structure, so you'll probably want that as a minimum.  You can add as many additional entires here as required.

# Notes

* The module is disabled in the CMS / admin system, so rewrites do not currently happen here
* When enabled, the module will always add an HTTP header of `X-CDN: Enabled` to show that it's working, even if none of the other rewrite operations are carried out.  If this is not present and you think it should be, ensure that you have set `cdn_rewrite` to true, that you have specified the `cdn_domain` in your config file and that you have `enable_in_dev` set to true if you are testing in dev mode.


# Credits
* Complete copy of [DorsetDigital's silverstripe-cdnrewrite](https://github.com/DorsetDigital/silverstripe-cdnrewrite) with a few extra options.
