# httpload: event loop based multiprocessing HTTP load test tool

## Introduction

**httpload** is a tool for testing web sites. It issues a series of
HTTP requests in parallel. The URLs to be hit are read from a file and
tried randomly. It runs a single process. It's particularly adequate
for testing asynchronous web servers in multi-core/multi-cpu machines
like [Nginx](http://nginx.org).

The [tool](http://www.acme.com/software/http_load/) was created by
[Jef Poskanzer](http://www.acme.com/jef/).

This version is IPv6 and SSL/TLS enabled.

## Vanilla installation

 1. Clone the repo from git://github.com/perusio/httpload.git
    or grab the tarball from [downloads](https://github.com/perusio/httpload/archives/master).
 
 2. Issue `make` & `make install`. By default the binary will be
    installed under `/usr/bin` and the man page under `/usr/share/man/man1`.
    
 3. Done

## Debian package installation 

 1. Either download the binary package from
    [debian.perusio.net](http://debian.perusio.net/unstable) or download the
    source package and build from source while following the
    [instructions](http://debian.perusio.net).
   
 2. Done.
 
## Usage

`httpload` requires you to specify an option for **starting** the
connection and one for **terminating** the connection.

 * `-parallel` or `-rate` for starting.
 
 * `-seconds` or `-fetches` for terminating.
 
 Additionally a `-jitter` option specifies that the rate given in
 `-rate` varies up to 10%.
 
The URLs in the URL file are visited in random order. You can specify
the seed of the random number generator with:
 
 * `-seed integer`     

The `-seed` option was added by
[Tim Bunce](https://github.com/timbunce/http_load/commit/eaac0cab647dfa68b863565082f70c8ddcc5ee85).

`httpload` can query HTTPS based servers. If you don't specify the
cipher using:

 * `-ciphers str` where `str` is a
   [openssl](http://www.openssl.org/docs/apps/ciphers.html) cipher
   specification strings.
   
   An example is `ECDHE-RSA-AES256-SHA`. 
      
A complete example of querying the URLs specified in file
`test_url_s.txt` using the specified cipher.
 
    httpload -cipher ECDHE-RSA-AES256-SHA -parallel 10 -seconds 10 test_url_s.txt 

    279 fetches, 10 max parallel, 11997 bytes, in 10.0124 seconds
    43 mean bytes/connection
    27.8654 fetches/sec, 1198.21 bytes/sec
    msecs/connect: 33.6593 mean, 347.213 max, 0.037 min
    msecs/first-response: 247.849 mean, 378.282 max, 31.069 min
    HTTP response codes:
    code 200 -- 279

The tool accepts three shortcut names for a *fast*, *high* and
*paranoid* cipher. Notwithstanding the *fast* cipher is not that
fast, since it only accepts SSLv3+ ciphers. There are speed diferences
between them. The shortcuts are:

 * `fastsec`  - CAMELLIA128-SHA
 * `highsec`  - AES256-SHA
 * `paranoid` - DHE-RSA-AES256-SHA
 
The complete usage documentation is on the
[manpage](http://github.perusio.org).

## Comparison with other testing tools

 * [`ab`](http://httpd.apache.org/docs/2.2/programs/ab.html):
   `httpload` differs significantly from `ab`, since uses the
   [Apache Portable Runtime](http://apr.apache.org/docs/apr/1.4/) which
   uses threads instead of a single process (loop) like `httpload`. `ab`
   is **CPU bound**.

 * [`siege`](http://www.joedog.org/siege-home): also relies on
   **threads** contrary to `httpload` that uses a single process
   (loop).
   
## TODO   

 1. Implement CSV output for statistical and graphical treatment of
    the test results.
 
 2. Implement a single URL target given on the command line.
 
 3. Implement
    [`epoll(7)`](https://secure.wikimedia.org/wikipedia/en/wiki/Epoll)
    I/O event notification in Linux as an alternative to [`select(2)`](https://secure.wikimedia.org/wikipedia/en/wiki/Select_\(Unix\)).
