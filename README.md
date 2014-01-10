[![Analytics](https://ga-beacon.appspot.com/UA-46910666-1/agektmr/PortableCache.js)](https://github.com/igrigorik/ga-beacon)

# PortableCache.js

Cache assets, reduce downloads, load faster.

## What is PortableCache.js?

[ApplicationCache](http://www.whatwg.org/specs/web-apps/current-work/multipage/offline.html) 
is an offline enabler that can also speed up your website by reducing   
the number of requests to a server. But there are a few glitches that keeps   
people from using it. You might have wished:

* Permanently cache heavy resources without caching root HTML.
* Update assets without reloading. (AppCache requires page reload.)
* Per content versioning. (AppCache downloads entire resources in manifest 
  again!)

PortableCache.js is a resource loader for mobile web developers to solve those 
problems.

* Declarative.
* Uses the best available storage on user's browser:
    * FileSystem
    * IndexedDB
    * WebSQL
    * LocalStorage
* Fallback gracefully when no storage mechanisms are available.
* Provides imperative option to handle control.

Couple of bonus points:

* lazyload images.
* Supports responsive images (NOT IMPLEMENTED YET).

## Demo
### [PortableCache Example](http://demo.agektmr.com/portable-cache/)

Simplest declarative usage with lazyload images.

### [Web Audio Drumpad](http://demo.agektmr.com/drumpad/)

Imperative resource caching with deferred AngularJS bootstrap.

## Quick Start

You can quickly try out this library by following 3 steps.

1. Insert following `meta` tag to your existing project's `head` tag.<br/>
   `<meta name="portable-cache" content="version=20131228">`
1. Insert `script` tag to load `portable-cache.min.js` (Make sure it is below 
   `meta[name="portable-cache"]`).<br/>
   `<script 
   src="https://raw.github.com/agektmr/PortableCache.js/0.6.0/dist/portable-cache.min.js">`
1. Replace attribute name of resources you'd like to cache to 
   `data-cache-url`.<br/>
   `<img src="img/image.jpg">`<br/>
   `<img data-cache-url="img/image.jpg">`

## Configuration

Configuration is set by using `meta[name="portable-cache"]`. The `content` 
attribute accepts comma separated parameters as listed below.

    <meta name="portable-cache" content="version=20130627, preferred-storage=localstorage, root-path=/portable-cache, debug-mode=yes, auto-init=no>

<!-- TODO: Fix formatting of cells -->
<table>
<tr>
<th>Key</th>
<th>Value</th>
<th>Default</th>
<th>Details</th>
</tr>
<tr>
<td>version</td>
<td>string</td>
<td>''</td>
<td>Required. String that indicates current version. If this differs from cookie stored previous version, resources will be updated.</td>
</tr>
<tr>
<td>preferred-storage</td>
<td>(auto|filesystem|idb|sql|localstorage)</td>
<td>'auto'</td>
<td>Optional. Preferred storage to use. If the preferred storage is not available on the browser, PCache gracefully falls back.</td>
</tr>
<tr>
<td>root-path</td>
<td>string</td>
<td>'/'</td>
<td>Optional. Cache version usually is tied to the URL path you are on. Specify a root path when you want the cache to be restricted to the directory.</td>
</tr>
<tr>
<td>auto-init</td>
<td>(yes|no)</td>
<td>'yes'</td>
<td>Optional. You can manually bootstrap PortableCache by setting this 'no'.</td>
</tr>
<tr>
<td>debug-mode</td>
<td>(yes|no)</td>
<td>'no'</td>
<td>Optional. Enables debug messages in console if 'yes'</td>
</tr>
</table>

## Declarative APIs
### Caching and loading resources

All you have to do is to replace attributes that indicates URL of the resource 
to `data-cache-url`.

#### script

    <script data-cache-url="js/main.js"></script>

You can use `async` attribute to indicate that the script can immediately 
execute. Otherwise, scripts will be executed in order. `defer` attribute is not 
supported (for natural reason).

    <script data-cache-url="js/main.js" async></script>

#### link

    <link rel="stylesheet" data-cache-url="css/style.css">

#### img

    <img data-cache-url="img/image.jpg">

You can defer loading of images until user actually see them in viewport by 
adding `lazyload` attribute.

    <img data-cache-url="img/image.jpg" lazyload>

### Per element versioning

You may want to retain version of certain cached elements. These versions will 
override global version specified in `meta[name="portable-cache"]`.

    <img data-cache-url="img/image.jpg" data-cache-version="20131228">

### Responsive images (NOT IMPLEMENTED)

You can load responsive image by using `srcset` semantics. Details TBD.

## Imperative APIs

For configuration, use `meta[name="portable-cache"]` explained at Declarative 
APIs.

### CacheEntry
#### Properties
##### url

URL of this resource that can replace `src` when fallback.

##### src

Resource URL to be replaced with `src` or `href`

##### tag

HTML tag associated with this CacheEntry

##### type

Request type on XHR (binary|json|text)

##### mimetype

MIME Type of remote resource.

##### version

Cache version string.

##### content

Resource content which is either Blob or text.

##### elem

Original DOM Element.

##### lazyload

Boolean value that indicates if lazyload is requested.

##### async

async for `script` tag

#### Methods
##### load(callback)

TBD

##### readCache(callback, errorCallback)

TBD

##### createCache(cacheExists, callback, errorCallback)

TBD

##### removeCache()

TBD

##### fetch(callback, errorCallback)

TBD

##### constructDOM(callback)

TBD

##### getContentAs(type, callback, errorCallback)

TBD

### Events

PortableCache fires `pcache-ready` event after loading `link` and `script` 
resources.

## Examples
### Simplest Declarative Example

    <!DOCTYPE html>
    <html>
      <head>
        <title>PortableCache Example</title>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
        <meta name="portable-cache" content="version=20130630">
        <link rel="stylesheet" data-cache-url="css/bootstrap.css">
        <script src="js/portable-cache.js"></script>
      </head>
      <body>
        <div class="navbar">
          <div class="navbar-inner">
            <div class="container">
              <span class="brand">PortableCache.js</span>
            </div>
          </div>
        </div>

    ...........snip..............

            <h2>Current Status</h2>
            <p>Under development. Very early stage.</p>
            <h2>Example Image Stabs</h2>
            <img data-cache-url="img/abstract1_640x428.jpg" alt="" lazyload>
            <img data-cache-url="img/abstract2_640x441.jpg" alt="" lazyload>
            <img data-cache-url="img/abstract3_640x541.jpg" alt="" lazyload>
            <h2>Author</h2>
            <ul>
              <li>Eiji Kitamura (<a href="http://google.com/+agektmr" target="_blank">+agektmr</a>, <a href="http://twitter.com/agektmr" target="_blank">@agektmr</a>)</li>
            </ul>
          </div>
        </div>
      </body>
    </html>

### AngularJS manual initialization
#### HTML

        <script data-cache-url="js/angular.js"></script>
        <script data-cache-url="js/audio.js"></script>
        <script data-cache-url="js/main.js"></script>
        <script src="js/portable-cache.js"></script>

#### JavaScript

    var app = angular.module('App', []);
    app.directive('aDirective', function($window) {
      ...
    });
    // Invoke Angular manual initialization after 'pcache-ready' event.
    document.addEventListener('pcache-ready', function() {
      angular.bootstrap(document, ['App']);
    });

### Imperative cache resource handling

    var map = [
      '/sample/snare.wav',
      '/sample/bass.wav',
      '/sample/hihat.wav'
    ];
    var buffer = [];
    for (var i = 0; i < map.length; i++) {
      (function(i, url) {
        var cache = new CacheEntry({url:url, type:'binary'});
        cache.load(function(cache) {
          cache.getContentAs('arraybuffer', function(b) {
            buffer[i] = c.createBuffer(b, false);
          });
        });
      })(i, map[i]);
    }

## Server side optimization

PortableCache sends a version string stored in cache as `pcache_version` in 
cookie. Notably, if the browser is already proved to be unsupported (fallback 
without caching), it carries a string `NOT_SUPPORTED` instead of a version 
string. Use this cookie so your server can serve an HTML without 
PortableCache.js in mind to avoid overheads.

## Browser Support

Following browsers are supported by PortableCache.js.

* Chrome (FileSystem API)
* Firefox (IndexedDB)
* IE 9 (LocalStorage)
* IE 10, 11 (IndexedDB)
* Android Browser 2.3 (WebSQL)
* Android Browser 4 (WebSQL)
* Safari 7 (WebSQL)

Following browsers are confirmed to gracefully fallback on PortableCache.js

* IE 6, 7, 8

Need tests on following browsers

* Android Browser 3 (WebSQL)
* Safari 5 (WebSQL)
* Safari 6 (WebSQL)

Browsers not listed here are yet to be tested.

## Author

* Eiji Kitamura ([+agektmr](https://google.com/+agektmr), 
  [@agektmr](https://twitter.com/agektmr))