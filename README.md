proxyt
======

TCP proxy CLI with optional TLS support in pure Node.JS

About
-----

I wrote `proxyt` to be a simple tool that makes spinning up a TCP proxy (with
optional TLS) support an easy task.  I normally use `nc` whenever I need a
simple proxy, but I wanted something with slightly more functionality that
worked across multiple operating systems without needing to be recompiled.
Since I already use Node everywhere at home, this seemed like an obvious fit.

I also wanted something that could reload certificates without having to be
shut down / restarted.  `proxyt` takes a `-i <interval>` option (explained
below) that will cause the certs to be reloaded automatically on a set
interval.

All logging is done via [Bunyan](https://github.com/trentm/node-bunyan).

Examples
--------

### TCP proxy

Listen on all interfaces on port 80 and forward incoming requests to 10.0.1.234
on port 8080.

    proxyt 0.0.0.0:80 10.0.1.234:8080

### TCP proxy with TLS

    proxyt -k pem.key -c pem.cert 0.0.0.0:443 10.0.1.234:8080

Listen on all interfaces on port 443 using TLS and forward incoming requests to
10.0.1.234 on port 8080.

### TCP proxy with TLS and advanced options

    proxyt -k pem.key -c pem.cert -i 86400 -H my-host.example.com 0.0.0.0:443 10.0.1.234:8080

Listen on all interfaces on port 443 using TLS and forward incoming requests to
10.0.1.234 on port 8080.  Also, reload the cert and key every 86400 seconds
(every day) and verify that the incoming SNI hostname matches `my-host.example.com`.

Usage
-----

    $ proxyt -h
    Usage: proxyt [options] <listen> <target>

    Example:

      Listen on all interfaces on port 80 (locally) and forward
      incoming requests to 10.0.1.234 on port 8080

        $ proxyt 0.0.0.0:80 10.0.1.234:8080

      Listen locally via TLS on port 443 (locally) and forward incoming
      requests to 1.2.3.4 on port 5678

        $ proxyt -k pem.key -c pem.cert 127.0.0.1:443 1.2.3.4:5678

    Options:

      -c, --cert <cert>       certificate file to use (must be specified with -k)
      -H, --sni-host <host>   SNI Hostname (optional), connections that do not match will be dropped
      -h, --help              print this message and exit
      -i, --interval <secs>   interval in seconds to reload TLS key and cert, defaults to none
      -k, --key <key>         key file to use (must be specified with -c)
      -l, --log-level <lvl>   bunyan log level to use, defaults to info
      -n, --no-dns            do not attempt to resolve IP addresses
      -v, --version           print the version number and exit

License
-------

MIT License
