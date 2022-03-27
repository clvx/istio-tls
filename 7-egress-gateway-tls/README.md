The call leaves untoched as http://edition.cnn.com:80 but it's routed to the istio egress gateway which then makes the call to CNN. CNN then responds with a redirect for HTTPS which is another request to https://edition.cnn.com:443  

TLS implies the connection will be routed based on the SNI header to the destination without terminating the TLS connection.
