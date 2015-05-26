# Introduction #

Svwar is a free SIP PBX extension line scanner. In concept it works similar to traditional wardialers by guessing a range of extensions or a given list of extensions.

Svwar can:
  * identify extensions on PBXs and through SIP proxies
  * Scan for large ranges of numeric extensions
  * Scan for extensions using a file containing a list of possible extension names
  * Use different SIP request methods for scanning since not all PBX servers behave the same
  * resume previous scans

# Svwar Usage #
```
usage: svwar.py [options] target
examples:
svwar.py -e100-999 10.0.0.1
svwar.py -d dictionary.txt 10.0.0.2


options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -v, --verbose         Increase verbosity
  -q, --quiet           Quiet mode
  -s NAME, --save=NAME  save the session. Has the benefit of allowing you to
                        resume a previous scan and allows you to export scans
  --resume=NAME         resume a previous scan
  -p PORT, --port=PORT  destination port of the SIP UA
  -t SELECTTIME, --timeout=SELECTTIME
                        timeout for the select() function. Change this if
                        you're losing packets
  -d DICTIONARY, --dictionary=DICTIONARY
                        specify a dictionary file with possible extension
                        names
  -m OPTIONS, --method=OPTIONS
                        specify a request method. The default is REGISTER.
                        Other possible methods are OPTIONS and INVITE
  -e RANGE, --extensions=RANGE
                        specify an extension or extension range  example: -e
                        100-999,1000-1500,9999
  -z PADDING, --zeropadding=PADDING
                        the number of zeros used to padd the username.
                        the options "-e 1-9999 -z 4" would give 0001 0002 0003
                        ... 9999
  -c, --enablecompact   enable compact mode. Makes packets smaller but
                        possibly less compatable
  -R, --reportback      Send the author an exception traceback. Currently
                        sends the command line parameters and the traceback
  --force               Force scan, ignoring initial sanity checks.
```

# Target #

Svwar requires the user to pass a target, which can either be an ip address or a destination hostname.

### Examples ###
```
./svwar 10.0.0.1
```

To specify a hostname instead of the IP:
```
./svwar siphost.com
```

# Options #

## Save ##
The save option allows users to store the current session properties to a database. You can then make use of svreport to manage the sessions and export to other formats. Refer to SvreportUsage page for this.

### Example ###
```
./svwar -s session1 10.0.0.1
```

This also serves the purpose of being compatible with the input and resume options.

## Resume ##
Resumes a previously incomplete session. To list sessions make use of "svreport.py list". When a session is saved, svwar will periodically save the current state and also save the state upon exit.

### Example ###
```
./svwar --resume session1
```

## Destination port ##
By default, most SIP devices listen on the udp port 5060. However some SIP phones might listen on a high port. Make use of svmap to scan for ports which speak SIP on a target address. You can then pass the non-standard port to svwar by specifying "-p" option.

### Example ###
```
./svwar -p5061 10.0.0.1
```

## Timeout or Select time ##
This option allows you to specify the timeout for the select() function. If the network is slow, then it is recommended that you set this to something higher than the default. The default is 0.005. Try with 0.01 first, and start increasing.

### Example ###
```
./svwar -t 0.1 10.0.0.1
```

## Compact mode ##
SIP supports compact mode, where some headers can be written in short form. By default this is disabled because some devices might not support it.

### Example ###
```
./svwar -c 10.0.0.1
```

## Method ##
By default, war uses the REGISTER method. However some devices might not reveal existing extensions through this method. You may specify a different method to scan with, such as OPTIONS and INVITE. Note that INVITE can be noisy and generate a "ring" at the other end.  For a list of method consult with the relevant RFCs or the [wikipedia page](http://en.wikipedia.org/wiki/SIP_Requests).

### Example ###
```
./svwar -m INVITE 10.0.0.1
```

## Verbose ##
The verbose gives you more info. If you need to view all debug information, then specify -vv instead of -v.

### Example ###
```
./svwar -vv 10.0.0.1
```

## Quiet ##
Quiet mode does not print anything except for critical errors. Be sure to save to a session if you want to still view the results later on.

### Example ###
```
./svwar -q 10.0.0.1
```


## Report Back ##
This option allows the end user to send a bug report to the author

### Example ###
```
./svwar -R 10.0.0.1 -s test
```

## Force scan ##
Svwar does a sanity check before it starts scanning, to make sure that a PBX server is really listening at the target. This option overrides the sanity check.


### Example ###
```
./svwar --force 10.0.0.1
```

## Extension range mode ##
By default svwar will try to guess numeric ranges between 100 and 999. You can specify the ranges by making use of the following format:
start-end,start2-end2,start3-end3,...

### Example ###
```
./svwar -e 1-99,1000-9999,150-200 1.0.0.1
```

## Zero padding ##
When making use of the extension range mode, possible extension numbers can be padded with a given number of zeros. For example, with options -z4, when trying extension number 1 the extension would be 0001.

### Example ###
```
./svwar -z4 -e1-9999 10.0.0.1
```

## Dictionary guessing mode ##
Dictionary refers to a text file with a list of possible extension names. This allows for alphanumeric PBX extensions.

### Example ###
```
./svwar -d dictionary.txt 10.0.0.1
```




# Further examples #

Scan from 00000 to 99999 using padding, and save the session to 'session1':
```
./svwar -s session1 -e0-99999 -z5 10.0.0.1
```

Scanning for extensions using a dictionary file with verbose enabled:
```
./svwar -d dictionary.txt 10.0.0.1  -v
```