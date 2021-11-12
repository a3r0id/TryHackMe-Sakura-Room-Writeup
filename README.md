# TryHackMe Sakura Room Writeup
 A quick writeup on TryHackMe.com's Sakura Room by The OSINT Dojo

## Official Video Walkthrough by OSINT Dojo

[Official Walkthrough By The OSINT Dojo | Steps 1-3](https://www.youtube.com/watch?v=bR0NJn1Xqog)

[Subscribe To OSINT Dojo on Youtube](https://www.youtube.com/watch?v=bR0NJn1Xqog)

## Links:

[Sakura Room](https://tryhackme.com/room/sakura)

[osintdojo.com](https://www.osintdojo.com/)

[My THM Profile](https://tryhackme.com/p/hostinfosec)
![](https://tryhackme-badges.s3.amazonaws.com/hostinfosec.png)

![](https://raw.githubusercontent.com/hostinfodev/TryHackMe-Sakura-Room-Writeup/main/images/hostinfosec.png)

The following is a write-up on TryHackMe.com's room called “Sakura” by OSINT Dojo. This write-up will not necessarily solve the room for you but instead should give you a bit more insight on how you can go about the challenge based on my experience while completing the room. Keep reading to get the full scoop of what I learned and how I completed the task. I am in no sense a professional OSINT practitioner and this write-up should not be taken as industry-standard advice or procedural learning but simply as a complimentary sharing of my knowledge.

Upon getting started on the room, you’ll be introduced to a juicy backstory which includes an imaginary incident in which The OSINT Dojo was hacked and a single image was left behind by the attacker. Great, a typical, edgy black-hat who simply wants to cause havoc. Let’s go ahead and enumerate all of the information that we have been provided with.

The instructions will give you a huge jump on the investigation so let’s make things interesting and completely ignore them for now and just work off of the initial information that has been provided. Our first step in any OSINT investigation is to aggregate all the information that we have on our target to one central notebook/enumeration software. This, being a blog, will pretty much constrain us to text and imagery but I will also be linking some online tools/resources as this investigation will not require any software to complete and can be done by using only online services, making this an easy-to-follow write-up.
Go ahead and write the URL of the image down in a new empty document in your notebook so we don’t have to keep jumping back to the room for information. You should practice this with every piece of information you gather during an investigation so we aren’t loosing leads or mixing up data. This also ensures we have an organized case to present to upper management/law enforcement (if needed). 

# [1] TIP-OFF

Upon first glance, the image is fairly basic and not much can be said about it’s author. Initially, I’ll look for useful information in the presentation (the actual photographic representation) of an image file before subsequently checking for metadata & hidden information within the image file itself. Scouring the presentation of an image typically consists of looking for readable text, symbols, faces and in some cases pin-able geographic features but can go even further in-depth with advanced steganography practices which search for patterns/watermarks in the image that might not be apparent to the human eye or even when decode/decrypted (multi-layered abstraction) which we will not get into today, for the sake of me remaining young while writing this.

Since we haven’t found much on the presentation side of things let’s see what’s under the hood.
During my experience, I quickly resorted to examining the image file for artifacts. Since “sakurapwnedletter.svg”’s file-type is SVG, we can expect a decent amount of information since Scalable Vector Graphics (.SVG) is an XML-based file that is not in binary/encrypted. This means we can open the file in a text-editor and read all of the data contained without needing to decode it or execute it in an another program.

Upon opening the file in a text-editor of your choice, you’ll quickly notice you’ve hit gold and that the attacker has made a grave mistake. Among all the markdown chaos there is one very important string, the author’s Linux username, which is included in the file’s export directory.

I’m not going to give this one away as it is directly a flag so let’s refer to this as “{username-1}”.

Artifacts:
- https://raw.githubusercontent.com/OsintDojo/OsintDojo.github.io/d846483eb41dd4fdb6d00ac84ecdb4a66be6a191/TryHackMe/Sakura/sakurapwnedletter.svg
- inkscape:export-filename="/home/xxxxxxxxxx/Desktop/pwnedletter.png"

Resources:
- https://en.wikipedia.org/wiki/Scalable_Vector_Graphics#Compression
- https://developer.mozilla.org/en-US/docs/Web/SVG/Element/metadata


# [3] RECONNAISSANCE

Now that we have our first lead, let’s move forward and see where it takes us.
When given a username in an investigation, my first move is a simple Google search, which in fact is one of the most effective OSINT tools in existence. Google scrapes and archives almost every site on the web, and effectively every popular site in general. A quick search of {username-1} returns two major social media links to {username-1} which consists of Reddit user @{username-1} and a jp.linkedin.com account that is also handled by a “{username-1}”. The Reddit account has not posted and doesn’t have any info provided besides a birthday, April 13th. The birthday might prove useful in a real-world investigation but in this case, to save time and be reasonable, we will deem this a false-positive. The Linkedin account proved otherwise as it contains a plethora of information and appears to represent a genuine individual. We have successfully found a full name that is consistent with the {username-1} as well as some other identifying information such as work/educational history. In a real-world investigation, this would not generally mean we have positively identified our attacker but for the sake of this write-up and it’s simplicity, just go ahead and try the full name as a flag, after all, it is one of the questions.

I’m not going to give this one away as it is directly a flag so let’s refer to the full name as “{Jane Doe}”.

Now that we’ve determined that {Jane Doe} is our attacker, we know who we are looking for and we know what her history is but we still aren’t certain about many key aspects of {Jane Doe}.

I decided to dig a bit further into {Jane Doe}’s online presence, instead of turning to public records as this investigation is not based on a real individual or situation. All were false-positives. 

https://www.linkedin.com/in/{username-1}/?originalSubdomain=jp

To do a deeper dive into {username-1}, I used a great online service, https://whatsmyname.app/, which allows you to enumerate usernames across many websites, many of which, a Google search won’t find.
This brought my attention to a Github account handled by {username-1}, which featured two uploaded repositories and a few forked crypto-related repositories. The repository that stood out to me is https://github.com/{username-1}/PGP which, like you’d imagine, contains a PGP key.

Since PGP keys are essentially a blob of strings/bytes that are encoded in base64 encoding I simply used an online service to decode the PGP key, https://www.base64decode.org/, which gave me an email in raw text and answered Task #3 - 1.
That is the last we will see of this email.

Artifacts:
- https://www.linkedin.com/in/{username-1}/?originalSubdomain=jp
- https://github.com/{username-1-Lowercase}
- https://github.com/{username-1-Lowercase}/PGP

Resources:
- https://whatsmyname.app/
- https://protonmail.com/blog/what-is-pgp-encryption/


# [4] UNVEIL

Task #4 asks for information about a crypto-wallet and hints that the information lies in {Jane Doe}’s Github account. Upon inspecting the attacker’s Contribution Activity on Github, I noticed a deleted repository titled “ETH”. The repo had two commits, the first of which seems to contain an accidental leak of the attacker’s mining pool URL, including an Ethereum address + password as well as hinting the mining pool that the attacker used. This severity of an  accidental information disclosure seems extremely unlikely but is more common in the wild than one might expect. We are all human.

The first 3/4 flags of Task #4 can already be answered with the information we have, now let’s preform some recon on the wallet itself.
Since we have the attacker’s wallet address, we can view all of their transactions as all Cryptocurrency transactions are OSINT.
Using https://etherscan.io, we can see the wallet’s entire history, leading to the remaining flags of Task #4.

![](https://github.com/hostinfodev/TryHackMe-Sakura-Room-Writeup/blob/main/images/wallet.png)

![](https://raw.githubusercontent.com/hostinfodev/TryHackMe-Sakura-Room-Writeup/main/images/wallet_pool_txn.png)

Artifacts:
- https://github.com/{username-1-Lowercase}/ETH/commit/5d83f7bb37c2048bb5b9eb29bb95ec1603c40135#diff-ed62f5e8bb5f88d470bd6a8aa3cf3c18ad1be17b29153b4896f45e7e57cfb5da
- stratum://{WALLET_ADDRESS}.{USERNAME}:{PASSWORD}@{POOL_ADDRESS}:{PORT}
- https://etherscan.io/address/{WALLET_ADDRESS}

Resources:
- https://etherscan.io

# [5] TAUNT

After examining the screenshot of the Twitter exchange between our attacker and the OSINT Dojo Twitter account, 
it stands out that the attacker is using another handle that we didn’t know about, “AikoAbe3”.
Just as with the last username, I started with a basic Google search. Upon searching Google, it seemed that I’d hit a dead with the new username as all results were false positives. Now remember when I said Google search is potentially the most useful tool in your OSINT toolbox? Yea.. When a user posts something on Twitter, Google adds that post to their searchable data, which includes all of the text from the post + media and metadata, including the username/handle at the time of indexing. Imagine how powerful your investigation skills could be if you could simply tell Google to fetch results across a huge portion of the web that only contain a specific string of text!? Great News, you can! To search for a string of text inside all page bodies, use “allintext:YOUR SEARCH”. There’s actually many useful Google Search Operators that make your queries more accurate and are a go-to for any OSINT researcher. Upon searching in text, the first result should be a hit.

I’m not going to give this one away as it is directly a flag so let’s refer to the new Twitter username as “{username-2}”.

The next flag calls for a link to where the attacker saved their WiFi passwords and SSIDs.
Upon scrolling through {username-2}’s Twitter account, I noticed a post that contains the text “Regular WiFi and Passwords”.
In the attacker’s post, it also hints towards “DEEP” & “PASTE” which hints towards a deep web pastebin-like service. To get the latest working links to hidden paste services lets head to https://deepweblinks.net/pastebin/.
Upon trying a few of them all were down except http://depastedihrn3jtw.onion, which seems to match the screenshot taken by the attacker. This hidden service is called “Deep Paste” and allows users to paste any text they want, so you can image it’s a cesspool of the worst of the web, let’s just stay off of there for today.

![](https://raw.githubusercontent.com/hostinfodev/TryHackMe-Sakura-Room-Writeup/main/images/EsdhaUSVkAAM803.png)

When I searched the MD5 hash (0a5c6e136a98a60b8a21643ce8c15a74) from the screenshot on Deep Paste I successfully found the paste which contained the following text:

```
Saving here so I do not forget

School WiFi Computer Lab: 			GTRI-Device		GTgfettt4422!@
Mcdonalds: 					Buffalo-G-19D0-1        Macdonalds2020
School WiFi: 					GTvisitor		GTFree123
City Free WiFi: 				HIROSAKI_Free_Wi-Fi 	H_Free934!
Home WiFi: 					DK1F-G			Fsdf324T@@
```

It is safe to assume that we have found the post and it’s URL(http://depastedihrn3jtw.onion/show.php?md5={THE_HASH_FROM_EARLIER}) which is in fact a flag.

The next flag calls for the attackers “HOME” BSSID, this sounds like a long shot but with modern wardriving/WAP mapping techniques, but a lot can actually be dug up on a great service called https://wigle.net. Not every Wireless Access Point is mapped on Wigle but it still contains a huge amount. On Wigle, I simply entered the attackers home SSID into the filter and was quickly given a single result that led to a city in Japan, (Edit: you need to manually move the map to japan and look for the single result which will be a tiny purple dot, depending on your current zoom) the BSSID was directly above the SSID which was the flag as well as the City in the next challenge as that is the city by which the attacker considers “HOME”.

![](https://raw.githubusercontent.com/hostinfodev/TryHackMe-Sakura-Room-Writeup/main/images/wigle.png)

New Developments:
- https://raw.githubusercontent.com/OsintDojo/OsintDojo.github.io/main/TryHackMe/Sakura/taunt.png

Artifacts: 
- https://www.google.com/search?q=allintext%3AAikoAbe3
- https://twitter.com/{username-2}
- https://twitter.com/{username-2}/status/1353157336538386432/photo/1
- http://depastedihrn3jtw.onion/show.php?md5={THE_HASH_FROM_EARLIER}

Resources:
- https://moz.com/learn/seo/search-operators
- https://deepweblinks.net/pastebin/
- https://wigle.net/


# [6] HOMEBOUND

There’s a few more posts that contain images on the attacker’s Twitter that give us some great geographic insight on the attacker’s whereabouts.

Firstly, the attacker has retweeted a picture involving cherry blossoms in Bethesda, Maryland which is located just northwest of Washington, D.C. 

Maybe this is where the attacker was at one point and even possibly the location where she carried out the attack. In another post, the attacker has actually uploaded their own media, which looks to be genuinely taken by the attacker and shows some vital geographic features, even a unique monumental landmark. In the lower-middle region of the image you can barely make out the Washington Monument at the National Mall in Washington D.C, USA.

The first flag of Task #6 calls for the three-letter airport code that is nearest to this image in which the attacker has boarded a flight. At this point, all we need to do is find the nearest airport to the Washington Monument.

Another image that the attacker uploaded contains the readable text “JAL - First Class Lounge - Sakura Lounge”. Let’s go back to trusty Google search and see what we can find, let’s try searching “JAL+First Class Lounge+Sakura Lounge”.
The top result should be “https://www.jal.co.jp/en/inter/service/lounge/hnd/first/”, which, upon review of the lounge, seems to match the the photograph, therefore this is probably a flag! 

Lastly, the attacker has posted a map on their Twitter that is quoted “Sooo close to home! Can't wait to finally be back! :)”. Task #6 - Flag #3 asks for ”the name of the lake that can be seen in the map shared by the attacker as they were on their final flight home ”. Since we know home is most likely in Japan. Let’s pull up maps.google.com. Controlling the map view, fly over to Japan and flip on Satellite view. I tried this image against a Google image search in hopes of identifying the exact region depicted in the image but came up with limited results. There is also online services such as https://discover.digitalglobe.com/ that allow you to search by “shapefile/KML” which can be extremely useful but we won’t be getting into that now. Luckily this image displays a fairly large region and is in exceptional quality so we can manually search on Google maps to match the map with a region and ultimately figure out what lake this is. Personally, I’ll look for the largest landmark when searching from above. In this case we’ll use the island on the left, which turns out to be Sado Island.
Once Sado Island is found on Google Maps, we can move our attention inland and behold, we have found our lake and ultimately the 2nd to last flag!

Now the last flag I’ll let you do on your own, remember what we found on Wigle?
The BSSID that we found belonged to a Wireless Access Point in **what** city?

Good luck!

Artifacts:
- https://twitter.com/{username-2}/status/1355364359090757635
- https://twitter.com/ArielleLBaker/status/1353483416159858688/retweets
- https://twitter.com/{username-2}/status/1353733617487241217/photo/1
- https://twitter.com/{username-2}/status/1353717763097899010/photo/1
- https://twitter.com/{username-2}/status/1353471045148110848/photo/1

Resources:
- https://maps.google.com
- Your Prefrontal Cortex

# Conclusion:

I hope you enjoyed this write-up and learned a few new basic skills.
I’d like to personally thank OSINT Dojo for creating an awesome room that is pure OSINT and I hope to see more in the future!

This is literally my first write-up as well as my first completed room on THM.
I had a great time sharing my experience with you all so this definitely won’t be my last.

For any edit requests/feedback, or simply to say hey, please dm me on Discord @Aero#9306.

If you would like to buy me a coffee:
https://ko-fi.com/hostinfodev.

-Aero


















