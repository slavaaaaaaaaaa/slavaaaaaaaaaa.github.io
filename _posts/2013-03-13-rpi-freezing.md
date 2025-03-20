---
type: posts
tag: blag
layout: post
title: Freezing a Raspberry Pi
---
Just over 12 years ago, my dad and I dunked a Raspberry Pi model B (first generation) in liquid nitrogen. This is the original blog post I wrote on the subject. I added formatting, fixed a few grammar errors, added some link descriptions, and rotated an image - everything else is as originally published.

The last copy I have of this post is [included](/assets/posts/rpi-freezing-post.pdf), and here's [the archived article about this by Geek.com](https://web.archive.org/web/20151113155128/http://www.geek.com/chips/raspberry-pi-proven-to-be-stable-when-submerged-in-liquid-nitrogen-1555235/).

You can find my reflections on this [in an upcoming post](/posts/reflections-on-my-first-blog-post).

<!-- toc -->

- [Introduction](#introduction)
- [Setup](#setup)
- [Procedure](#procedure)
- [Conclusion](#conclusion)
- [Next time](#next-time)
- [Comments](#comments)

<!-- tocstop -->

## Introduction

Being subscribed to the RSS feed on [RaspberryPi.org](https://raspberrypi.org), I always wake up to a bunch of things they post. One morning, one of them was (can’t seem to find the actual post now), and linked to [Свежезамороженный Raspberry Pi (Freshly Frozen Raspberry Pi)](https://web.archive.org/web/20200923011158/https://habr.com/ru/post/178647/).

Basically, these guys stuck their Pi into a fridge at -70C and got the core temperature down to -58C without issues.

My dad is a molecular biophysicist and deals with a huge magnet on a daily basis. This bastard is filled with liquid nitrogen and has to be refilled every couple of weeks. So as soon as he heard about this from me, he said we should put it into liquid nitrogen (which has a boiling temperature of -196C).

This experiment went extremely well. I logged our procedure, and ran some stress tests using stress command. Unfortunately, the logs of stress aren’t very helpful, but next time we’ll be better prepared.

## Setup

Liquid nitrogen was poured into a foam container. Raspberry Pi (older model, second revision with 256mb ram) was put into a Korean plastic box, was powered via USB and controlled via SSH over ethernet.

Some of our biggest concerns were:

* plastic on the Raspberry Pi not being able to withstand the rapid change in temperature,
* plastic box not being able to withstand temperature,
* Raspberry Pi freaking out due to condensation,
* SD card freaking out to to drop in temperature and condensation.

Below you can see how it all looks after we took it out the first time:

<img style="max-width: 100%" src="/assets/posts/rpi-freezing-initial-setup.jpg" />

After we got it all figured out and working, we started trying different things.

I’d have separate SSH terminal windows open, showing different stats. During this whole experiment I was writing rushed bash scripts to automate things. One of the things we’ll do better next time is better preparation with better monitoring scripts. Here are some of the stats I was checking throughout the procedure:

```bash
watch -n .5 /opt/vc/bin/vcgencmd measure_temp
```

to watch the temperature at the core;

```bash
while [ true ]; do
    TEMP=$(/opt/vc/bin/vcgencmd measure_temp);
    DATE=$(date +”%m-%d-%y-%T”);
    stress –cpu 2 –io 4 –vm-bytes 64M –timeout 30s –verbose >> stress-$TEMP-$DATE.log;
    sleep 5;
done
```

does the stress command, throws it all in a file and repeats;

```bash
while [ true ]; do
    DATE=$(date +”%m-%d-%y-%T”);
    scp -r /root linkxs@10.1.38.231:~/RPI-temp-testing/$DATE/;
    sleep 15;
done
```

keeps throwing all the logs back to my computer, just in case the SD card decides to burst into blue flames.

The two scripts above were started in a `screen` session, so it can be out of the way. We were also monitoring `uptime` and `top`. Lastly, we kept pinging the Pi to make sure Ethernet is still active, since that’s the one part of the Pi that’s only rated to 0C and up. (“LAN9512 is specified by the manufacturers being qualified from 0°C to 70°C, while the AP is qualified from -40°C to 85°C, as per [Raspberry PI FAQ](http://www.raspberrypi.org/faqs)).

## Procedure

At room temperature, booted up, core temperature was 44C. When the stress command was run, it raised to 46-47C.

First, we decided to put it inside the box into an industrial fridge at -80C. I didn’t take notes then, but the Pi was stable for around 10-15 minutes in there, with, if I recall correctly, ~-75C lowest.

At that point, while running all the scripts above, we put the box into liquid nitrogen.

When the temperature dropped to -78C, ping stopped responding and the Pi seemed to have shut down. We took it out for less than a half a minute, and put it back in. Same thing happened at -82C this time, and next time at -87C.

Now, we rebooted and didn’t start any of the scripts above, only monitored the temperature, uptime and ping. At -105C the `watch` command for temperature got a segfault, while some pings were timing out (approximately every one ping out of 6 was unsuccessful). Several seconds later, the Pi pinged out completely and when we took it out the LEDs weren’t on at all. Then, it seemed to be stuck in a reboot loop for about 10 seconds, until it warmed up a bit and successfully booted.

At +1C, we resubmerged it. This time, the last temperature reading we got was -110C.

Now we put it inside a plastic bag with some industrial drying solvent (rice on crack), since we were afraid that the massive amount of condensation was affecting its performance. Unfortunately, it doesn’t look like I got a picture of it in the bag. The bag went inside the box and then into the foam box with liquid nitrogen.

You can see the condensation here:

<img style="max-width: 45%" src="/assets/posts/rpi-freezing-condensation-1.jpg" />
<img style="max-width: 45%" src="/assets/posts/rpi-freezing-condensation-2.jpg" />

This time when we put it back in, it took it a lot longer to cool down. The cause for that is probably the air in the bag.

It was submerged at -26C, and 6 minutes later it was at -58C. At -87C I started running a script that my friend Adran rushly wrote:

```bash
echo “———————-”
date
temp=$(/opt/vc/bin/vcgencmd measure_temp | awk -F”=” ‘{print $2}’ | awk -F”‘” ‘{print $1}’)
echo “temp.value $temp”
for volt in core sdram_c sdram_i sdram_p
do
    voltage=$(/opt/vc/bin/vcgencmd measure_volts $volt | awk -F”=” ‘{print $2}’ | tr -d “V”)
    echo “volt$volt.value $voltage”
done
echo “———————-”
```

This script was quickly modified from [a `munin` plugin called `pisense`](https://github.com/perception101/pisense) since we didn’t have proper networking. It measured the voltage at the current temperature. Here’s the output for when I periodically ran it:

```bash
root@raspberrypi:~# ./adran.sh
———————-
Sat May 11 22:43:54 UTC 2013
temp.value -77.7
voltcore.value 1.20
voltsdram_c.value 1.20
voltsdram_i.value 1.20
voltsdram_p.value 1.23
———————-
root@raspberrypi:~# ./adran.sh
———————-
Sat May 11 22:45:09 UTC 2013
temp.value -84.7
voltcore.value 1.20
voltsdram_c.value 1.20
voltsdram_i.value 1.20
voltsdram_p.value 1.23
———————-
root@raspberrypi:~# ./adran.sh
———————-
Sat May 11 22:46:04 UTC 2013
temp.value -90.1
voltcore.value 1.20
voltsdram_c.value 1.20
voltsdram_i.value 1.20
voltsdram_p.value 1.23
———————-
root@raspberrypi:~# ./adran.sh
———————-
Sat May 11 22:46:21 UTC 2013
temp.value -91.2
voltcore.value 1.20
voltsdram_c.value 1.20
voltsdram_i.value 1.20
voltsdram_p.value 1.23
———————-
root@raspberrypi:~# ./adran.sh
———————-
Sat May 11 22:47:43 UTC 2013
temp.value -97.6
voltcore.value 1.20
voltsdram_c.value 1.20
voltsdram_i.value 1.20
voltsdram_p.value 1.23
———————-
root@raspberrypi:~#
root@raspberrypi:~# ./adran.sh
———————-
Sat May 11 22:50:00 UTC 2013
temp.value -104.1
voltcore.value 1.20
voltsdram_c.value 1.20
voltsdram_i.value 1.20
voltsdram_p.value 1.23
———————-
root@raspberrypi:~#
```

This time, the Pi shut off at -107C.

This is as far as we went.

## Conclusion

* The Pi is (more or less) stable at -100C.
* Voltage doesn’t significantly change at these temperatures.
* The Raspberry Pi is a **BEAST**.
* This wasn’t logged, so not exactly proven, but I noticed that while running the stress script, the lower the temperature, the higher was the load shown by uptime.

## Next time

Next time, there will be many things we’ll do differently:

* Try the newest model of the Pi – 512mb RAM model, assembled in Britain
* Provide protection from condensation before the tests
* Better logging: premade stable, tested, more efficient scripts
* Will be asking on #raspberrypi on freenode if anyone wants any light scripts to be run at these low temperatures
* Eliminate the SD card (boot to RAM)
* Try using the HDMI/Composite out instead of ethernet, as ethernet seems to be the first thing to go
* Possibly use GPIO for power instead of USB
* Since LN isn’t conductive, it’s theoretically possible to submerge the Pi in there altogether, however the board is likely not to survive the rapid temperature change. It’s a possibility too keep in mind for the future, though.
* Try simulating vacuum, to see how the Pi would react in space

Lastly, here are some more pictures for your satisfaction:

<img style="max-width: 45%" src="/assets/posts/rpi-freezing-extra-1.jpg" />
<img style="max-width: 45%" src="/assets/posts/rpi-freezing-extra-2.jpg" />
<img style="max-width: 45%" src="/assets/posts/rpi-freezing-extra-3.jpg" />
<img style="max-width: 45%" src="/assets/posts/rpi-freezing-extra-4.jpg" />
<img style="max-width: 45%" src="/assets/posts/rpi-freezing-extra-5.jpg" />
<img style="max-width: 45%" src="/assets/posts/rpi-freezing-extra-6.jpg" />

Feel free to comment, email me, and be sure to visit [http://fucktre.es](https://fucktre.es/) – my more important blog to raise awareness of asshole trees.

## Comments

#### Pingback: Cooling a Raspberry Pi with Liquid Nitrogen | Shea Silverman's Blog

#### Pingback: Cooling a Raspberry Pi with Liquid Nitrogen | Raspberry World

#### *swiss* May 15, 2013 at 12:33 am

Why did you use while [ true ]… just while true

- *linkxs* Post author May 15, 2013 at 12:35 am

    Because bad at bash :/ Was doing it rushly and don’t do enough bash scripting to remember proper syntax, so everything was googled on the spot. While my pi was in a -196C tub. Yeah, I didn’t have much time to figure out if I need [ ] or not :)

#### *Anonymous* June 1, 2013 at 7:40 am

while :; is even shorter to type in bash

#### *informatimago* May 15, 2013 at 3:21 pm

Next step: overclock it! Raspberry Pi @ 5 GHz :-)

* *linkxs* Post author May 15, 2013 at 6:51 pm

    Holy crap, 5ghz! Yeah let’s run Crysis on it! ;)

#### *Tpo* May 15, 2013 at 5:06 pm

ALthough you wouldn’t be able to directly submerge it, next time laminate the board maybe to protect against condensation. And if you cooled it in the box first Im sure you could then put it in the nitrogen after. Doubt it would still be running by then though!

* *linkxs* Post author May 15, 2013 at 6:52 pm

    Heh, good idea!

#### *kroell* May 15, 2013 at 8:27 pm

Try put it in a bag with mineral oil before submerging. Should be nonconductive and replenishes eventually water on the board.

* *Camilo Martin* May 17, 2013 at 6:29 pm

    This actually sounds like a great idea!

#### *Ryan Dobie-Watt* May 17, 2013 at 3:49 am

Wouldn’t dehumidifying the air around the Pi prevent condensation as well? Perhaps instead of air, fill the container with the industrial drying solvent. Vacuum would pose an interesting challenge, since it would insulate the Pi, making it harder to cool.

#### *lokir* May 17, 2013 at 5:00 am

Great idea :) Try to overclock Your PI to 1.2 Ghz and then freez it.

#### *andy* May 17, 2013 at 7:59 am

OMG next time video it pls!

#### *hackbyte* May 17, 2013 at 8:50 am

AWESOME ;)

I mean, a really cool idea. I’m curious what you can get out of a supercooled Pi next time, with better preparation of your experiments.

In example, using a serial (RS232 oder any other simple-) link for external logging (maybe just in plaintext to avoid failures in tcp/ip overhead) can get you way beyond the point where ethernet begins to fail.

Additionally, it could be nice to get some wireless links (WLAN, BT, simple Serial-RF modules) to test if it even gonna fail at these low temperatures (which even happens regularily in space, if i remember correctly). ;)

You just got a new fan. ;)

Greetings from Wilhelmsburg, Hamburg, Germany, Europe, Earh, Sol-System, Milkyway. ;)

Hacky

#### *Alan* May 17, 2013 at 1:36 pm

You can try to put it on the Moon surface to avoid condensation problem!

#### *Ed Plitt* May 20, 2013 at 10:13 pm

Nigel Griffiths wrote a monitoring tool called nmon, that is ported to the pi.

You can get it via apt-get

There is a mode you can run which will record performance data to a text file, which can then be displayed in Excel, including graphs. I don’t think it records the temp information, but you seem to have that covered already. Adding the temp data capture to the nmon data would be great.

Please let me know if I can help

Thanks,

Ed

* *linkxs* Post author May 21, 2013 at 10:46 pm

    That sounds useful! We’ll definitely take a look at that before doing the next run!
