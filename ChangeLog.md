# v.0.2.8 #
  * Feature: INVITE sends a BYE and supports ACK
  * Feature: man pages can be produced with --manpage and man pages are included
  * Bug fix: removed fingerprinting completely
  * Change: moved pptable.py and svhelper to libs/
  * Change: Number of changes to adhere to Debian's guidelines (copyright/license notices etc)
  * Bug fix: fixed an svcrack unhandled exception
# v0.2.7 #
  * Feature: svcrash.py has a new option -b which bruteforces the attacker's port
  * Feature: svcrack.py now tries the extension as password by default, automatically
  * Feature: svcrack.py and svwar.py now support setting of source port
  * Feature: new parameter --domain can be passed to all tools which specifies a custom domain in the SIP uri instead of the destination IP
  * Feature: new --debug switch which shows the messages recieved
  * Bug fix: Sometimes nonces could not be extracted due to an incorrect regex
  * Bug fix: Fixed an unhandled exception when decoding tags
  * Bug fix: now using hashlib when available instead of md5
  * Bug fix: removed the space after the SIP address in the From header which led to newer version of Asterisk to ignore the SIP messages
  * Bug fix: dictionaries with new lines made svcrack.py stop without this fix
  * Change: renamed everything to start with sv
  * Bug fix: changed the way shelved files are opened by the fingerprinting module
  * Change: fingerprinting disabled by default since it was giving too many problems and very little benefits

# v0.2.6 #
  * Feature: svcrash.py introduced - aimed at mitigating attacks by svwar.py and svcrack.py
  * Bug fix: helper.py updated to prevent choking on malformed tags


# v0.2.5 #
  * Feature:  svwar.py has "scan for default / typical extensions" option. This option tries to guess numeric extensions which have certain patterns such as 1212 etc. Option is -D, --enabledefaults
  * General:  svwar.py and svcrack.py now have a new option which allows you to set how long the tools will scan without receiving any response back. This allows us to prevent flooding the target. Some PBX servers now have built-in firewalls / intrusion prevention systems which will blacklist the IP address of anyone using svwar or svcrack. Therefore if the IP is blacklisted it makes sense to stop scanning the target. The default for this option is 10 seconds. Set this option by using --maximumtime [[seconds](seconds.md)]
  * Removed:  svlearnfp.py is now discontinued. The tool is still included for historic reasons but disabled.
  * Feature:  svmap.py now includes the following new features:
    * --debug - shows messages as they are received (useful for developers)
    * --first - scans the first X number of hosts, useful for random or large address pool scanning
    * --inputtext - scans IP ranges taken from a text file
    * --fromname - sets the from header to something specific useful for abusing other security issues or when svmap is used in a more flexible way then usual ;-)
  * Feature:  svreport.py now has two new modes:
    * - stats, which lists some statistics
    * - search, allows you to search through logs looking for specific user agents
  * Bug fix:  svwar.py now by default does not send ACK messages (was a buggy feature that did not follow the standard)
  * Bug fix:  svwar.py - the template passed through --template option is now checked sanity.


# v0.2.4 #
  * Feature:  svwar.py can now scan for templated numbers. This allows more flexible usage of ranges of numbers, allowing for prefixes and suffixes as need be ;-)
  * Bug fix:  svwar.py now sends ACK to be nice to other devices.
  * Bug fix:  each tag is padded with a unique 32 bit
  * Bug fix:  Contact header is always added to the request to always send well formed SIP requests
  * Bug fix:  Large data is sent fragmented now (mysendto)
  * Bug fix:  svwar.py now handles new SIP response codes


# v0.2.3 #

  * Feature: Fingerprinting support for svmap. Included fphelper.py and 3 databases used for fingerprinting.
  * Feature: Added svlearnfp.py which allows one to add new signatures to db and send them to the author.
  * Feature: Added DNS SRV check to svmap. Use ./svmap.py --srv domainname.com to give it a try


# v0.2.svn #

  * Feature: added the ability for svreport to count results when doing a list
  * Bug fix: fixed a bug related to resuming a scan which does not have an extension

# v0.2.1 (maintenance) #

## General ##

  * Feature:  updated the report function to include more information about the system. Python version and operating system is now included in the bug report. option now supports optional feedback.

  * Feature:  Store information about the state of a session. Sessions can be complete or incomplete, so that you can resume incomplete sessions but not complete ones.

  * Feature:  Added -e option to svmap. Allows you to specify an extension. This is useful when using -m INVITE options on a SIP phone.

  * Bug fix:  Added a check to make sure that the python version is supported. Anything less than version 2.4 is not supported

  * Bug fix:  IP in the SIP msg was being set to localhost when not explicitly set. This is not correct behavior and was fixed. As a result of this behavior some devices, such as Grandstream BT100 were not being detected. Thanks to robert&someone from bulgaria for reporting this

  * Bug fix:  fixed a bug in the database which was reported anonymously via the --reportback / -R option. Thanks whoever reported that. Bug concerns the dbm which does not support certain methods supported other database modules referenced	by anydbm. Reproduced on FreeBSD. Thanks to Anthony Williams for help i dentifying this

  * Bug fix:  Ranges of extensions in svwar could not take long numeric extensions (xrange does not support long / large numbers). Thanks to Joern for reporting this

  * Bug fix:  svwar was truncating extension names containing certain characters. Fixed.

  * Bug fix:  when binding to a specific interface, the IP within the SIP message could be incorrect (when there are multiple interfaces). This has been fixed.

  * Cosmetic: Certain PBXs reply with "603 Declined" when svwar finds that the extension does not exist. This creates extra noise. It is now being suppressed.

# v0.2 #

## Major changes ##
  * Session support which allows you to resume previous scans as well as store the results in database format
  * Exporting of previous results to various formats: pdf, xml (html), csv and plain text
  * Easy updating by making use of subversion (svn update)
  * Better UI, more intuitive help, clean output and more debug info when needed
  * And my favorite feature: random scanning techniques


## general ##
  * feature: when bind port is already busy give error and choose next one
  * feature: verbose and quiet mode
  * bug fix: reviewed all code for logic bugs and consistency between tools
  * feature: output for all tools
  * feature: resume previous scan
  * feature: update sipvicious tools by executing svn update

## svmap ##
  * feature: random scan and randomize
  * feature: ip range helper:
    * support 1.1.1.1-20, 1.1.1.1-1.1.2.30 and 1.1.1.1/24 ranges
    * support multiple ranges: ./svmap 1.1.1.1/24 1.1.2.1-20

## svwar ##
  * bug fix: listen for responses after done scanning

## svreport ##
  * take input from a database produced by sv tools
  * output to xml, pdf, csv, stdout
  * able to resolve dns for ip addresses
  * list scans
  * delete scans


# v0.1 #
First release.