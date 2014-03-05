# AngularJS and Internet Explorer
## AngularJS和IE

AngularJS works seamlessly with most modern browsers. Safari, Google Chrome, Google Chrome Canary, and Firefox work great. The notorious Internet Explorer version 8 and earlier can cause us problems.
`AngularJS能很好地兼容常见的现代浏览器，比如Safari，Chrome，Chrome Canary和Firefox。但在臭名昭著的IE8及以下版本的浏览器中，我们可能会遇到一些问题。`


For more information, read the AngularJS docs guide on IE124.
`注：想了解更多关于这方面的信息，可以阅读AngularJS文档指南中的IE124（这里有一个跳转）`


If we are planning release our applications for Internet Explorer v8.0 or earlier, we need to pay some
extra attention to help support it.
`如果大家想在IE8之前的浏览器中运行AngularJS，我们需要一些额外的支持。`

Internet Explorer does not like element names that start with a prefix ng: because it considers the prefix to be an XML namespace. IE will ignore these elements unless the elements have a corresponding namespace declaration:
`IE不能很友好的支持带ng:的前缀，因为IE会认为这是一个XML命名空间，它会忽略这个前缀，除非该元素有一个类似下面这样的命名空间声明：`
<html xmlns:my="ignored">
This xmlns:ng=”http://angularjs.org” makes IE feel more comfortable.
`注：xmlns:ng="http://angularjs.org"能让AngularJS更好的工作`

If we use non-standard HTML tags, we need to create the tags in the head of the document if we want IE to recognize them. We can do so simply in the head element.
`如果大家使用非标准的HTML标签，我们需要在header标签中使用JavaScrit来创建这个标签，比如下面这样：`

```html
<!doctype html>
<html xmlns:ng="http://angularjs.org"> <head>
    <!--[if lte IE 8]
      <script>
        document.createElement('ng-view');
        // Other custom elements
      </script>
    <![endif]-->
  </head>
  <body>
<!-- ... -->
```
It is recommended that we use the attribute directive form, as we don’t need to create custom elements to support IE:
`建议大家使用属性标签，因为我们并不需要创建自定义元素就能支持IE浏览器`

备注：124 http://docs.angularjs.org/guide/ie


<div data-ng-view></div>
To make AngularJS work with IE7 and earlier, we need to polyfill JSON.stringify. We can use the
JSON3125 or JSON2126 implementations.
`为了使IE7或更早的版本支持AngularJS，我们需要一个JSON.stringify补丁，我们可以使用JSON3125(注) 或者 JSON2126(注) 的实现`


In our browser, we need to conditionally include this file in the head. We must download the file,
store it in a location relative to the root of our application, and reference it in the head, like so:
`我们需要在head中用条件注释来加载它，引用位于网站根目录的相对位置，比如下面这样： `

```html
<!doctype html>
<html xmlns:ng="http://angularjs.org"> <head>
    <!--[if lte IE 8]
    <script src="lib/json2.js"></script>
    <![endif]-->
</head> <body>
```
<!-- ... -->
To use the ng-app directive with IE support, we set the element id to ng-app, as well. <body id="ng-app" ng-app="myApp">
<!-- ... -->
We can take advantage of the angular-ui-utils library’s ie-shiv module to help give us custom elements in our DOM.
In order to use the ui-utils ie-shiv library, we need to ensure that we have the angular-ui library installed. Installation is easy if we download the ui-utils library and include the module. We can find the ui-utils library on GitHub here: https://github.com/angular-ui/ui-utils127.
Let’s ensure that we’ve included the ui-utils module in an accessible location to your app and include the file just like this:
￼125http://bestiejs.github.io/json3/ 126https://github.com/douglascrockford/JSON- js 127https://github.com/angular- ui/ui- utils
AngularJS and Internet Explorer 514
<!--[if lte IE 8]>
<script type="text/javascript">
  // define our custom directives here
</script>
<script src="lib/angular-ui-ieshiv.js"></script>
<![endif]-->
With that in place, we only activate the ie-shiv on Internet Explorer versions 8 and earlier. The shiv enables us to add our custom directives onto its global object, which, in turn, creates the proper declarations for IE.
The shiv library looks for the window.myCustomTags object. If the window.myCustomTags is defined, the library will include these tags at load time along with the rest of the Angular library directives:
<!--[if lte IE 8]>
<script type="text/javascript">
  // define our custom directives here
  window.myCustomTags = ['myDirective'];
</script>
<script src="lib/angular-ui-ieshiv.js"></script>
<![endif]-->
Ajax Caching
IE is the only major browser that caches XHR requests. An efficient way to avoid this poor behavior is to set an HTTP response header of Cache-Control to be no-cache for every request.
This behavior is the default behavior for modern browsers and helps to provide a better experience for IE users.
We can change the default headers for every single request like so:
.config(function($httpProvider) { $httpProvider.defaults
    .headers.common['Cache-Control'] = 'no-cache';
});
SEO with AngularJS
Search engines, such as Google and Bing, are engineered to crawl static web pages, not JavaScript- heavy, client-side apps. Search engine robots are typically designed to be quick and efficient, so most do not render JavaScript when crawling over a page.
That’s because our JavaScript-heavy apps need a JavaScript engine to run, like PhantomJS or v8, for instance. Web crawlers typically load a web page without using a JavaScript interpreter.
AngularJS and Internet Explorer 515
It is for good reason that search engines do not include JS interpreters in their crawlers: They don’t need to, and doing so slows them down and makes them more inefficient for crawling the web.
Getting Angular Apps Indexed
There are several different ways that we can tell Google to handle indexing our app. The first, more common approach is to use a back end to serve our Angular app. This method has the advantage of being simple to implement without much duplication of code.
The second approach is to render all of the content delivered by our Angular app inside a <noscript> tag in our JavaScript. We’ll cover this topic lightly, as implementing the <noscript> approach depends heavily upon how we deliver our apps. For instance, we can use a <noscript> tag when we’re rendering our pages on the server-side.
Server Side
Google and other advanced search engines support the hashbang URL format, which we use to identify the current page that’s being accessed at a given URL. These search engines transform this URL into a custom URL format that makes them accessible to the server.
The search engine visits the URL and expects to get the HTML (with the fully rendered HTML content) that our browsers will receive. For instance, Google will turn the hashbang URL from:
http://www.ng-newsletter.com/#!/signup/page
into the URL:
http://www.ng-newsletter.com/?_escaped_fragment_=/signup/page
Within our Angular app, we need to tell Google to handle our site slightly differently depending upon which style we handle.
Hashbang Syntax
Google’s Ajax crawling specification was written and originally intended for delivering URLs with the hashbang syntax, which was an original method of creating permalinks for JS applications.
We need to configure our app to use the hashPrefix (default) in our routing:
AngularJS and Internet Explorer 516
angular.module('myApp', []) .configure(['$location', function($location) {
  $location.hashPrefix('!');
}]);
HTML5 Routing Mode
The new HTML5 pushState doesn’t work the same way: It modifies the browser’s URL and history. To get Angular apps to “fool” the search bot, we can add a simple element to the header:
<meta name="fragment" content="!">
This element tells the Google spider to use the new crawling spec to crawl our site. When it encounters this tag, instead of crawling our site like normal, it will revisit the site using the ?_escaped_fragment_= tag.
Assuming we’re using HTML5 mode with the $location service, we can set our page to use html5Mode like so:
angular.module('myApp', []) .configure(['$routeProvider', function($routeProvider) {
$routeProvider.html5Mode(true); }]);
With the _escaped_fragment_ in our query string, we can use our back-end server to serve static HTML instead of our client-side app.
Now, our back end can detect if the request has the _escaped_fragment_ in the request, and, if so, we can serve static HTML back instead of our pure Angular app.
We can achieve this result using a proxy, like Apache or Nginx, or our back-end service. Setting these up is out of scope of this book; however, we’ll discuss how to set them up with a NodeJS app.
Options for Handling SEO from the Server Side
We have a number of options available to us to make our site SEO-friendly. We’ll walk through three different ways to deliver our apps from the server side:
• Using Node/Express middleware • Using Apache to rewrite URLs
• Using Nginx to proxy URLs
￼
AngularJS and Internet Explorer 517
Using Node/Express Middleware
Note: Although we are using NodeJS in this example, this implementation is simply one way to serve static HTML snapshots to our back end. This technique works regardless of the back end you’re using to deliver your HTML.
To deliver static HTML using NodeJS and Express (the web application framework for NodeJS), we must add some middleware that looks for the _escaped_fragment_ in our query parameters:
// ideas shared by
// In our app.js configuration app.use(function(req, res, next) {
var fragment = req.query._escaped_fragment_; if (!fragment) return next();
// If the fragment is empty, serve the // index page
if (fragment === "" || fragment === "/")
    fragment = "/index.html";
// If fragment does not start with '/' // prepend it to our fragment
if (fragment.charAt(0) !== "/")
    fragment = '/' + fragment;
// If fragment does not end with '.html' // append it to the fragment
if (fragment.indexOf('.html') == -1)
    fragment += ".html";
  // Serve the static html snapshot
try {
var file = __dirname + "/snapshots" + fragment; res.sendfile(file);
} catch (err) { res.send(404);
} });
This middleware expects our snapshots to exist in a top-level directory called ‘/snapshots’ and serve files based upon the request path.
AngularJS and Internet Explorer 518 For instance, it will serve a request to / as index.html, while it will serve a request to /about as
about.html in the snapshots directory. Use Apache to Rewrite URLs
If we’re using the Apache server128 to deliver our Angular app, we can add a few lines to our configuration that will serve snapshots instead of our JavaScript app.
We can use the mod_rewrite mod to detect if the route requested includes the _escaped_fragment_- query parameter or not. If it does include it, then we’ll rewrite the request to point to the static version in the /snapshots directory.
In order to set the rewrite in motion, we need to enable the appropriate modules:
$ a2enmod proxy
$ a2enmod proxy_http
Then we need to reload the Apache config:
$ sudo /etc/init.d/apache2 reload
We can set the rewrite rules either in the virtual host configuration for the site or the .htaccess file
that sits at the root of the server directory.
RewriteEngine On
Options +FollowSymLinks
RewriteCond %{REQUEST_URI}  ^/$
RewriteCond %{QUERY_STRING} ^_escaped_fragment_=/?(.*)$
RewriteRule ^(.*)$ /snapshots/%1? [NC,L]
Use Nginx to Proxy URLs
If we’re using Nginx129 to serve our Angular app, we can add some configuration to serve snapshots of our app if there is an _escaped_fragment_ parameter in the query strings.
Unlike Apache, Nginx does not require us to enable a module, so we can simply update our configuration to replace the path with the question file instead.
In our Nginx configuration file (e.g., /etc/nginx/nginx.conf), we need to ensure our configuration looks like:
128http://httpd.apache.org/ 129http://wiki.nginx.org/Main
￼
AngularJS and Internet Explorer 519
server {
  listen 80;
  server_name example;
if ($args ~ "_escaped_fragment_=/?(.+)") { set $path $1;
rewrite ^ /snapshots/$path last;
}
location / {
root /web/example/current/;
# Comment out if using hash urls if (!-e $request_filename) {
rewrite ^(.*)$ /index.html break; }
    index index.html;
  }
}
Once this step is complete, we’re good to reload our configuration:
sudo /etc/init.d/nginx reload
Taking Snapshots
We can take snapshots of our HTML app to deliver our back-end app, using a tool like PhantomJS130 or zombie.js131 to render our pages. When Google requests a page using the _escaped_fragment_- query parameter, we can simply return and render this page.
We’ll discuss two methods to take snapshots: using Zombie.js and using a Grunt tool. We’re not going to cover using the fantastic PhantomJS132 tool, as there are plenty of great resources that demonstrate it.
Using Zombie.js to Grab HTML Snapshots
To set up Zombie.js133, we need to install the npm package zombie:
130http://phantomjs.org/ 131http://zombie.labnotes.org/ 132http://phantomjs.org/ 133http://zombie.labnotes.org/
￼
AngularJS and Internet Explorer 520
$ npm install zombie
Now, we can save our file with NodeJS by using zombie. First, a few helper methods we’ll use in the
process:
var Browser = require('zombie'),
url = require('url'),
fs = require('fs'),
saveDir = __dirname + '/snapshots';
var scriptTagRegex = /<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi;
var stripScriptTags = function(html) { return html.replace(scriptTagRegex, '');
}
var browserOpts = { waitFor: 2000, loadCSS: false, runScripts: true
}
var saveSnapshot = function(uri, body) {
var lastIdx = uri.lastIndexOf('#/');
if (lastIdx < 0) {
// If we're using html5mode path = url.parse(uri).pathname;
} else {
// If we're using hashbang mode path =
      uri.substring(lastIdx + 1, uri.length);
  }
if (path === '/') path = "/index.html"; if (path.indexOf('.html') == -1)
    path += ".html";
var filename = saveDir + path; fs.open(filename, 'w', function(e, fd) {
if (e) return;
AngularJS and Internet Explorer 521
    fs.write(fd, body);
  });
};
Now all we need to do is run through our pages, turn every link from a relative link into an absolute link (so the crawler can follow them), and save the resulting HTML.
We’re setting a relatively high waitFor in the browser options above. This option will cover 90% of the cases we care about. If we want to get more precise on how and when we take a snapshot, instead of waiting the two seconds, we need to modify our Angular app to fire an event and listen for the event in our Zombie browser.
Since we like to automate as much as possible and prefer not to muck with our Angular code, we prefer to set our timeout relatively high to attempt to let the code settle down.
Our crawlPage() function:
var crawlPage = function(idx, arr) { // location = window.location
if (idx < arr.length) {
var uri = arr[idx];
var browser = new Browser(browserOpts); var promise = browser.visit(uri) .then(function() {
// Turn links into absolute links
// and save them, if we need to
// and we haven't already crawled them var links = browser.queryAll('a'); links.forEach(function(link) {
var href = link.getAttribute('href'); var absUrl = url.resolve(uri, href); link.setAttribute('href', absUrl); if (arr.indexOf(absUrl) < 0) {
          arr.push(absUrl);
        }
});
// Save
      saveSnapshot(uri, browser.html());
      // Call again on the next iteration
      crawlPage(idx+1, arr);
    });
} }
AngularJS and Internet Explorer 522 Now we can simply call the method on our first page:
crawlPage(0, ["http://localhost:9000"]);
Using grunt-html-snapshot
Our preferred method of taking snapshots is by using the Grunt tool grunt-html-snapshot. Since we use Yeoman134, and Grunt is already in our build process, we set up this task to run after we make a release of our apps.
To install grunt-html-snapshot, we can use npm like so:
npm install grunt-html-snapshot --save-dev
If we’re not using Yeoman135, we need to include this task as a Grunt task in our Gruntfile.js:
grunt.loadNpmTasks('grunt-html-snapshot');
Once this task is set, we’ll set some configuration about our site. To set up configuration, we create a new config block in our Gruntfile.js that looks like:
htmlSnapshot: {
  debug: {
    options: {}
  },
  prod: {
    options: {}
} }
Now we get to add our preferred options for the different stages:
134http://yeoman.io 135http://yeoman.io
￼
AngularJS and Internet Explorer 523
htmlSnapshot: {
  debug: {
    options: {
      snapshotPath: 'snapshots/',
      sitePath: 'http://127.0.0.1:9000/',
      msWaitForPages: 1000,
      urls: [
'/',
'/about'
] }
}, prod: {
options: {} }
}
To see a list of all of the available configuration options, check out the documentation page at https://github.com/cburgdorf/grunt-html-snapshot136.
Prerender.io
Alternatively, we can use an open-source tool, such as Prerender.io137, which includes a Node server that renders our site on the fly, and an Express middleware that communicates with the back end to prerender HTML on the fly.
Essentially, prerender.io takes a URL and returns the rendered HTML (with no script tags). Essentially, the prerender server we deploy is called from our app like so:
GET http://our-prerenderserver.com/http://localhost:9000/#/about
This GET returns the rendered content of our #/about page.
Setting up a prerender cluster is actually pretty easy to do. We’ll show you how to integrate your own prerender server into your Node app, as well. Prerender.io is also available for Ruby on Rails through a gem called prerender_rails, but we won’t cover how to set it up.
Setting up our own server to run it is pretty easy. We simply run npm install to install the dependencies and run the command through either Foreman or Node:
136https://github.com/cburgdorf/grunt- html- snapshot 137http://prerender.io/
￼
AngularJS and Internet Explorer 524
$ npm install
$ node index.js
# Or through foreman $ foreman start
The prerender library is also convenient to run on heroku:
$ git clone https://github.com/collectiveip/prerender.git
$ heroku create
$ git push heroku master
We store our rendered HTML in S3, so we recommend you use the built-in s3 cache. Read the docs how to set up this cache here138.
After our server is running, we just need to integrate the fetching through our app. In Express, this integration is very easy using the Node library prerender-node.
To install prerender-node, we use npm again:
$ npm install --save prerender-node
After we’ve installed this library, we can tell our Express app to use this middleware:
var prerender =
require('prerender-node')
.set('prerenderServiceUrl', 'http://our-prerenderserver.com/');
app.use(prerender);
And that is it! This last bit tells our Express app that if we see a crawler request (defined by having the _escaped_fragment_ or the user agent string), it should make a GET request to our prerender service at the appropriate URL and get the prerendered HTML for the page.
<noscript> Approach
We can also use the <noscript> tag to render our pages without needing to resort to using a server back end. Unfortunately, this method is complex in that for all of our pages, we’d need to copy all of the elements of the page from outside of the <noscript> tag into such a tag.
Doing so can become cumbersome and take a lot of work to keep the two in sync, so we don’t recommend this approach without a build tool to assist.