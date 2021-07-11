---
layout: post
title: networking - netcat
date: 2021-07-04
---

I need to rebuild my mental model of networking (long forgotten, now inaccurate).
<br>
Ultimately I want to remember what exactly happens when a packet is received by my computer, how it is "sent" to the correct application, how it made it there.

Here I play with `netcat` on a linux VM.

## netcat

man output

> NAME
> nc — arbitrary TCP and UDP connections and listens

What does this mean?

The description is a bit more useful

> DESCRIPTION
> The nc (or netcat) utility is used for just about anything under the sun involving TCP, UDP, or UNIX-domain sockets.  It can open TCP connections, send UDP packets, listen on arbitrary TCP and UDP ports, do port scanning, and deal with both IPv4 and IPv6.  Unlike telnet(1), nc scripts nicely, and separates error messages onto standard error instead of sending them to standard output, as telnet(1) does with some.

Why does it mention `telnet`? I guess there's some history that links the two somehow. TBC

Listen to a port (thanks to ` -l`)
<br>
It's kind of in server mode.

```
nc -l -p 4004
```


> Listen for an incoming connection rather than initiating a connection to a remote host.  The destination and port to listen on can be specified either as non-optional arguments, or with options -s and -p respectively.  Cannot be used together with -x or -z.  Additionally, any timeouts specified with the -w option are ignored.

Establishes a connection.
<br>
It's kind of in client mode.

```
nc localhost 4004
```

Executing these 2 commands in 2 consoles create a connection and one can send data from either console :)


### Observation 1

If you do `nc localhost -p 4004` without first opening a connection, nothing happens and it returns.
<br>
`strace` shows:

```
connect(3, {sa_family=AF_INET, sin_port=htons(4004), sin_addr=inet_addr("127.0.0.1")}, 16) = -1 EINPROGRESS (Operation now in progress)
pselect6(4, NULL, [3], NULL, NULL, NULL) = 1 (out [3])
getsockopt(3, SOL_SOCKET, SO_ERROR, [ECONNREFUSED], [4]) = 0
fcntl(3, F_SETFL, O_RDWR|O_NONBLOCK)    = 0
close(3)                                = 0
exit_group(1)                           = ?
+++ exited with 1 +++
```

So something about a ECONNREFUSED but I don't know (yet) what these various system calls do.


### Observation 2

I can check the port is opened:

```
nc -zv 127.0.0.1 4000-4010
```

This scans the port on the localhost between 4000-4010.

I noticed when I executed this that it caused the connection on the other end to close.
<br>I ran `strace` and realised all this does is just looping "normal" nc on all ports: so scanning ports here means trying to establish a connection on all the passed ports.


### Observation 3

```
strace nc -l -p 4004
```

The last few lines are what makes most sense to me:

```
socket(AF_INET, SOCK_STREAM, IPPROTO_TCP) = 3
setsockopt(3, SOL_SOCKET, SO_REUSEADDR, [1], 4) = 0
setsockopt(3, SOL_SOCKET, SO_REUSEPORT, [1], 4) = 0
bind(3, {sa_family=AF_INET, sin_port=htons(4004), sin_addr=inet_addr("0.0.0.0")}, 16) = 0
listen(3, 1)                            = 0
accept4(3,
```

* `socket` creates a socket:
<br>Function signature: `int socket(int domain, int type, int protocol);`
<br>`AF_INET`: IPv4 Internet protocols, `SOCK_STREAM`: the socket type (leaving that one aside), `IPPROTO_TCP`: I guess IP over TCP)

* `setsockopt` sets some options

* `bind`
<br>Not fully clear yet but this is important so that the process binds the socket just created and itself. This is only done on the server side.
<br>

* `listens`
<br>
The man page is great :)
> marks the socket referred to by sockfd as a passive socket, that is, as a socket that will be used to accept incoming connection requests using accept(2).

* `accept`
<br>
It gets ready to accept connection.


### Observation 4

I don't understand why doing `nc -l -p 4004` twice at the same time doesn't fail on the 2nd call with something like "already used".
<br>
I thought there will be some sort of checks.
