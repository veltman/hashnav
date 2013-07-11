hashnav
=======

A very simple JS hash-based navigation library if you want permalinks, working back buttons, etc. without a page request.  It supports simple query strings.  It uses the 'onhashchange' function for browsers that support it and checks for changes on a 100ms interval for browsers that don't.  There are lots of much better hash-based navigation libraries out there but I always end up writing this functionality myself because they are too heavy or I'm lazy.

## Usage ##

Include the `hashnav.js` script and then call the `initHashNav()` function to start listening for changes.  The initHashNav() needs one argument: a callback function that will be called with the details of the new hash whenever it changes.  If you don't supply a function, nothing will happen.

An object is passed to the callback function with the details of the hash as follows:

	{
		raw: "details?state=CA&zip=90210",
		base: "details",
		parameters: {
			state: "CA",
			zip: "90210"
		}
	}

* `raw` is the entire hash string, minus the initial '#'
* If the hash string has a '?' in it, it is assumed to have an old-fashioned query string, and `base` is the part before the '?'.  If the hash string does not have a '?' in it, `base` is the same as `raw`.
* If the hash string has a '?' in it, anything after the '?' is split into name-value pairs based the & and equal sign and added to `parameters`.  If there are no parameters, it is `null`.  If a parameter has a name, but no value, the parameter gets the value `null`.

** Keep in mind that everything gets sent back as a string.  If you want to treat something as an integer or anything else, make sure to convert it first. **

## Basic example: ##

	<script src="hashnav.js"></script>
	<script>

		function logHash(hash) {			
			if (hash.parameters) {				
				console.log("There are parameters.");
				for (var name in parameters) {
					console.log("The value of " + name + " is " + parameters[name]);
				}
			} else {
				console.log("There are no parameters.");
				console.log(hash.raw);
			}		
		}

		initHashNav(logHash);

	</script>

## More examples of what the callback function would receive: ##

`page.html#contact` produces:

	{
		raw: "contact",
		base: "contact",
		parameters: null
	}

`page.html#section?gallery` produces:

	{
		raw: "section?gallery",
		base: "section",
		parameters: {
			gallery: null
		}
	}

`page.html#search?language=en&sort=desc` produces:

	{
		raw: "search?language=en&sort=desc",
		base: "search",
		parameters: {
			language: "en",
			sort: "desc"
		}
	}

`page.html#?&a&b` produces:

	{
		raw: "?&a&b",
		base: "",
		parameters: {
			a: null,
			b: null
		}
	}