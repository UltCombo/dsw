# Dynamic Service Worker


DSW allows you to enable and use Service Workers in a much easier way, also helping you to create and maintain your Progressive Web Apps working offline.

## Installing it

It's node program which you may install globally:

```npm install -g dsw```

Or locally:

```npm install dsw --save-dev```

## Using it

DSW will look for a file called `dswfile.json`. So:

```
cd path-to-your-project
touch dswfile.json
```

You will use your prefered editor to make changes to this file later.

And now, you will add this to your `index.html` file, like so:

```
< script src="dsw.js"></script>
< script>
    DSW.setup()
        .then(function(){
            // inform the user your page works offline, now!
        })
        .catch(function(){
            // do something if the page will not work offline
            // or if the current browser does not support it
        });
</script>
```

Done!

If you installed it globally, you should simply evoke:

```dsw path-to-your-project```

If you installed locally, though:

```node node_modules/dsw/ path-to-your-project```

This second example is specially useful if you intend to run it in a stand alone project or want to trigger it using a script in your `package.json` file.

From now on, let's work as if you had installed it globally in our examples.

You will notice a `dsw.js` file that has been created in your project's root path.

Now, let's set up your project's offline configuration.

When you change something in your `dswfile.json`, you shall re-execute the command above.

## Configuring it

Open the `dswfile.json` in the root of your project and let's add some content like this:

```
{
    "dswVersion": 2.2,
    "applyImmediately": true,
    "dswRules": {
        "yourRuleName": {
            "match": { },
            "apply": { }
        }
    }
}
```

That's it! You may have many rules.
Reminding that `applyImmediately` is optional. It will replace the previously registered service worker as soon as the new one loads.

### Matching

The `match` property accepts:

- status: An array with the matching statuses (eg.: [404, 500])
- extension: An array of matching extensions (eg.: ["html", "htm", "php"])
- path: A regular expression (cast in a string, so JSON can treat it)

### Applying

The `apply` property for each rule is used when a request matches the `match` requirements.
It may be:

- fetch: The (string)path to be loaded instead of the original request
- redirect: same as fetch, but setting the header status to 302
- cache: An object containing cache information for the request

#### Cache information

DSW will treat the cache layer for you.

Pass to the cache object in your apply definition, an object containing:

- name (mandatory, although a default name will be used if this is not passed)
- version (optional)

You can also define `cache: false`. This will force the request **not to be cached**.

Seens silly, but is useful when you want an exception for your cached data.

# Examples

Using both `match` and `apply`, we can for do a lot of things.<br/>
Don't forget to re-run `dsw path-to-project` whenever you made a change to your `dswfile.js` file.

#### Treating not found pages (404)

Add this to your `dswfile.js`:

```
{
    "dswVersion": 2.2,
    "dswRules": {
        "notFoundPages": {
            "match": {
                "status": [404],
                "extension": ["html"]
            },
            "apply": {
            	"fetch": "/my-404-page.html"
            }
        }
    }
}
```

Create a `my-404-page.html` with any content.

Now, access in your browser, first, the `index.html` file(so the service worker will be installed), then any url replacing the `index.html` string, and you will see your `my-404-page.html` instead.

#### Caching data

Let's see an example of requests being cached:

```
{
    "dswVersion": 2.2,
    "dswRules": {
        "myCachedImages": {
            "match": {
                "extension": ["png", "jpg", "gif"]
            },
            "apply": {
            	"cache": {
            		"name": "my-cached-images",
            		"version": 1
            	}
            }
        }
    }
}
```

#### Dealing with cache exceptions(cache: false)

Let's see an example of requests being cached for *all images* except one specific image:

```
{
    "dswVersion": 2.2,
    "dswRules": {
        "myNotCachedImage": {
            "match": {
                "path": "\/images\/some-specific-image"
            },
            "apply": {
            	"cache": false
            }
        },
        "myCachedImages": {
            "match": {
                "extension": ["png", "jpg", "gif"]
            },
            "apply": {
            	"cache": {
            		"name": "my-cached-images",
            		"version": 1
            	}
            }
        }
    }
}
```

#### Redirecting an URL

You may want to redirect requests some times, like so:

```
{
    "dswVersion": 2.2,
    "dswRules": {
        "secretPath": {
            "match": {
                "path": "\/private\/"
            },
            "apply": {
            	"redirect": "/not-allowed.html"
            }
        }
    }
}
```

# Contributing

So, you want to contribute? Cool! We need it! :)

Here is how...and yep, as Service workers are still a little too new, it is a little bit weird! Here is how I've been doing this, and if you have any better suggestion, please let me know :)

1 - Clone the project

```git clone 









