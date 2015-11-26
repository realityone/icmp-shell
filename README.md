> fork from http://icmpshell.sourceforge.net

================================================================
         ___ ____ __  __ ____    ____  _          _ _ 
        |_ _/ ___|  \/  |  _ \  / ___|| |__   ___| | |
         | | |   | |\/| | |_) | \___ \| '_ \ / _ \ | |
         | | |___| |  | |  __/   ___) | | | |  __/ | |
        |___\____|_|  |_|_|     |____/|_| |_|\___|_|_|
                        VERSION: 0.2

               (c) eluks - all rights reserved.

                   by: Peter Kieltyka (elux)
           http://peter.eluks.com / peter@eluks.com


                      January 31, 2002
================================================================


What is ICMP Shell?
===================
ICMP Shell is a program written in C for the UNIX environment
that allows an administrator to access their computer remotely
via ICMP.


How does it work?
=================
The ISHELL server is run in daemon mode on the remote server.
When the server recieves a request from the client it will
strip the header and look at the ID field, if it matches the
the server then it will pipe the data to "/bin/sh". It will
then read the results from the pipe and send them back
to the client and the client prints the results to stdout.

By default the client and server send packets with an ICMP
type of 0 (ICMP_ECHO_REPLY), however this can be changed
on both the client and server side. ISHELL does not care what
type you send out from the client or server end, the types
do not have to match.

ISHELL does not only pipe commands to a server and send back
the output. It also works with interactive programs (ie. gdb).
However, there comes a minor problem from this. ISHELL cannot
display a shell prompt (#). The reason for that is because there
is no way to differentiate between a command an interaction with
a program. If you have any ideas on how to implement that then
I'd be more then happy to hear from you ;)


Firewall? No one said anything about a firewall!
================================================
By default ISHELL uses icmp type 0 (ICMP_ECHO_REPLY) to send/recv.
With a little bit of research I have found that icmp type 0 works
best with this program.  Other types do work, however some kernels
process ICMP_ECHO_REQUEST packets automatically (BSD) while others
do not (Linux).


Installation
============
Call 'make' and follow the instructions.


Files
=====
MD5 (ish.c)      = 07934540ee7ca6ac7919bb1ea49fd7ff
MD5 (ish_main.c) = e2885ef2eb7688caff9b45f8c81daf8f
MD5 (ish_open.c) = 81b11fce190a321a02b5313b1b244aa7
MD5 (ishd.c)     = de574728574dc3a8d5389172ca4e3b6a
MD5 (ishell.h)   = 380b110ba648164a82a0ffddbb0920f9


The server/client have been tested on:
- Linux Mandrake 8.1   x86
- FreeBSD 4.4          x86
- OpenBSD 3.0          x86
- Solaris 8            sparc


Some IMPORTANT words on the usage
=================================
1.) ISHELL uses raw sockets on both the client and server
side, therefore root privileges ARE REQUIRED to use this
program.

2.) When configuring the options for the server/client make
sure the following options are the same on both the
client and the server:

-i <id>
-p <packetsize>


Usage: Server
=============
ICMP Shell v0.1  (server)   -   by: Peter Kieltyka
usage: ./ishd [options]

options:
 -h               Display this screen
 -d               Run server in debug mode
 -i <id>          Set session id; range: 0-65535 (default: 1515)
 -t <type>        Set ICMP type (default: 0)
 -p <packetsize>  Set packet size (default: 512)

example:
./ishd -i 65535 -t 0 -p 1024


Usage: Client
=============
ICMP Shell v0.1  (client)   -   by: Peter Kieltyka
usage: ./ish [options] <host>

options:
 -i <id>          Set session id; range: 0-65535 (default: 1515)
 -t <type>        Set ICMP type (default: 0)
 -p <packetsize>  Set packet size (default: 512)

example:
./ish -i 65535 -t 0 -p 1024 host.com


ICMP Type Reference
===================

Here is a list of icmp types that you can use with ISHELL:

Type    Name                                    Reference
----    -------------------------               ---------
  0     Echo Reply                               [RFC792]
  1     Unassigned                                  [JBP]
  2     Unassigned                                  [JBP]
  3     Destination Unreachable                  [RFC792]
  4     Source Quench                            [RFC792]
  5     Redirect                                 [RFC792]
  6     Alternate Host Address                      [JBP]
  7     Unassigned                                  [JBP]
  8     Echo                                     [RFC792]
  9     Router Advertisement                    [RFC1256]
 10     Router Selection                        [RFC1256]
 11     Time Exceeded                            [RFC792]
 12     Parameter Problem                        [RFC792]
 13     Timestamp                                [RFC792]
 14     Timestamp Reply                          [RFC792]
 15     Information Request                      [RFC792]
 16     Information Reply                        [RFC792]
 17     Address Mask Request                     [RFC950]
 18     Address Mask Reply                       [RFC950]
 19     Reserved (for Security)                    [Solo]
 20-29  Reserved (for Robustness Experiment)        [ZSu]
 30     Traceroute                              [RFC1393]
 31     Datagram Conversion Error               [RFC1475]
 32     Mobile Host Redirect              [David Johnson]
 33     IPv6 Where-Are-You                 [Bill Simpson]
 34     IPv6 I-Am-Here                     [Bill Simpson]
 35     Mobile Registration Request        [Bill Simpson]
 36     Mobile Registration Reply          [Bill Simpson]
 37     Domain Name Request                     [Simpson]
 38     Domain Name Reply                       [Simpson]
 39     SKIP                                    [Markson]
 40     Photuris                                [Simpson]
 41-255 Reserved                                    [JBP]


