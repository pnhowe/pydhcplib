:: 

                   _ _                _ _ _
   _ __  _   _  __| | |__   ___ _ __ | (_) |__
  | '_ \| | | |/ _` | '_ \ / __| '_ \| | | '_ \
  | |_) | |_| | (_| | | | | (__| |_) | | | |_) |
  | .__/ \__, |\__,_|_| |_|\___| .__/|_|_|_.__/
  |_|    |___/                 |_|


===========
 Pydhcplib
===========

Pydhcplib is a python library to read/write and encode/decode dhcp
packet on network.

N.B. This is a fork of this project http://pydhcplib.tuxfamily.org/pmwiki/.
The changes are mostly a port to python3, addition of option 82(rfc3046) parsing, 
packaging work, some code readability cleanup, a few minor bug fixes, and
the implementation of a missing feature as described in the end of this document [1_].

Installation :
==============

Use pip to install::

  $ pip3 install .

Or build a dpkg and install it (NOTE, you can update debian/changelog to change the package name)::

  dpkg-buildpackage -b -us -uc
  sudo dpkg -i ../python3-dhcplib_0.7.3-1noble1_amd64.deb

There is also a PPA with pre-build debian packages for a few distros
at: https://launchpad.net/~pnhowe/+archive/ubuntu/t3kton

How to use pydhcplib :
======================

Look in the examples directory to learn how to use the modules.::
  
  $ man pydhcp
  $ man pydhcplib

.. 1:

Differences to the original pydhcplib
=====================================

The short story is I've "stolen" the udp raw socket code from [the
amazing] *busybox* project changing it to work with the udp payload
[the actual dhcp packet] this library creates.

This was required to make it work in the case the fields `giaddr` and
`ciaddr` are zero and the broadcast bit flag is not set. This requires
unicasting the udp packet to the `yiaddr` address, which does not yet
exist. Using the kernel to send the packet fails as there is no ARP
information available.  This requires using raw sockets to inject the
missing `hwaddr` information.

