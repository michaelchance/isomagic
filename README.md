IsoMagic
========

An Isomorphic Javascript Framework based on Express and TLC
-----------------------------------------------------------

### What?
* **Isomorphic**- Runs on the Server and the Browser.  Write it once, works on both.
* **Easily Extendable**
* **Simple Workflows**- Express-style middleware, TLC formats, that's it.
* **Maintainable**- Well structured TLC formats reduce javascript, offloading data translation to the template files
	
### How?

Install with npm
`npm install isomagic`

Write your index.js and run the server
```javascript
var config = require('./path/to/config.json');//configuration object, same object as the client
var options = {...} //some server options - often these are different on the client
var IsoMagic = require('isomagic');
var myApp = new IsoMagic(config, options, function(){
	myApp.listen(3000);
});
```

Create your index.html.  There's a few files you're going to need to add to your head:
* jQuery
* the files enclosed in this project's clientrouter/ folder
* tlc.js, available via `npm install tlc` or at https://github.com/michaelchance/tlc
* isomagic.js from this project
```html
<!doctype html>
<html>
<head>
	<script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/2.1.4/jquery.min.js"></script>
	<!-- Order is important for the following files -->
	<script type="text/javascript" src="path/to/clientrouter/path-to-regexp.js"></script>
	<script type="text/javascript" src="path/to/clientrouter/layer.js"></script>
	<script type="text/javascript" src="path/to/clientrouter/route.js"></script>
	<script type="text/javascript" src="path/to/clientrouter/index.js"></script>
	<script type="text/javascript" src="path/to/tlc.js"></script>
	<script type="text/javascript" src="path/to/isomagic.js"></script>
	<script type="text/javascript">
		//jQuery is currently required, so why not use a convenience method to load the config.json file?
		$.getJSON('path/to/config.json', function(config){ // Same config object you loaded on the server
			var options = {...} // Some options, often different from the server
			var myApp = new IsoMagic(config, options, function(){
				console.log('app has loaded!');
			});
		}); 
	</script>
</head>
<body>
	<h1>Hello World!</h1>
</body>
</html>
```

Next, you need to set up your Config, and install some Extensions

### Config

The config object is the soul of your application.
```javascript
{
	"extensions" : [], // Required - Array of Extension Config objects that tell IsoMagic how to load your installed extensions.  Without these, IsoMagic doesn't do much!
	"routes" : [], // Required - Array of Route Config objects that tell IsoMagic how to mount extension middleware to particular routes.  Also needed to do something interesting!
	"basePath" : "/", // Optional-- the base path that your application will handle requests on, defaults to "/"
	"document" : "index.html", // Optional-- the file that gets loaded for processing by the middleware.  Defaults to "index.html"
	"browserEvents" : ["click","submit","mouseenter","mouseleave","change"], //Optional, default shown.  An array of browser events that IsoMagic will capture and process for you with TLC modules
}
```

### Extensions