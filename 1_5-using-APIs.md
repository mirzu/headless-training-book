# 1.5 What exactly is a REST API?

REST: Everyone has heard about it, but not everyone has the same definition. It's become synonymous with the idea of a "web API," but it's real meaning is quite a bit more specific.

> In short it's a web API that is designed to work like the WWW.

## Outline
* API terms
    * REST
    * HTTP Methods (Verbs)
    * HTTP Status Codes
    * Media Type

## REST
Set of guidelines that define best practices for making an API. REST is not a formal specification, but rather a set of principles that guide API design, making your API work like the World Wide Web.
> From [wikipedia](http://en.wikipedia.org/wiki/Representational_state_transfer) Representational state transfer, an architectural style consisting of guidelines for creating scaleable web services.

If an API is following the REST principles it should have the following aspects:

1. base [URI](http://en.wikipedia.org/wiki/URI), such as http://example.com/resources/
2. an [Internet media type](http://en.wikipedia.org/wiki/Internet_media_type) for the data. This is often JSON but can be any other valid Internet media type (e.g. XML, Atom, microformats, images, etc.)
3. Use standard [HTTP methods](http://en.wikipedia.org/wiki/HTTP_method) (e.g., GET, PUT, POST, or DELETE)
4. hypertext links to reference state
5. hypertext links to reference related resources

[The Academic Roots of REST API Guidelines](http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)

## HTTP Methods
Define the desired action to be used when accessing a resource. You are likely familiar with GET and POST, but there are also:
HEAD, OPTIONS, PUT, DELETE, TRACE and CONNECT.

### Safe and UnSafe Methods.
Safe methods are, by convention: GET, HEAD, OPTIONS and TRACE. Accessing with these methods will not change the state of the application.

By contrast, methods such as POST, PUT, DELETE and PATCH are intended for actions that may cause side effects either on the server.

[more info](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods)

## Media Type
The [Internet media type](http://en.wikipedia.org/wiki/Internet_media_type) identifies the type of data the file contains. HTML pages will have the media type `text/HTML` and most of the APIs you use will be `application/json`. Some might use `application/xml` - I'm sorry.
