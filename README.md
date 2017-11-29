# TCLtools
Сollection of TCL scripts for Cisco IOS penetration testing

What software is featured?
---------------------------

 * TCLproxy — Proxy server implementation

TCLproxy
--------
TCLproxy can be used to create SOCKS4a proxy server or forward a remote port to a local port.

```
cisco# tclsh tclproxy.tcl
TCLproxy v0.0.1

Usage: tclsh tclproxy.tcl [-D address]... [-L address]...

Proxy server implementation.

  -D [bind_address:]port
    The script will create a SOCKS4a proxy.

  -L [bind_address:]port:remote_host:remote_port
    The script will forward a remote port to a local port.
    Multiple connections and multiple forwarding are supported.

 You can also use forwarding between some VRF tables if it is possible on this hardware:
    -D [VFR_server_table@][bind_address]:port[@VFR_clients_table]
    -L [VFR_server_table@][bind_address]:port[@VFR_clients_table]:remote_host:remote_port

  optional arguments:
  -f, --upgrade-the-speed      The speed can be increased by 1-15 KB/s, but connections don't close automatically. Dangerous!
  -h, --help                   Show this help message and exit
  -q, --disable-output         Quite mode. Dangerous, sometimes you can not stop the script after the start!
  -n, --disable-dns            Never do DNS resolution with -D

   example:
    $ sudo py3tftp -p 69
    cisco# copy tftp://192.168.1.10/tclproxy.tcl flash:/
    cisco# tclsh tclproxy.tcl -L 59001:10.0.0.1:445 -D 59000
    cisco# del flash:/tclproxy.tcl
```

About TCL
=========
TCL is a high-level, general-purpose, interpreted, dynamic programming language. Cisco IOS has a realization of 8.3.4 TCL version:

```
cisco# tclsh
cisco(tcl)# puts $tcl_version
8.3

cisco(tcl)# puts $tcl_patchLevel
8.3.4
```
There are differences between different versions of IOS, but it's possible to write rather stable software for all IOS version.


How to execute the scripts?
===========================
For executing the script you have to obtain privilege level 15 on the hardware. You can use command copy and upload the scripts on the hardware by FTP or TFTP:

```
cisco# copy tftp://192.168.1.10/tclproxy.tcl flash:/
cisco# copy ftp://192.168.1.10/tclproxy.tcl flash:/
```
or you can use tclsh for create a file: https://www.cisco.com/c/en/us/support/docs/ip/telnet/116214-technote-technology-00.html

Good practice is setting limit to the minimum size of free memory:

```
cisco# configure terminal
cisco(config)# scripting tcl low-memory 5242880
cisco(config)# end
```

There are commands to control current consumption of CPU or MEM of tcl scripts:

```
cisco# show processes cpu | i Tcl
cisco# show processes mem | i Tcl
```

Remarks for the scripts
=======================

 * Do not use TCLproxy for TCP/IP port scanning. TCL doesn't support -async option of socket, and the SOCKS will not work about 30 seconds after connection to a filtered port.
 * On older versions of IOS scripts can write the output to the another console. It's an IOS bug.
 * The script should be stopped after the session is broken; script will write something to the console

The scripts were tested on Cisco 2811 Integrated Services Router, Cisco Catalyst 2960, and Cisco Catalyst 3750-X.

Contact Us
==========

You can Open a New Issue to report a bug or suggest a new feature. Or you can drop a few lines at mohemiv@gmail.com.