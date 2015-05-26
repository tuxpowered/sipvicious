### Why SIPVicious? ###

Because the tools are not exactly the nicest thing on earth next to a SIP device. And the play on the sound seems to work. As an extra bonus, it rhymes with the name of Sex Pistol's bass player.

### Why did you publish tools that can be used for illegal purposes? ###

The idea behind the tools is to aid administrators and security folks make informed decisions when evaluating the security of their SIP-based servers and devices. The tools are intended to be used for educational and demonstrational purposes. We advise people to request permission before making use of the tool suite against any network. Just like a knife, it can be used for good and bad. We hope that SIPVicious tool suite proves to be a very sharp one.

### I'm running `svwar` against a PBX server and not getting any results ###

That might mean two things:
  1. You do not have any valid extensions in your guess method. If you know any valid extension on the target PBX try that out and make sure that it shows up in the results. You can specify numeric ranges to try via the `-e` (or `-r`) option, or dictionary files containing possible extension names via the `-d` option.
  1. You're running `svwar` against a PBX that does not differentiate responses between existing and non-existing extensions. You can try different methods, such as INVITE and see what comes up.

`svwar` has an option `--force`. This should be used in lab environment when one is testing a SIP device for behavior quirks. Using this in live environment may create unnecessary network traffic.


### `svwar` refuses to scan - Am I doing something wrong? ###

This is similar to the previous question.

Example:

```
./svwar.py target -e 1001-1010
ERROR:TakeASip:SIP server replied with an authentication request for an unknown extension. Set --force to force a scan.
WARNING:root:found nothing
```

A number of PBX systems don't follow the SIP RFC and have implemented protection against enumeration. In Asterisk, you can enable this protection by setting the following in your `sip.conf`:

```
alwaysauthreject=yes
```

Once that protection is enabled, `svwar` does not work by default. Note that other enumeration methods may still exist.


### Why does `svcrack.py` send so many REGISTER messages for the same extensions but only few of them actually trying Auth? ###

This is done to generate a new nonce because on some systems, the nonce expires. However not all servers expire the nonce so if you would like to reuse the nonce, `svcrack` has an option `-n`.


### What is a stateless scanner? ###

A stateless scanner does not keep the state in conventional computer memory. This can make scanning large ranges more efficient in terms of resource usage. The way that this works in SIPVicious is that the tools send SIP messages and store the state in those messages. When SIPVicious tools receive a response back, then they decode the state in the response and determine the result.

In the case of `svmap`, `svwar` and `svcrack`, the data is stored in the tags in the "To" or "From" header.

### I have a question about `svcrash` ###

Check out the SvcrashFrequentlyAskedQuestions

### Why is my question not here? ###

Probably because my telepathic skills leave a lot to be desired. Don't hesitate to email me sandro@enablesecurity.com

### Can we hire you? ###

Sure, I am available for (legit) pentesting and interesting security projects especially in the UK and Malta. Get in touch [mailto:sandro@enablesecurity.com](mailto:sandro@enablesecurity.com).