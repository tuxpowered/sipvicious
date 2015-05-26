Svmap is a free and Open Source scanner to identify sip devices and PBX servers on a target network. It can also be helpful for systems administrators when used as a network inventory tool. Svmap was designed to be faster than the competition by specifically targeting SIP over UDP.

Svmap can:
  * identify SIP devices and PBX servers on default and non-default ports
  * scan large ranges of networks
  * scan just one host on different ports, looking for a SIP service on that host or just multiple hosts on multiple ports
  * take previous scan results as input, allowing you to only scan known hosts running SIP
  * use different scanning methods (make use of REGISTER instead of OPTIONS request)
  * get all the phones on a network to ring at the same time (using INVITE as method)
  * randomly scan internet ranges
  * resume previous scans

# Svmap usage #

```
Usage: svmap.py [options] host1 host2 hostrange
examples:
svmap.py 10.0.0.1-10.0.0.255 \
> 172.16.131.1 sipvicious.org/22 10.0.1.1/24 \
> 1.1.1.1-20 1.1.2-20.* 4.1.*.*
svmap.py -s session1 --randomize 10.0.0.1/8
svmap.py --resume session1 -v
svmap.py -p5060-5062 10.0.0.3-20 -m INVITE


Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -v, --verbose         Increase verbosity
  -q, --quiet           Quiet mode
  -s NAME, --save=NAME  save the session. Has the benefit of allowing you to
                        resume a previous scan and allows you to export scans
  --resume=NAME         resume a previous scan
  --randomscan          Scan random IP addresses
  -i scan1, --input=scan1
                        Scan IPs which were found in a previous scan. Pass the
                        session name as the argument
  -p PORT, --port=PORT  Destination port or port ranges of the SIP device - eg
                        -p5060,5061,8000-8100
  -P PORT, --localport=PORT
                        Source port for our packets
  -x IP, --externalip=IP
                        IP Address to use as the external ip. Specify this if
                        you have multiple interfaces or if you are behind NAT
  -b BINDINGIP, --bindingip=BINDINGIP
                        By default we bind to all interfaces. This option
                        overrides that and binds to the specified ip address
  -t SELECTTIME, --timeout=SELECTTIME
                        Timeout for the select() function. Change this if
                        you're losing packets
  -c, --enablecompact   enable compact mode. Makes packets smaller but
                        possibly less compatable
  -m METHOD, --method=METHOD
                        Specify the request method - by default this is
                        OPTIONS.
  -R, --reportback      Send the author an exception traceback. Currently
                        sends the command line parameters and the traceback
  --randomize           Randomize scanning instead of scanning consecutive ip
                        addresses
```

# Target IP addresses #

To specify a range of IP address, one can make use of the CIDR notation. For example to scan the 1.1.1.0 subnet one would run the following command:
```
./svmap 1.1.1.1/24
```

You can also specify a name instead of an IP address:
```
./svmap sipvicious.org
```

.. and also use CIDR notation with the name:
```
./svmap sipvicious.org/24
```

Another way of specifying a custom range is to make use of "-", for example:
```
./svmap 1.1.1.50-1.1.1.60
```

Or you could use a shorter method:
```
./svmap 1.1.1.1-20
```

Could also use a wildcard:
```
./svmap 1.1.*.*
```

And Finally one can combine any of these methods:
```
./svmap 1.1.1.1-20 1.1.2.* sipvicious.org/24
```

If you would like to randomly scan internet ranges, the syntax is:
```
./svmap --randomscan
```

If however, you would like to scan a range randomly:
```
./svmap --randomize sipvicious.org/24
```

# Options #

## Save ##
The save option allows users to store the current session properties to a database. You can then make use of svreport to manage the sessions and export to other formats. Refer to SvreportUsage page for this.

Example:
```
./svmap -s session1 1.0.0.1/8
```

This also serves the purpose of being compatible with the input and resume options.

## Resume ##
Resumes a previously incomplete session. To list sessions make use of "svreport.py list". When a session is saved, svmap will periodically save the current state and also save the state upon exit.

```
./svmap --resume session1
```

## Input ##
The input option accepts previous sessions as input. Make use of svreport to list previous scans. It serves the purpose of being able to scan specific devices on specific ports. This allows security testers to scan the same devices at different times using different methods.

One particular usage example is to scan for SIP devices using default options. Then at a given time scan for the SIP devices found in the previous scan using the INVITE method, which can get all the scanned devices to ring at the same time.

```
./svmap -i session1 -v
```

## Random Scan ##
The --randomscan option scans internet ranges to SIP devices. It avoids non routable (internal and reserved) IP addresses.

```
./svmap --randomscan 
```

## Randomize Scan ##
The --randomize option randomizes the given ranges of IPs instead of scanning sequentially.

```
./svmap 1.0.0.1/24 10.0.0.1/24
```

## Destination port ##
By default, most SIP devices listen on the udp port 5060. However some SIP phones might listen on a high port. For example, X-lite is known to listen on "random" high ports. In that case, you can use ranges of ports to find out the port on which the SIP device is listening on.

```
./svmap -p5061,5080-5090 10.0.0.1-2
```

## Source port ##
By default, svmap listens on udp port 5060. However there are times when that port is already taken and svmap cannot bind on the default port. When this is the case, SIPVicious tools will listen on the next available port. However, in the case that one wishes to specify a port, one can make use of the -P option to specify another udp port to bind to.

```
./svmap -P5666 10.0.0.1
```

## Timeout or Select time ##
This option allows you to specify the timeout for the select() function. If the network is slow, then it is recommended that you set this to something higher than the default. The default is 0.005. Try with 0.01 first, and start increasing.

```
./svmap -t0.1 1.1.1.1
```

## Compact mode ##
SIP supports compact mode, where some headers can be written in short form. By default this is disabled because some devices might not support it.


```
./svmap -c 101.10.1.1
```

## Method ##
By default, svmap uses the OPTIONS method. However some devices might not support this method (even though they should). You may specify a different method to scan with, such as REGISTER and INVITE. Note that INVITE can be noisy and generate a "ring" at the other end.  For a list of method consult with the relevant RFCs or the [wikipedia page](http://en.wikipedia.org/wiki/SIP_Requests).

```
./svmap -m INVITE 1.1.1.1
```

## External IP ##
This option allows you to specify the external IP address which is used in the SIP request itself.

```
./svmap -x 88.11.1.1 1.1.1.1
```

## Binding IP ##
This option allows you to specify the IP to bind to.

```
./svmap -b 127.0.0.1 127.0.0.1
```

## Verbose ##
The verbose gives you more info. If you need to view all debug information, then specify -vv instead of -v.

```
./svmap -vv 1.1.1.1
```

## Quiet ##
Quiet mode does not print anything except for critical errors. Be sure to save to a session if you want to still view the results later on.

```
./svmap -q 10.1.1.1
```

## Report Back ##
This option allows the end user to send a bug report to the author

```
./svmap -R 1.1.1.1
```

# Further examples #

Scan a subnet with verbose mode:
```
./svmap 10.0.0.1/24 -v
```

Scan a subnet running compact mode on port range 1024-2080:
```
./svmap 10.0.0.1/24 -p1024-2080 -c
```

Scan a subnet and save the results to a session named "scan1":
```
./svmap -s scan1 10.0.0.1/24
```

Scan a list of previously scanned hosts and make use of the INVITE method:
```
./svmap -i scan1 -m INVITE
```