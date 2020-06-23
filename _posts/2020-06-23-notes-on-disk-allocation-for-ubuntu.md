---
layout: post
title: Notes on disk space allocation for Ubuntu Desktop (and also how to debug your disk space issues)
published: true
---

So a notification appeared informing me I was running out of space. The last time I set up Ubuntu, I didn't know how much space I needed to allocate to each partition. So I winged it. Here's how my disk space looks like about 8 months later and what I'd probably do for my next Ubuntu installation setup.

To give some context, I'm primarily an operations engineer who also does development work. I work a lot with Docker images which ended up being the problem.

Anyhow, here is what I did to get to the root cause.

To check which drive was running out of space, I used:

```sh
sudo df -h | egrep -e '^/dev' | grep -v /loop;
```

This yielded the file system partitions recognised and used by Ubuntu:

```
Filesystem       Size  Used Avail Use% Mounted on
udev             6.8G     0  6.8G   0% /dev
/dev/nvme0n1p1   256M   35M  222M  14% /boot/efi
/dev/nvme0n1p5    92G   88G     0 100% /
/dev/nvme0n1p7   550G   86G  436G  17% /home
/dev/nvme0n1p8    59G  6.3G   50G  12% /usr
/dev/nvme0n1p9    30G  1.6G   27G   6% /opt
/dev/nvme0n1p10   30G   64M   28G   1% /tmp
```

So I ran out of space on the root filesystem.

To get an overview of all the partitions, I used:

```sh
sudo lsblk;
```

```
nvme0n1      259:0    0 953.9G  0 disk 
├─nvme0n1p1  259:1    0   260M  0 part /boot/efi
├─nvme0n1p2  259:2    0    16M  0 part 
├─nvme0n1p3  259:3    0  48.8G  0 part 
├─nvme0n1p4  259:4    0  1000M  0 part 
├─nvme0n1p5  259:5    0  93.1G  0 part /
├─nvme0n1p6  259:6    0  29.8G  0 part [SWAP]
├─nvme0n1p7  259:7    0 558.8G  0 part /home
├─nvme0n1p8  259:8    0  59.6G  0 part /usr
├─nvme0n1p9  259:9    0  29.8G  0 part /opt
└─nvme0n1p10 259:10   0  29.8G  0 part /tmp
```

`nvme0n1p2-4` are the Windows partitions that I kept so I could do a dual boot.

So what was in the root filesystem? To dig deeper, I naviated to `/` and ran:

```sh
sudo du -d 1 -m .
```

This will yield a list of directories with their sizes in megabytes. For my system, I drilled it down to `/var/lib/docker`:

```
15      /var/lib/docker/containers
1       /var/lib/docker/network
1       /var/lib/docker/builder
356     /var/lib/docker/swarm
210     /var/lib/docker/image
14788   /var/lib/docker/volumes
1       /var/lib/docker/tmp
1       /var/lib/docker/runtimes
1       /var/lib/docker/trust
62631   /var/lib/docker/overlay2
1       /var/lib/docker/buildkit
1       /var/lib/docker/plugins
77999   /var/lib/docker
```

Docker alone uses 77999 megabytes.

Using `echo $(($(docker images | wc -l) - 1))`, I have 343 images (-1 for the headers row).

Using `echo $(($(docker volume ls | wc -l) - 1))`, I have 49 volumes.

Anyway, moral of the story, allocate more space to `/var` when doing disk partitioning.
