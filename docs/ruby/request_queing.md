# Scout Ruby Request Queuing Configuration

Scout can measure the time it takes a request to reach your Rails app from farther upstream (a load balancer or web server). This appears in Scout as "Request Queuing" and provides an indication of your application's capacity. Large request queuing time is an indication that your app needs more capacity.

<img src="/_static/images/request_queue_annotated.png" alt="request queuing" />

To see this metric within Scout, you need to configure your upstream software, adding an HTTP header that our agent reads. This is typically a one-line change.

## HTTP Header

The Scout agent depends on an HTTP request header set by an upstream load balancer (ex: HAProxy) or web server (ex: Apache, Ngnix).

__Protip:__ We suggest adding the header as early as possible in your infrastructure. This ensures you won't miss performance issues that appear before the header is set.

The agent will read any of the following headers as the start time of the request:

```ruby
%w(X-Queue-Start X-Request-Start X-QUEUE-START X-REQUEST-START x-queue-start x-request-start)
```

Include a value in the format `t=MICROSECONDS_SINCE_EPOCH` where `MICROSECONDS_SINCE_EPOCH` is an integer value of the number of microseconds that have elapsed since the beginning of Unix epoch.

__Nearly any front-end HTTP server or load balancer can be configured to add this header.__ Some examples are below.

## Apache

Apache's __mod_headers__ module includes a `%t` variable that is formatted for Scout usage. To enable request queue reporting, add this code to your Apache config:

````
RequestHeader set X-Request-Start "%t"
````

### Apache Request Queuing and File Uploads

If you are using Apache, you may observe a spike in queue time within Scout for actions that process large file uploads. Apache adds the `X-Request-Start` header as soon as the request hits Apache. So, all of the time spent uploading a file will be reported as queue time.

This is different from Nginx, which will first buffer the file to a tmp file on disk, then once the upload is complete, add headers to the request. 

## HAProxy

HAProxy 1.5+ supports timestamped headers and can be set in the frontend or backend section. We suggest putting this in the frontend to get a more accurate number:

````
http-request set-header X-Request-Start t=%Ts
````

## Nginx

Nginx 1.2.6+ supports the use of the `#{msec}` variable. This makes adding the request queuing header straightforward.

General Nginx usage:

````
proxy_set_header X-Request-Start "t=${msec}";
````


Passenger 5+:

````
passenger_set_header X-Request-Start "t=${msec}";
````

Older Passsenger versions:

````
passenger_set_cgi_param X-REQUEST-START "t=${msec}";
````

Note: The Nginx option is local to the <a href="https://www.phusionpassenger.com/library/config/nginx/reference/#this-configuration-option-is-not-inherited-across-contexts">location block</a>, and isn't inherited.