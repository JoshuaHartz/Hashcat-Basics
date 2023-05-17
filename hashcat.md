# Hashcat

[Hashcat Link](https://hashcat.net/hashcat/)

Hashcat is a popular open-source password recovery and cracking tool used by security professionals, penetration testers, and hackers to recover passwords or perform password cracking attacks. It is designed to utilize the power of modern CPUs and GPUs to efficiently process and crack different types of password hashes.


# What is a Hash?

In computer science and cryptography, a hash is a mathematical function that takes an input (or "message") and produces a fixed-size string of characters, which is typically a sequence of numbers and letters. The output, known as the hash value or hash code, is unique to the input data.

Example: The string "cheetos" when ran through a MD5 Hash Generator is : c4e37f0b4f5b543c3f6ed9a29aa73c91


## Different Types of Hashes.

There are many many many types of hashing algorithms out there. Hashcat supports a fair share of them. I doubt you will use a majority of the hashing algorithms that hashcat supports but its good to know them just in case. To view what hashing algorithms hashcat supports view the link above.


# How to Install Hashcat.

## Linux Tutorial (encouraged)
This section will be done using **Ubuntu**, I imagine the process should be somewhat the same for other flavors of linux. If you are using Kali Linux you do not need to follow along as it comes pre-installed.

1. In your terminal its always good to start with the commands **sudo apt update** and **sudo apt upgrade**

```
sudo apt update
sudo apt upgrade
```
2. Now its time to install Hashcat. We can use the same process above for the hashcat package.
```
sudo apt install hashcat
```
3. Once that is done check that the installation worked by running this command
```
hashcat -h
```
This should dump a whole lot of information and funny buzzwords, dont worry i'll get to those soon.

![image](https://github.com/JoshuaHartz/Summer_Work_hashcat_fork/blob/b852e1b4ff37feb82f1806e32fbd97df71fdb979/hashcat%20images/install_image.PNG)
## Windows Docker Desktop

1. Install Docker Desktop

[Docker Desktop](https://www.docker.com)

2. Download Docker Image

[Github Image Link](https://github.com/FITSEC/docker_images/tree/main/cyber_ops)

Link above also includes instructions for installing the docker. PLEASE READ

What success looks like:
![image](https://github.com/JoshuaHartz/Summer_Work_hashcat_fork/blob/9657beae0faff8419c04ed4db34f22949b8f88e7/hashcat%20images/docker1.PNG)

![image](https://github.com/JoshuaHartz/Summer_Work_hashcat_fork/blob/9657beae0faff8419c04ed4db34f22949b8f88e7/hashcat%20images/docker2.PNG)


# How to use hashcat (basics)

For this section of the tutorial I will be using linux however all commands I do will still work on other ditributions.

You can follow along using this link
[Hashcat Documentation](https://hashcat.net/wiki/doku.php?id=hashcat)

Lets start at the beginning, the options.
## Hash Modes
```
hashcat -m
```
This is where we choose what hash we want to crack. -m is most effective when paired with a hash mode from the list of hash modes (below the options section) If you are unsure of what hash you have, run it through [Hash Checker](https://hashes.com/en/tools/hash_identifier). For now we will only worry about MD5 hashes but remember with -m you can specify any of those numbers if you have those hashes.

Example Usage:
```
hashcat -m 0 
```
The '0' here is the hash code for MD5 hashes. 

## Attack Mode
The next option that we use is the **-a** option. This option specifies the attack-mode that we want to use.
```
hashcat -m 0 -a
```
With -a we have alot less options to choose from unlike -m

![image](https://github.com/JoshuaHartz/Summer_Work_hashcat_fork/blob/2a678ca00965126abb9a3635f14b4d9bd3bed3eb/hashcat%20images/hashcat_attack.PNG)

Attack mode 0 (-a 0) is the Dictionary Attack. A Dictionary attack tries all words in a user specified list against the user given hashes.

Attack mode 1 (-a 1) is the Combinator attack. This attack concatenates words from multiple wordlists and tries them against the hashes.

Attack mode 3 (-a 3) is the Brute Force and mask attack. You will rarely have to use this as this is detrimentally slow since a brute force attack goes through all possible combinations, even with a mask attack its still very slow.

Attack mode 6 (-a 6) is the Hybrid Attack. The hybrid attack cobines wordlists and masks (wordlists + masks).
Attack mode 7 (-a 7) is also a Hybrid Attack. This one however does masks + wordlists.

Attack mode 9 (-a 9) is the Association attack. This attack uses a known component and tries it against the hash.

So thats alot of words and no examples. So im going to show you an example for each.

### Attack mode 0 Dictionary Attack
For this example I will be using the rockyou password breach as my wordlist, and I will randomly choose certain passwords from it to hash. If you do not have the wordlist download it here [Rockyou](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwips6_A8Pf-AhUKjLAFHcdaDkMQFnoECBQQAQ&url=https%3A%2F%2Fgithub.com%2Fbrannondorsey%2Fnaive-hashcat%2Freleases%2Fdownload%2Fdata%2Frockyou.txt&usg=AOvVaw3snAERl1mU6Ccr4WFEazBd)

Hashes:

018a9567ea15470312c40d3e5d6bbcd4

6fb42da0e32e07b61c9f0251fe627a9c

1655c033a3314ba8d5be92931e1caa92

Here is what the hashes look like in a file for hashcat to use:

![image](https://github.com/JoshuaHartz/Summer_Work_hashcat_fork/blob/78a48eb08ceae95f88f3e22d0d89f0017c987fbe/hashcat%20images/hashesfile.PNG)

Try on your own in linux or docker.

General Format:

```
hashcat -m 0 -a 0 <file of hashes> <wordlist>
```
My usage:

```
hashcat -m 0 -a 0 hashes.txt rockyou.txt
```
What runtime looks like:

![image](https://github.com/JoshuaHartz/Summer_Work_hashcat_fork/blob/78a48eb08ceae95f88f3e22d0d89f0017c987fbe/hashcat%20images/dictattack.PNG)

In the image we can see that hashcat found a value for all 3 hashes. You can see this above where it says "session" or you could use an output file option by declaring -o "outfilename"


### Attack mode 1 Combinator Attack
For this exmaple I wrote 2 small wordlists and added them to this repo month_wordlist and year_wordlist. From these wordlists I randomly chose a month + year to hash. Ex: january1669

Try to crack these Hashes:

761bbe168ba6958b86fab51c005844f1

8d51dbdd279cacda9ba00899448c6627

5121cb040026c142f8ce8e73455bee47


General Usage:
```
hashcat -m 0 -a 1 <file of hashes> <wordlist1> <wordlist2>
```
My usage
```
hashcat -m 0 -a 0 hashes2.txt month_wordlist.txt year_wordlist.txt
```

![image](https://github.com/JoshuaHartz/Summer_Work_hashcat_fork/blob/c2e47d2db11fd61f9c8bf1c6453dfb781a38c7df/hashcat%20images/combinationattack.PNG)

### Attack mode 3 Mask Attack
Mask Attack has fully replaced the brute force method. A Mask attack can do everything a brute force attack can do but better. 

But what is a mask? A mask is a simple string that configures the keyspace of the password candidate engine using placeholders.
These placehodlers are:

![image](https://github.com/JoshuaHartz/Summer_Work_hashcat_fork/blob/214792f4ed556dde86160068293a4ac771d922a9/hashcat%20images/charsets.PNG)

For example here, ?l represents a-z lowercase. Hashcat will recognize and try each combination by replacing ?l with a different value each time.

For this example I converted "ndqnoibv" (I slammed on my keyboard to generate this) to a hash. 

The hash is:

9529a0b106bac138927829332a0c42ee

I dont reccomend trying this for yourself as it took me the better half of 20 minutes to complete.

However, if you want too, here is my hashcat command.
```
hashcat -m 0 -a 3 9529a0b106bac138927829332a0c42ee ?l?l?l?l?l?l?l?l
```
there are 8 "?l" because the known password is 8 characters. If you dont know how long the password is, leave it blank.

![image](https://github.com/JoshuaHartz/Summer_Work_hashcat_fork/blob/b1b1196e9c400699705455a5d5e2f0aefa180502/hashcat%20images/brute_attack.PNG)

### Attack mode 6 Hybrid Attack (wordlist + mask)

For this attack mode we are going to use the month wordlist again but this time we are going to simulate the year appended to them with a mask instead of a wordlist. I generated 3 new hashes that have the format month+year. Ex: january8914

Hashes:

000fb96a46d84877a3f22c375ec283c5

64178f6e29d60abc80cfce2061ddbd03

e1c1664115118cf36638385ab222dd10

Example usage:
```
hashcat -m 0 -a 6 <hashfile> <wordlist> <mask>
```
My usage:
```
hashcat -m 0 -a 6 hashes2.txt month_wordlist.txt ?d?d?d?d
```

![image](https://github.com/JoshuaHartz/Summer_Work_hashcat_fork/blob/ef26bdf577087e077583e1107b7661b18c40cb43/hashcat%20images/maskattack1.PNG)

### Attack mode 7 Hybrid Attack (mask + wordlist)

Same concept as the the Attack mode above, however the mask now goes before the wordlist.
```
hashcat -m 0 -a 7 <mask> <wordlist>
```

So if we use last example and pretend that the numbers are before the month (year+month), we get 8914january.

Try These hashes:

33fb23c48de72be0e2269db39cc41dc5

1daa8f91b20a17aaad9b5deb8cd990ee

7c74b5072875e342a08bd2949e7c4925



