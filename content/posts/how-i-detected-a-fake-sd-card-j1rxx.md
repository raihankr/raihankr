---
title: How I Detected a Fake SD Card
slug: how-i-detected-a-fake-sd-card-j1rxx
url: /post/how-i-detected-a-fake-sd-card-j1rxx.html
date: '2026-03-05 18:29:34+07:00'
lastmod: '2026-03-08 16:52:53+07:00'
toc: true
isCJKLanguage: true
---



# How I Detected a Fake SD Card

# Origin Story

　　The story begins around two years ago when I bought this 256 GB SD card from an online marketplace. Well, the price was cheap (I forgot how much exactly) and the rating was good (I think, otherwise I wouldn't bother buying it). Sure it looks weird that it was cheap yet has a big size but the reviews look promising enough.

　　A few days later, the thing arrived, and I couldn't hold myself to migrate all my data to the SD card already. My mistake was that I didn't do any backup in before migrating my data. I don't really remember how it got corrupted. But, I think, the moment I moved all my files from my old SD card to the new SD card simultaneously, I didn't check wether the files are safe or not. And I think, I must have did a cut instead of a copy because what I remember now is that I have lost all my files.

　　Just after I've moved all things, I checked my stuff there. And as you guys may expect, it corrupted. Well, nothing else can I do. I don't sure if i'm able to do a recovery either. I kinda regret my action, but ... what now? Nothing but keep my lives going like before. They're just a bunch of 0s and 1s anyway (I miss them).

## Founding the Problem

　　About last month that I found the corrupted SD card again. I almost completely forgot about what had happened to my data and that SD card. I was just a few steps away from repeating my mistakes. But, luckily, I didn't fall for the same trap, because I kept procrastinating it. Until ...

　　A few days ago, with my friend, I talked about SD cards and how they are almost always have smaller capacity than what they were advertised for—that's because the different in units used by the producer of the SD card and our machine. That was when I decided to gain further information about how SD cards store its data. And then I stumbled upon [this video](https://www.youtube.com/watch?v=4dtD8ZL4YNo).

　　The author of the video experienced similar thing as what had happened to me. He got scammed after buying some cheap flash drives. He explained that scammers usually bought cheap flash drives, label it, and change the code to make it looks like the size were bigger. They then sell with with a slightly higher price to make it look more valuable than what it actually is. That part of the video had me suspicious with my 256 GB SD card.

# Identifying Counterfeit

　　As I'm being suspicious, I searched on the internet for a way to tell wether an SD card is a scam or not. That's when I found this tool called f3. The name stands for Fight Flash Fraud or Fight Fake Flash. 

## Tools

　　This what you gonna need to detect a fake SD card:

- An SD card adapter;
- A Linux or macOS PC/laptop (if you're looking for a Windows alternative, try [H2TestW](https://h2testw.org/)*); and,
- The f3 package**.

　　\* Unfortunately I can't provide a specific guide for it since I'm not using Windows majorly anymore, but their landing page should tells enough of how to use the software

　　** Install from your distribution package manager or follow[ this guide](https://fight-flash-fraud.readthedocs.io/en/latest/introduction.html#installation).

　　‍

## F3 Write/Read Method

　　There are two methods that we can use to detect a fake SD card. The first method is using f3write and f3read

```shell
$ sudo f3write /media/han/7955-BC54
F3 write 8.0
Copyright (C) 2010 Digirati Internet LTDA.
This is free software; see the source for copying conditions.

Removing old file 1.h2w ...
Free space: 249.66 GB
Creating file 1.h2w ... OK!
Creating file 2.h2w ... OK!
Creating file 3.h2w ... OK!
Creating file 4.h2w ... OK!
Creating file 5.h2w ... OK!
Creating file 6.h2w ... OK!
Creating file 7.h2w ... OK!
Creating file 8.h2w ... OK!
Creating file 9.h2w ... 3.50% -- 14.04 MB/s -- 5:08:28
```

## F3 Probe Method

```shell
$ sudo f3probe -nt /dev/sdc
F3 probe 8.0
Copyright (C) 2010 Digirati Internet LTDA.
This is free software; see the source for copying conditions.

WARNING: Probing normally takes from a few seconds to 15 minutes, but
		 it can take longer. Please be patient.

Bad news: The device “/dev/sdc’ is a counterfeit of type limbo

You can "fix" this device using the following command.
f3fix ­--last-sec=526076 /dev/sdc

Device geometry:
           	     *Usable* size: 256.87 MB (526077 blocks)
			  	Announced size: 250.00 GB (524288000 blocks)
						Module: 256.00 GB (2^38 Bytes)
		Approximate cache size: 0.00 Byte (0 blocks), need-reset=no
		   Physical block size: 512.00 Byte (2^9 Bytes)

Probe time: 4.94s
 Operation: total time / count = avg time
	  Read: 131.4ms / 2158 = 60us
	 Write: 4.81s / 2152 = 2.2ms
	 Reset: Ous / 0 - Ous
```

### Success

```shell
$ sudo f3probe -t /dev/sdc
F3 probe 8.0
Copyright (C) 2010 Digirati Internet LTDA.
This is free software; see the source for copying conditions.

WARNING: Probing normally takes from a few seconds to 15 minutes, but
		 it can take longer. Please be patient.

Probe finished, recovering blocks... Done
Good news: The device “/dev/sdc’ is the real thing

Device geometry:
				 *Usable* size: 29.16 GB (61157376 blocks)
				Announced size: 29.16 GB (61157376 blocks)
						Module: 32.00 GB (2^35 Bytes)
		Approximate cache size: 0.00 Byte (0 blocks), need-reset=no
		   Physical block size: 512.00 Byte (2^9 Bytes)

Probe time: 5'52°
 Operation: total time / count = avg time
	  Read: 2'03" / 4197132 = 29us
	 Write: 3'46" / 4192321 = 54us
	 Reset: Ous / 1 = Ous
```

## F3 Fix

```shell
$ sudo f3fix --last-sec=526076 /dev/sdec
F3 fix 8.0
Copyright (C) 2010 Digirati Internet LTDA.
This is free software; see the source for copying conditions.

Drive “/dev/sdc’ was successfully fixed
```
