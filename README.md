# crimediggers
Solving the challenges of the Dutch National Police

## Intro
The Dutch National Police is hosting a website with challenges for security and IT professionals.
I have done my best to solve the challenge and will explain how I managed to solve these challenges.

### OSINT
The OSINT challenge was to find out the name of the person behind an email address: Z3r0b3t404@gmail.com.  

#### Step one
Filling in the e-mail at google login and google calendar to figure out whether the account had a name registered to it failed.  

#### Step two
Decode the leet-speak to generic text: zerobeta04@gmail.com, a search for this lead to a domain name zerobeta04.nl    

#### Step three
Check the robots.txt of this website (http://zerobeta04.nl/robots.txt) where an entry existed called notes.txt  

#### Step four
In the robots.txt was mentioned the following "Boggle, moet nog weg" and an url leading to second-hand selling platform Marktplaats (http://web.archive.org/web/20191129084244/https://www.marktplaats.nl/a/telecommunicatie/mobiele-telefoons-blackberry/m1487205624-pgp-blackberry.html).  

#### Step five
Checking out the marktplaats showed that the article was removed.  

#### Step six
Using the wayback machine I was able to retrieve the article showing the seller showed the seller JG_zerobeta04 from alledgedly Stitswerd, active for 7 months (since 2019 November 29) on the platform is selling a PGP phone.  

#### Step seven
JG are maybe initials for his name, the seller mentions looking for Accordeon Hohner Starlet.  

#### Step eight
Using some tricky search with Google and the wayback machine on marktplaats.nl with articles mentioning Hohner starlet from this period I was able to retrieve a account "jiuseppegrande" selling the accordeon with the same logo as seen in the previous images of the other user.  

#### Step nine
Searching google for this user lead to a twitter account: jiuseppe_grande. A user with a bio containing hex-encoded "Ransom". Sifting through his account you can see this person is possibly involved in ransomware attacks.  

#### Step 10
jiuseppe_grande tweeted a picture matching the Boggle as seen earlier on marktplaats, a reverse image lookup on Google lead to a website "http://joshgrootegast.nl/elloco_backup" where his name "Josh Grootegast" from Almere was seen.  

#### Step 11
As a final link to confirm that this person is indeed a match his email appeared on the website as well: z3r0.b3t4.04@gmail.com.  

#### Step 12
Josh Grootegast is confirmed to be the person in question we were looking for.  

### Reverse engineering

#### Step 1
In the challenge a timestamp was given: October 28, 2019 3:23AM.

#### Step 2
The javascript was beautified using duckduckgo for improved readability.  
The javascript function takes one argument t which is an acronym for time.  
The javascript was used at the time given, thus all new Date() objects need to be replaced with date objects using the original time the code was generated.

#### Step 3
I first tried to convert this date to an js time value in integer, however this did not give me the correct code.
Next I tried in regular date string format dd-mm-yyyy hh:mm solving the challenge.


### TCP/IP

#### Step one
A packet capture file was provided from a network tap.  
I loaded this file into Wireshark, which is common software for packet captures.  

#### Step two
I filtered on http requests and saw the suspect was browsing the internet for "how to blackmail" his boss indicating the person was up to no good.  

The person seemed to look for the Tor browser / darkweb and ransomware indicating this person would be attempting to acquire ransomware to blackmail his employer.  

#### Step three
Not much more http traffic was captured, however when filtering on pop3 and smtp protocol I retrieved e-mail communicating with a seller for ransomware from the deepweb, the suspect paid BTC and received the malware as a zip file encoded in base64 and the password to decrypt it.  

Decrypting the zip file gave a text document with a text indicating the suspect got scammed and the # flag for this challenge was in this message.

### Pentest

#### Step one
For this challenge a virtual machine was made available.  
Installing this virtual machine into vmware workstation and booting it showed the existence of Josh user and a Jenkins user.  
However, as we don't know the password we have to use a side-channel attack to gain root.  

#### Step two
Resetting a linux machine multiple times will lead to the grub menu for recovery options.  

Doing this the user has the ability to change the boot commands for grub by pressing e.  

A side-channel attack is to change the linux kernel boot arguments from ro to rw to make the rootfilesysem read-write and to add init=/bin/sh so that the first program that is executed upon boot will be a shell and because the user context is root at that time it will spawn a root shell.  

#### Step three
We have two options: either to change the password of any user since we are root or to just browse the user directories.  
I decided to leave the user passwords because passive reconaissance is generally better than active reconaissance.  
The home directory of the Z3R0b3T4 user aka "Josh" contained a file on the desktop named TODO.txt and one named ransomware.py.  

#### Step four
Apparently from reading the TODO.txt the goal of this challenge was to login to jenkins, a platform for developers as there was no password protection existent yet.  

However, my way of solving this challenge is much faster and leaves no traces on the system.  

The python script ransomware.py contained code intended to encrypt files with a hardcoded key "Th@nksFORYoUrM0n3Y!".  

Furthermore it contained the Bitcoin address for the payment: 14ighask(... ommited to not reveal flag).

### GEO
yet to be solved

### Crypto

#### Step one
Dowloading the provided raw image showed that this image is a raw linux file system dump.
I first loaded the image into autopsy and analyzed it.  
Soon I discovered the existence of .ecryptfs in /home which hints that the user has an encrypted container on this system.  

#### Step two
Janny is a user on the system and in the ecryptfs folder inside janny we could see that a wrapped-passphrase exists.  
This passphrase together with the password of Janny can be used to recover the private directory that is now encypted.  

##### Step three
Password recovery seemed like a good option to me as we are able to unshadow /etc/passwd /etc/shadow.  
John is a utility that can be used to crack weak linux passwords and within minutes it showed that janny used the password "princess".  
Now as we can not decrypt this image outside a live system and chroot won't have an ecryptfs kernel module loaded the image needed to be converted to a virtual disk for vmware.  
This can be done through qemu-img convert -O vmdk Janny-PC.raw Janny-PC.vmdk but may take some time.  

