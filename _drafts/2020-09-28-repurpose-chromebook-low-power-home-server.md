---
layout: post
title:  "Repurposing my old Chromebook to a low power home server"
date:   2020-09-28 7:30:00 +0100
categories: repurpose asus c300 chromebook home server chromeos linux docker containers
---

## Can't sell it? Repurpose it!
It was around 4 years ago when I bought a used, slightly beat up Asus C300 Chromebook from a friend of mine for £50. It served me well, I installed Linux on it and used it to work on some side projects while commuting. However as time passed by, I slowly forgot about it and it ended up in a box, being put away. I always thought it's a shame to just let it sit there, but I don't wanted to sell it for pennies and neither I wanted to throw it out as it is a perfectly fine machine for some lightweight tasks. I always had the idea to convert it to a home server (yes, with a 7W TDP Celeron N2830 CPU), but couldn't really find a guide online that explains how to do it and neither I had the urge to do the experimenting myself, up until now.

## The setup

[![Chromebook home server][chromebook_image]][chromebook_image]{:target="_blank"}

- 1x Asus C300 Chromebook
- 1x 128GB USB drive
- 1x vertical stand
- 1x GalliumOS installed

In order to run a generic home server, I knew ChromeOS will just not cut it, it has to be proper Linux. The distribution didn't really matter as I was going to use Docker anyways (and docker-compose) to run all server applications in containers. Fortunately I already had experience installing GalliumOS, therefore it became my distribution of choice. GalliumOS is an Ubuntu based distro, specifically created to be running on ChromeOS devices. I had to update the firmware on my Chromebook (it's a very simple, less than 10 steps process) then install the OS using a tool called chrx (another 2-3 steps). Once done, I was able to dual boot my Chromebook and got a fully operational Linux laptop.

## Software configuration

Once I had the OS up and running, I wanted to configure it so I can use it in closed-display (clamshell) mode as leaving it running with the display open wouldn't be too practical. I knew MacBooks had these option, but I also knew you had to have an external display and keyboard/mouse connected to it in order to make it work. Luckily with Linux, the only limit is your imagination (and your coding/Google-ing skills)! Since GalliumOS is Ubuntu based as mentioned before, I was certain there are people using Ubuntu who want their system to stay awake even after the shut the display. And I was not wrong. Simply editing `logind.conf` and turning off all power savings and auto-sleep functions through the GUI did the job.

The next thing I had to install was sshd, but thanks to this Reddit post I found out it was as simple as `apt install`-ing it (again, it's basically a skinny version of Ubuntu). Left all settings to default, then I SSH-ed from another box. All went smooth, I logged in from the remote machine, started `htop` and closed the lid to see that the configuration actually works! I left the connection open for about an hour, just to be 100% sure no sleep mode will kick in, then I considered the project a success. Was much smoother than I anticipated.

## Servers 'n containers

Although I would **_not recommend_** running any production workload on a _Chromebook_ via _Wifi_ connection, it'll serve perfectly fine as my home cron/CI server, especially knowing it can run on batteries for 10 hours (allegedly), without having a UPS. Following the documentation I installed Docker with no problem whatsoever and docker-compose as well. I also plugged in a tiny 128GB USB thumb drive I found laying around on my desk, as the built in eMMC is not the most spacious storage ever made. I re-formatted the drive to ext4, then mounted and finally updated Docker to use the partition on the USB device for data storage, which includes all containers, volumes, etc.
In case you're curious, please see the list of the containerised servers I used with some explanation to the whys below.

### Portainer

Website: [https://www.portainer.io/](https://www.portainer.io/)

Portainer provides a very user-friendly way to manage containers on remote boxes. Via a web interface, it provides information about resource utilisation, logs, etc. for each container.

### Grafana + InfluxDB + Telegraph
Grafana website: [https://grafana.com/](https://grafana.com/)

InfluxDB website: [https://www.influxdata.com/](https://www.influxdata.com/)

Telegraf website: [https://www.influxdata.com/time-series-platform/telegraf/](https://www.influxdata.com/time-series-platform/telegraf/)

I use this combo to monitor average packet loss, ping, DNS query time, CPU temperature and CPU/Memory/Disk utilisation.

### Jenkins CI
Website: [https://www.jenkins.io/](https://www.jenkins.io/)

I use Jenkins for all the cron/CI jobs I have, to build/deploy and run automated tests on some of my side projects.

### Sonatype Nexus
Website: [https://www.sonatype.com/nexus/repository-oss](https://www.sonatype.com/nexus/repository-oss)

To cut back on network utilisation, I've set Nexus up as well as a local docker/npm/pypi cache.

### Future improvements
I'll stop at this point as I reached my goal with this project (as always, the journey was more important than the result itself), this little device has loads of potential with it's 2x USB ports, a full size HDMI port and even an audio jack. At the end of the day, I feel much better to put this neat little tech in use, and convinced myself I don't need to buy _yet another_ RaspberryPI for a future project that'll never come.

---

Resources used:

[ASUS Chromebook C300](https://uk.store.asus.com/asus-chromebook-c300ma-13-3-light-weight-laptop-intel-dual-core-2gb-32gb-emmc.html)

[Intel Celeron N2830](https://ark.intel.com/content/www/us/en/ark/products/81071/intel-celeron-processor-n2830-1m-cache-up-to-2-41-ghz.html)

[GalliumOS](https://galliumos.org/)

[Installing GalliumOS](https://wiki.galliumos.org/Installing)

[chrx](https://chrx.org/)

[How can I tell Ubuntu to do nothing when I close my laptop lid?](https://askubuntu.com/a/372616)

[Does GalliumOS not come with ssh abilities installed?](https://www.reddit.com/r/GalliumOS/comments/5b7vwi/does_galliumos_not_come_with_ssh_abilities/d9mx0ie/?utm_source=reddit&utm_medium=web2x&context=3)

[How to change the default location for “docker create volume” command?](https://stackoverflow.com/a/52018760)

[chromebook_image]: https://tamas.dev/static/2020-09-28/chromebook.jpg