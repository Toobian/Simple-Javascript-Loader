# Simple Javascript Loader

For smaller projects you might not need a something like requirejs.
You'll just want to load javascript, and know when it's loaded.
That's all that Simple Javascript Loader does.

## Why would you want to use a javascript loader?

When a browser parses your html and finds a script tags, the browser stops rendering,
loads the script, interprets what needs to be done. When that's done it continues parsing.
It does that for EVERY script tag it encounters.

In a lot of cases javascript isn't dependent on another piece of javascript. In that case loading
those files side by side improves your sites performance quite a bit. This is what Simple Javascript
Loader does.

Even when you're loading just 3 javascript files, SJL will decrease the loadtime.

## Setup

First of all, you'll need to include `sjl.min.js` to your project:

```html
<!-- in your head -->
<script src="/assets/js/sjl.min.js"></script>
```

You can autoload a main js file like so:

```html
<!-- in your head -->
<script data-main="/assets/js/app.js" src="/assets/js/sjl.min.js"></script>
```

This will automatically load app.js for you.
This is the only fancy feature!

# Usage

```javascript
// Load a single file
sjl.load('/assets/js/jquery.js');

// Load multiple files.
sjl.load(['/assets/js/jquery.js', '/assets/js/underscore.js']);

// Do something after a javascript file is loaded
sjl.load('/assets/js/jquery.js', function(){
	
	$("document").ready(function(){
		// work with jQuery
	});
	
});

// Do something after a javascript file is loaded
sjl.load(['/assets/js/jquery.js', '/assets/js/underscore.js'], function(){
	
	$("document").ready(function(){
		// work with jQuery
	});
	
});
```

## Using the `.add` method

In some cases it's nice to add a bunch of files to a queue before starting to load.

```javascript
sjl.add(['/assets/js/jquery.js', '/assets/js/underscore.js']);
sjl.load(function(){
	// jQuery and underscore are loaded.
});
```

## Nested loading

In this case, Backbone.js depends on underscore and jQuery.

```javascript
sjl.add(['/assets/js/jquery.js', '/assets/js/underscore.js']);
sjl.load(function(){
	sjl.load('/assets/js/backbone.js', function(){
		// Do something with backbone.
	});
});
```

## Loading in sequence

Loading in sequence might just be the easiest way to handle javascript dependancies in SJL.

The `.sequence` method (or `.seq` for short) handles three kinds of arguments. Arrays, strings
and anonymous functions. Each part of the arguments is handles in sequence. 

- __String__<br/>Load a single javascript file.
- __Array__<br/>Load a group of javascript files. (Side by side for extra speed.)
- __Anonymous function__<br/>Fired in sequence.

Here is an example of how to use it:

```javascript
sjl.seq(
	[
		'/assets/js/libs/jquery.js',
		'/assets/js/libs/underscore.js',
		'/assets/js/libs/modernizr.js'
	],
	function(){
		// base libs loaded when this is fired
	},
	[
		'/assets/js/libs/hogan.js',
		'/assets/js/libs/backbone.js',
	],
	function(){
		// libs with dependancies are loaded
		// when this is fired
	}
);
```

### The end...