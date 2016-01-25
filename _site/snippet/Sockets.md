### OS X and iOS networking APIs
 Layer                | Protocol streams | URL loading                                             | Service discovery
     ------           | ---------------  |    --------                                             | ------------
 Foundation layer     |NSStream          |NSURLConnection and NSURLRequest|NSNetService
 Core Foundation layer|CFStream          |CFHTTPMessage                                            |CFNetService
 POSIX layer          |kqueue </br> Built on top of BSD sockets (directly or indirectly)           |                                |DNS Service Discovery
