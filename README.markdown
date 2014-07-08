Restclient
==========

Status Note
-----------

restclient is still maintained and in production use by the
author. However, I would not recommend starting new projects with
it. restclient was written years ago when the only options available
were raw httplib2/urllib type libraries and made my (and others') life
much easier. Now though, the `requests` library is available, better
documented, better tested, and more widely understood. `requests` does
everything restclient does so if you should use it instead.

I'm happy to take patches and bugfixes on restclient and will try to
keep it alive and available for anyone who is already using and
doesn't feel the need to switch. But there will probably not be any
new development on restclient.

Introduction
------------

A helper library to make writing REST clients in python extremely
simple. Specifically, while httplib2 and similar libraries are very
efficient and powerful, doing something very simple like making a POST
request to a url with a couple parameters can involve quite a bit of
code. Restclient tries to make these common tasks simple. It does not
try to do everything though. If you need to construct a request in a
very particular way, are expecting a large result, or need better
error handling, nothing is stopping you from using one of the lower
level libraries directly for that.

Installation
============

If you have setuptools installed, just do "easy_install restclient"


Documentation
=============

Restclient is very simple so the main documentation is in docstrings in the code itself. this page just serves as a quick starter.


    from restclient import GET, POST, PUT, DELETE
    r = GET("http://www.example.com/") # makes a GET request to the url. returns a string 
    POST("http://www.example.com/") # makes a POST request
    PUT("http://www.example.com/") # makes a PUT request
    DELETE("http://www.example.com/") # makes a DELETE request
    POST("http://www.example.com/",params={'foo' : 'bar'}) # passes params along
    POST("http://www.example.com/",headers={'foo' : 'bar'}) # sends additional HTTP headers with the request
    POST("http://www.example.com/",accept=['application/xml','text/plain']) # specify HTTP Accept: headers
    POST("http://www.example.com/",credentials=('username','password')) # HTTP Auth

Restclient also handles multipart file uploads nicely:

    f = open("foo.txt").read()
    POST("http://www.example.com/",files={'file1' : {'file' : f, 'filename' : 'foo.txt'}})


by default, POST(), PUT(), and DELETE() make their requests asynchronously. IE, they spawn a new thread to do the request and return immediately. GET() is synchronous. You can change this behavior with the 'async' parameter:

    POST("http://www.example.com/",async=False) # will wait for the request to complete before returning

Doing an asynchronous GET would be silly and pointless so I won't give an example of that but I'm sure you could figure it out.

With any of those, if you add a return_resp=True argument, restclient
will make the request like normal and then return the raw httplib2
response object. You'll have to extract the response body yourself,
but you'll also have access to the HTTP response codes, etc.

For finer grained control over the httplib2 library use the keyword 'httplib_params' 
to supply a dict of key/value pairs.  This will be passed unadulterated as 
parameters to httplib2.Http(). In addition, you can now include a debuglevel as
a key to 'httplib_params' to see the debug output from httplib2. The value 
is an integer for the amount of debug output desired. The debug level is set
for the one REST call and then reset to the previous value which allows 
general debugging to be enabled and then override with detailed debugging 
for specific calls.  

The handling of JSON data in POST and PUT requests is now handled by setting
the 'params' option to the data structure and adding a Content-Type header 
set to 'application/json'. For example:

    POST("http://www.example.com", params={'name':'Some User', 'action':'create'}, headers={'Content-Type': 'application/json'})


Credits
=======

written by Anders Pearson at the [Columbia Center For New Media Teaching And Learning](http://ccnmtl.columbia.edu/).

httplib2 debugging and JSON support added by Gerard Hickey (ghickey@ebay.com)
