### What does `svcrash.py` do? ###

It sends a SIP message response to `svwar.py` which triggers an unhandled exception. This may allow victims of SIP floods due to attackers using `svwar.py` to mitigate the attack temporarily. The bug in `svwar.py` was also fixed. Additionally, the behavior that allowed it to keep sending messages even when not responses are received was also changed.

### Does SIPVicious have a backdoor? ###

Nope - no backdoors. The code is open source and open to inspection. It did have a bug that causes a crash when handling malformed tags. This is what `svcrash.py` abuses.


### I do not think this is the right solution. Don't you? ###

I agree - this only addresses a symptom caused by the problem. Denial of service is a real problem and does not apply only to VoIP providers. Take a look at what others have done in other areas if this becomes a real issue.
Online gambling sites etc have been hit with such attacks since their infancy.

`svcrash` does however block the attack temporarily. We hope that this helps if this is costing you precious bandwidth.

Note: This is obviously not a long term solution.


### Won't the attackers catch up and fix the bug? ###

I expect unofficial fixes for old versions of `svwar` and `svcrack` in the near future. Keep in mind that new versions of SIPVicious (`svwar` and `svcrack`) try not flood the network when the tool receives no response.

This bug is fixed in the latest versions (containing `svcrash`).

The logic: flooding VoIP providers doesn't do anyone good (granted that the attackers want free phone calls). Therefore the timeout added in SIPVicious version 0.2.5 is actually beneficial for both the victims and the attackers.

### So SIPVicious can be used for DoS? ###

Yes. So can lots of useful & powerful tools like netcat and one can pull a more powerful DoS using such tools.

If what attackers want is to DoS (bandwidth saturation attack) a network, then they should use other tools (that send large UDP packets for example).


### Attackers can bypass this tool by doing X Y Z, you know? ###

Yep! Hopefully those attackers are also smart enough to know that flooding a network is not the way to make phonecalls for free. Heck, I hope they get a real job and stop bothering your network ;-)

The tool `svcrash.py` is meant to temporarily stop network floods caused by old versions of unmodified SIPVicious.


### Can I use the tool for fun? ###

I suppose you could seriously mess up some penetration testers :->

### How can I start svcrash.py automatically on bootup? ###

I suggest that you consult your server documentation. Every OS and Linux distribution, BSD flavor etc has its interesting tidbits.