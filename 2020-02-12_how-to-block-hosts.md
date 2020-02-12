# How to block hosts

Edit your `/etc/hosts` file by adding the hosts you want to block and
redirect them to your local IP.

```bash
$ vim /etc/hosts
```

This is an example of my hosts file.

```bash
$ cat /etc/hosts
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1	localhost
255.255.255.255	broadcasthost
::1             localhost
# Added by Docker Desktop
# To allow the same kube context to work on the host and the container:
127.0.0.1 kubernetes.docker.internal
# End of section
127.0.0.1	watsonplatform.net
127.0.0.1	gateway.watsonplatform.net
127.0.0.1	chatbase-area120.appspot.com
```

I blocked my outgoing connections because I was trying to configure an nginx
server and had to isolate the connection being made. You can test whether
your block is working by using `ping`.

```bash
$ ping https://chatbase-area120.appspot.com
PING https://chatbase-area120.appspot.com (127.0.0.1): 56 data bytes
64 bytes from 127.0.0.1: icmp_seq=0 ttl=64 time=0.048 ms
```
