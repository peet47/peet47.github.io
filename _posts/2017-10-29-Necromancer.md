---
layout: post
title: Necromancer  
categories:
date: 2017-10-29T12:17:23.696+01:00
---
A game between colleagues

First Target: https://www.vulnhub.com/entry/the-necromancer-1,154/

Download an boot the image.

Starting nmap with a quit disappointing result.

<pre>
  <code class="bash">
  Starting Nmap 7.60 ( https://nmap.org ) at 2017-10-29 12:23 CET
Nmap scan report for 192.168.99.100
Host is up (0.00023s latency).
All 1000 scanned ports on 192.168.99.100 are filtered
MAC Address: 08:00:27:DE:4E:19 (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 22.78 seconds
  </code>
</pre>

So the system is locked down. Maybe there is something on the wire?

<img src="images/necromancer/wireshark.png" class="fit image">

Bingo! Something is trying to send me on 4444. So we start listening.

<pre>
  <code class="bash">
netcat -l -p 4444
  ...V2VsY29tZSENCg0KWW91IGZpbmQgeW91cnNlbGYgc3RhcmluZyB0b3dhcmRzIHRoZSBob3Jpem9uLCB3aXRoIG5vdGhpbmcgYnV0IHNpbGVuY2Ugc3Vycm91bmRpbmcgeW91Lg0KWW91IGxvb2sgZWFzdCwgdGhlbiBzb3V0aCwgdGhlbiB3ZXN0LCBhbGwgeW91IGNhbiBzZWUgaXMgYSBncmVhdCB3YXN0ZWxhbmQgb2Ygbm90aGluZ25lc3MuDQoNClR1cm5pbmcgdG8geW91ciBub3J0aCB5b3Ugbm90aWNlIGEgc21hbGwgZmxpY2tlciBvZiBsaWdodCBpbiB0aGUgZGlzdGFuY2UuDQpZb3Ugd2FsayBub3J0aCB0b3dhcmRzIHRoZSBmbGlja2VyIG9mIGxpZ2h0LCBvbmx5IHRvIGJlIHN0b3BwZWQgYnkgc29tZSB0eXBlIG9mIGludmlzaWJsZSBiYXJyaWVyLiAgDQoNClRoZSBhaXIgYXJvdW5kIHlvdSBiZWdpbnMgdG8gZ2V0IHRoaWNrZXIsIGFuZCB5b3VyIGhlYXJ0IGJlZ2lucyB0byBiZWF0IGFnYWluc3QgeW91ciBjaGVzdC4gDQpZb3UgdHVybiB0byB5b3VyIGxlZnQuLiB0aGVuIHRvIHlvdXIgcmlnaHQhICBZb3UgYXJlIHRyYXBwZWQhDQoNCllvdSBmdW1ibGUgdGhyb3VnaCB5b3VyIHBvY2tldHMuLiBub3RoaW5nISAgDQpZb3UgbG9vayBkb3duIGFuZCBzZWUgeW91IGFyZSBzdGFuZGluZyBpbiBzYW5kLiAgDQpEcm9wcGluZyB0byB5b3VyIGtuZWVzIHlvdSBiZWdpbiB0byBkaWcgZnJhbnRpY2FsbHkuDQoNCkFzIHlvdSBkaWcgeW91IG5vdGljZSB0aGUgYmFycmllciBleHRlbmRzIHVuZGVyZ3JvdW5kISAgDQpGcmFudGljYWxseSB5b3Uga2VlcCBkaWdnaW5nIGFuZCBkaWdnaW5nIHVudGlsIHlvdXIgbmFpbHMgc3VkZGVubHkgY2F0Y2ggb24gYW4gb2JqZWN0Lg0KDQpZb3UgZGlnIGZ1cnRoZXIgYW5kIGRpc2NvdmVyIGEgc21hbGwgd29vZGVuIGJveC4gIA0KZmxhZzF7ZTYwNzhiOWIxYWFjOTE1ZDExYjlmZDU5NzkxMDMwYmZ9IGlzIGVuZ3JhdmVkIG9uIHRoZSBsaWQuDQoNCllvdSBvcGVuIHRoZSBib3gsIGFuZCBmaW5kIGEgcGFyY2htZW50IHdpdGggdGhlIGZvbGxvd2luZyB3cml0dGVuIG9uIGl0LiAiQ2hhbnQgdGhlIHN0cmluZyBvZiBmbGFnMSAtIHU2NjYi...
  </code>
</pre>

After digging around with what I got. I was base64 encoded, this took a while...
<pre>
  <code class="bash">
cat list.txt | base64 --decode
  Welcome!

  You find yourself staring towards the horizon, with nothing but silence surrounding you.
  You look east, then south, then west, all you can see is a great wasteland of nothingness.

  Turning to your north you notice a small flicker of light in the distance.
  You walk north towards the flicker of light, only to be stopped by some type of invisible barrier.  

  The air around you begins to get thicker, and your heart begins to beat against your chest.
  You turn to your left.. then to your right!  You are trapped!

  You fumble through your pockets.. nothing!  
  You look down and see you are standing in sand.  
  Dropping to your knees you begin to dig frantically.

  As you dig you notice the barrier extends underground!  
  Frantically you keep digging and digging until your nails suddenly catch on an object.

  You dig further and discover a small wooden box.  
  flag1{e6078b9b1aac915d11b9fd59791030bf} is engraved on the lid.

  You open the box, and find a parchment with the following written on it. "Chant the string of flag1 - u666"Dude:necromancer
  </code>
</pre>

YEAH, first flag.


We move on.
The hint here is:
Chant the string of flag1 - u666

OK, lets give it a try. 666 should be a port.

<pre>
  <code class="bash">
echo e6078b9b1aac915d11b9fd59791030bf | netcat -u 192.168.99.100 666
Chant had no affect! Try in a different tongue!
  </code>
</pre>

Crap... half of the game. After use master google I found that this is a MD5 summe.
e6078b9b1aac915d11b9fd59791030bf = opensesame

Lets give it another try.

<pre>
  <code class="bash">
  echo opensesame | netcat -u 192.168.99.100 666

  A loud crack of thunder sounds as you are knocked to your feet!

  Dazed, you start to feel fresh air entering your lungs.

  You are free!

  In front of you written in the sand are the words:

  flag2{c39cd4df8f2e35d20d92c2e44de5f7c6}

  As you stand to your feet you notice that you can no longer see the flicker of light in the distance.

  You turn frantically looking in all directions until suddenly, a murder of crows appear on the horizon.

  As they get closer you can see one of the crows is grasping on to an object. As the sun hits the object, shards of light beam from its surface.

  The birds get closer, and closer, and closer.

  Staring up at the crows you can see they are in a formation.

  Squinting your eyes from the light coming from the object, you can see the formation looks like the numeral 80.

  As quickly as the birds appeared, they have left you once again.... alone... tortured by the deafening sound of silence.

  </code>
</pre>

YEAH, second flag.

Next. The story mentioned that there was a formation like "numeral 80"
Just to be sure... nmap

<pre>
  <code class="bash">
  Starting Nmap 7.60 ( https://nmap.org ) at 2017-10-29 13:06 CET
Nmap scan report for 192.168.99.100
Host is up (0.00026s latency).
Not shown: 999 filtered ports
PORT   STATE SERVICE
80/tcp open  http
MAC Address: 08:00:27:DE:4E:19 (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 5.03 seconds

  </code>
</pre>

So connect via tcp/80 to the VM.

<img src="images/necromancer/tcp_80.png" class="fit image">
