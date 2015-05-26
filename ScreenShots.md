_this page is incomplete_

# Introduction #

Since this is a command line utility, there's no need for images ;)

# svmap #

When run without any options:

```
$ ./svmap.py 
usage: svmap.py [options] host1 host2 hostrange
examples:
svmap.py 10.0.0.1-10.0.0.255 \
> 172.16.131.1 sipvicious.org/22 10.0.1.1/24 \
> 1.1.1.1-20 1.1.2-20.* 4.1.*.*
svmap.py -s session1 --randomize 10.0.0.1/8
svmap.py --resume session1 -v
svmap.py -p5060-5062 10.0.0.3-20 -m INVITE


svmap.py: error: Provide at least one target
```

A normal svmap scan:

```
./svmap.py 1.1.1.1
| SIP Device          | User Agent   | Fingerprint |
----------------------------------------------------
| 1.1.1.1:5060        | Asterisk PBX | Asterisk    |
```