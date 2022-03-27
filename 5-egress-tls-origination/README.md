Istio needs to be configured to restrict external access to Pilot's registry otherwise all traffic would be allowed.

If `targetPort` isn't defined in the Service Entry, the request would be done as HTTP and then redirected to be done as HTTPS due the 301 status code that 
edition.cnn.com returns forcing 2 requests and also sending data in plain text.
In other words, the connection will be done as http://edition.cnn.com:80 and then redirected to https://edition.cnn.com:443 after reciving 301.

To avoid this with TLS Origination add `targetPort` in the ServiceEntry and a Destination Rule to upgrade the connection. The connection 
will start as http://edition.cnn.com:80/politics but it will be upgraded to https://edition.cnn.com:443/politics due the ServiceEntry before hitting the 
istio-proxy.
instead of http://edition.cnn.com:443/politics as the previous case.
