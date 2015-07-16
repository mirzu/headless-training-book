# 1.6 API Tools

## Outline
* Chrome Extensions
  * JSONView -> insert anchor links to lower sections
  * POSTMAN
* Command line tools
  * curl
  * resty
* Online tools
  * [Apiary](https://apiary.io/) - An API Documentation Tool
  * [Swagger](http://swagger.io/) - API Documentation and Editing Tools
  * [Runscope](https://www.runscope.com/) - An API Monitoring Tool, useful for CI
  * [MuleSoft](https://www.mulesoft.com/) - An API Services company, providing design, monitoring, and training services and resources

## Chrome Extensions
###  [JSONView](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en)

Makes reading JSON in the browser much, much easier. Lets go ahead and install this.

### [Postman](https://chrome.google.com/webstore/detail/postman-rest-client-packa/fhbjgbiflinjbdggehcddcbncdddomop?hl=en)

A tool that makes it easy to explore, test and experiment with APIs in the browser or as a desktop app.
Provides integration with [Newman](https://github.com/postmanlabs/newman), a command line API testing tool.

Note: One easy way to prevent your personal Chrome extensions and settings from interfering with your daily development workflow is to use two different Chrome installations. The [Canary](https://www.google.com/chrome/browser/canary.html) channel version of Chrome may not be 100% stable for daily browsing, but it has the latest dev tools updates and makes for a fine development browser. To keep things visually distinct, you might apply a different theme to your Canary browser. [Atlantic Canary](https://chrome.google.com/webstore/detail/atlantic-canary/kdhdhbgeochfkblomjngbnebbmoeblpg?hl=en) is a simple theme that follows the yellow aesthetic of Canary.

## Command line tools (Bonus Content)

### curl
[curl](http://curl.haxx.se/) curl is a command line tool for transferring data with URL syntax. It is very flexible and gives you a great deal of flexibility when sending URL requests.

```shell
curl http://USERNAME.drupal.4kclass.com/api/blogposts
```

useful Flags
`-X, --request <command>` Send a specific request method.
```shell
curl -X OPTIONS http://USERNAME.drupal.4kclass.com/api/blogposts
```

`-i, --include` Include header in output.
```shell
curl -X OPTIONS -i http://USERNAME.drupal.4kclass.com/api/blogposts
```

`-O, --remote-name` Save the output into a file named after the url.
```shell
curl -X OPTIONS -i -O http://USERNAME.drupal.4kclass.com/api/blogposts
```

`-u, --user <user:password>` Send a username and password.
```shell
curl -u fk:fkbuild http://USERNAME.drupal.4kclass.com/api/blogposts
```
There are many, many more options available that will allow very powerful manipulations how requests to URLs are crafted.

## Resty
A really useful little wrapper for curl that makes life easier when working with an API.

It's really simple to install:

    $ curl -L http://github.com/micha/resty/raw/master/resty > resty

Then source the script

    $ . resty

Now just set the REST host to your api endpoint.

    $ resty http://vagrant.restful.local.dev:8080/api

Now you can make REST requests on the commandline.

    $ GET /

Well that isn't terribly pretty, now is it? There are some ways using python and perl that make it easy to pretty print, but they are hard to remember.

    $ brew install yajl

    or

    $ sudo apt-get install yajl-tools

Now you can pretty print with a command that is easy to remember.

    $ GET / | json_reformat

[More info on Resty](https://raw.githubusercontent.com/micha/resty)

[More info on pretty printing JSON](http://www.skorks.com/2013/04/the-best-way-to-pretty-print-json-on-the-command-line/)
