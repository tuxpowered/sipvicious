# general #
  * others : improve documentation
  * feature: STUN support - NAT is a major PITA
  * feature: change behavior of -R. When an exception is not caught give option to send a bug report. When -R is specified, send output (needs to be part of logging) no matter what.
  * cosmetic: check if windows and then restrict width of output tables.
  * Add ACK support to each tool
  * Make sure that each SIP message is well formed
  * Make some major changes in the fingerprinting - maybe use simple parsing instead of regular expressions in some cases.

# svmap #
  * support for NAPTR
  * support for e164

# svcrack #
  * feature: scan multiple users
  * feature: intelligent scan - sequentially perform the following attacks:
    * username and modifications of that as password
    * numeric bruteforce
    * dictionary attack
    * alphanumeric bruteforce

# svreport #
  * add sipvicious logo to reports

# svlearnfp #
  * add a place for users to add comments

# future #
  * feature: work more on fingerprinting **in svn**
