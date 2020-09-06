# crimediggers
Solving the challenges of the Dutch National Police

## Intro
The Dutch National Police is hosting a website with challenges for security and IT professionals.
I have done my best to solve the challenge and will explain how I managed to solve these challenges.

### OSINT
The OSINT challenge was to find out the name of the person behind an email address: Z3r0b3t404@gmail.com.  
Step one: Filling in the e-mail at google login and google calendar to figure out whether the account had a name registered to it failed.  
Step two: Decode the leet-speak to generic text: zerobeta04@gmail.com, a search for this lead to a domain name zerobeta04.nl    
Step three: Check the robots.txt of this website (http://zerobeta04.nl/robots.txt) where an entry existed called notes.txt  
Step four: In the robots.txt was mentioned the following "Boggle, moet nog weg" and an url leading to second-hand selling platform Marktplaats (http://web.archive.org/web/20191129084244/https://www.marktplaats.nl/a/telecommunicatie/mobiele-telefoons-blackberry/m1487205624-pgp-blackberry.html).  
Step five: Checking out the marktplaats showed that the article was removed.  
Step six: using the wayback machine I was able to retrieve the article showing the seller showed the seller JG_zerobeta04 from alledgedly Stitswerd, active for 7 months (since 2019 November 29) on the platform is selling a PGP phone.  
Step seven: JG are maybe initials for his name, the seller mentions looking for Accordeon Hohner Starlet.  
Step eight: Using some tricky search with Google and the wayback machine on marktplaats.nl with articles mentioning Hohner starlet from this period I was able to retrieve a account "jiuseppegrande" selling the accordeon with the same logo as seen in the previous images of the other user.  
Step nine: Searching google for this user lead to a twitter account: jiuseppe_grande. A user with a bio containing hex-encoded "Ransom". Sifting through his account you can see this person is possibly involved in ransomware attacks.  
Step 10: jiuseppe_grande tweeted a picture matching the Boggle as seen earlier on marktplaats, a reverse image lookup on Google lead to a website "http://joshgrootegast.nl/elloco_backup" where his name "Josh Grootegast" from Almere was seen.  
Step 11: As a final link to confirm that this person is indeed a match his email appeared on the website as well: z3r0.b3t4.04@gmail.com.  
Step 12: Josh Grootegast is confirmed to be the person in question we were looking for.  
