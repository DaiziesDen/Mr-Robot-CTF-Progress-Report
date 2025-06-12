Bee-boop. Mr. Robot Progress Report

Yo! Hi all. This is my Mr. Robot write-up and my very first attempt at capturing flags without any follow-along videos. I will use other write-ups for guidance if I truly get stuck or have exhausted all of my options via Google recon. This will take some time, as I just had my first child. So, with time off from work, I have one month to find all the flags. I just hope I can finish before then. Prayers up!

Day One – May 18th

After installing Mr. Robot, I entered my Linux credentials to see if I could access it. I then ran Ifconfig to find the IP address of the host system: NAT Network 192.168.x.x and Bridged 10.0.0.x

I conducted an Nmap scan on 192.168.x.0/24 using -sV (Services) and -A (OS and Version Detection). I checked each IP in the Firefox browser until I discovered the Mr. Robot IP.

Once I found Mr. Robot's IP, I inserted it into Firefox and conducted recon. The author wrote a friendly message about learning from them and included a few commands. Prepare revealed a video about finances and the Mr.Robot movement. Fsociety presented a question about moving forward. Inform displays international scandals. Question provided statements regarding false patriotism, capitalism, executives, and businessmen. Wakeup is another video with men talking aggressively and Join simply asks for your email address. I was a bit skeptical of “join”, but I took the chance and entered one of my old emails. It disappeared, leaving a message behind: “I'll be in touch” message. After doing some recon on that specifically, I found out it’s just a gimmick to pay homage to the show.

I right-clicked on “View Source Page” and checked the inspector for any suspicious data or anything out of place then I checked the rest of Nmap’s results and saw that port 22 was closed and ports 80 and 443 were open. Both ran Apache and did not have a title (txt/html). So, I decided to go over a pen test checklist I found on LinkedIn a few days ago to help stay on track and learn. 

I checked the robots.txt file by adding \robots.txt to the end of the IP in the Firefox browser, and I discovered this:
![Capture d'écran 2025-05-18 214334](https://github.com/user-attachments/assets/c51b5ff8-cf36-4ebd-b62d-efd86bfb9bbb)

 
I tried the same things for sitemap.xml and the humans.txt files. Neither yielded any valuable data. 

Day Two – May 20th
Wooh. Day two. 

I input the key-1-of-3.txt file at the end of the IP and received a string of numbers and letters. 
![Capture d'écran 2025-05-20 224525](https://github.com/user-attachments/assets/c7c32fb6-091c-4d8c-88a7-2982299d3a98)


I looked at the string for a while. I thought it looked like a hash. I have minimal experience using hash functions aside from a homework assignment in my master's program, but I remember being just as puzzled. So, it eventually clicked. I tried a decoder for all hash types. No takers. So, maybe it isn't a hash???

I tried the same string in the encoder, and it produced a value: 46e0b76caa96401382bf2ac1fa62xxxx
After trying to force it to work in the browser and playing encoder–decoder tag via Base64, URL, MD5, SAML, I circled back to “maybe it isn’t a hash???”. So, maybe it’s a key or something that will lead me to another key. I’m moving on.

I went back to the robots.txt file, copied the \fsocity.dic, and inserted it into the browser, and was met with never-ending scrolling. Tons of data. It could be a list of critical usernames, passwords, or something to throw me off. I am not sure if they will be of use, but I created and saved the file using vi. It could be useful later on.

Day Three – May 23rd

I wanted to use gobuster as I have in the past, but I forgot how it works. So, I watched some YouTube videos to explain it to me. I ended up using [Samantha's Mr.Robot write-up](https://samanthactf.medium.com/tryhackme-mr-robot-ctf-37f95341d926) instead, as it was simpler to digest. Learning these commands will be fun for sure. 


I just found out that -- help explains the letters and their functions in the commands to you. This is a lifesaver.

I did not have dirb or the wordlists set up on my Kali machine, so I downloaded the wordlists from GitHub and found dirb in another directory. I moved dirb into the share folder and then added the GitHub repository file into the dirb directory, and bam, finished. So, I ran gobuster dir -e -u 10.0.0.x -w /usr/share/dirb/wordlists/common.txt -x txt,php,py,cgi
I think txt, php, py, and cgi are the types of files to search through? I might be wrong, but that makes the most sense to me. 

The gobuster command displayed 800 thousand pieces of data to sift through, so I’m going to let the scan run and check it in the morning. Time to sleep.

Day Four – May 24th

Well, that was short-lived. Turns out I didn’t have much time to sleep. Diaper duties. 

The gobuster command finished after 8 hours and nothing big turned up. I did, however, find a link to an HP login website. 
![Capture d'écran 2025-05-24 220311](https://github.com/user-attachments/assets/4db5fd81-e7e4-46f9-b472-2b2ccb155369)


Will try to brute force it later on. In the meantime, I got stuck again and did a deep dive into other .txt files to see what else I need to learn to look for. I looked into /readme and /license.txt. /readme had a hidden message:
![Capture d'écran 2025-05-24 225613](https://github.com/user-attachments/assets/eb795da7-d840-4bda-9e7a-024390240626)

 
The /license.txt file didn't yield much data until I checked the Inspector page. I found another string!
![Capture d'écran 2025-05-24 060400](https://github.com/user-attachments/assets/98df96e5-6005-4a0d-89bd-e259413edd2a)
Maybe this one is an actual hash? I decoded it and received elliot:ER28-xxxx. Odd-looking for a username or password. So, I tried all the combinations. 

Using the data found in the license.txt file, I was able to log in to the HP page under Elliot Alderson. It turns out he is an admin for WordPress. I was able to recover an old post from the user, and it seems Elliot might be the one who created the Mr. Robot content, or at least posted the video, about FSociety. I recovered all of the deleted posts, but it did not yield any new data. 

I then checked the media page and found a screenshot that reveals a username: alphxxxx and password: Dylan_xxxx. I inserted the credentials into the HP site and received a username error. At least I know the password is correct since it didn't give me a password error. I conducted more recon on the media page and found two new usernames to try: Dxxxx and mich0xxxx. Using mich0xxxx and Dylan_xxxx worked wonderfully. I successfully logged in as Krista Gordon.

On Krista's profile, in her bio, it hints at “another key”. So maybe I'm getting close. 

Day Five – May 27th

I got stuck again and did not know what to look for. I’m sure I found something, but I can’t make heads or tails of it, so I used the [Christine's Mr.Robot write-up](https://dev.to/christinec_dev/lets-hack-the-world-in-the-mr-robot-ctf-4bj5). Christine mentioned running a WPScan, so I followed suit. I discovered that it is a vulnerability scanner, specifically for WordPress. The interesting findings section revealed a good amount of data, so I followed up on each one. 

Most of the data pointed to the 2015 theme editor. I thought it might have something to do with the header that was mentioned in the Nmap scan. I clicked each available tab and only a few loaded. The others remained in an idle loading state. The exception was the editor tab, which revealed a full executable script. Maybe I can edit this? Or copy and paste it somewhere like the usernames from earlier?

I looked into 404 templates and saw numbers and letters all over the place. According to a YouTube video, there is something called a reverse shell that can be placed into each template. So, I tried [pentestmonkey’s reverse shell script](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php) that I found on GitHub. I was unsuccessful in all of my attempts. I tried templates for 404 and PHP. I have internet access, I can ping my other devices and machines, but the pages are not loading properly. I think something is misconfigured in my network settings. I conducted another Nmap on my network and noticed something I didn't see earlier. All of my ports are closed. I know for a fact I "looked" at it, but I thought it was normal. Something I will work on moving forward.

I will be taking a detour to figure this out. Though my master's program covered networking, it seems it didn't stick as I’d hoped. Time to go learn more basic networking. 

Day Six – June 9th

Yo! Hi, again. I found out that I need to implement port forwarding to resolve my issue. It is more complex than I thought, seeing as I know nothing about networking. I finally figured it out by watching countless videos, reading articles, asking Google and ChatGPT, and comparing all the notes to what makes sense in my mind. I tried everything I could think of, and it finally worked. I successfully configured my network to port forward from my host machine and receive the data on my guest machine. It was not a straightforward solution like the YouTube videos displayed. Creating rules and enabling them in the firewall did nothing. I aggressively attacked Google with question after question and took every little detail I found, wrote it out, pieced it together until I formed the final questions for Google. 

“What is Sudo iptables and Netsh Interface? How do I use them to open ports? Will it help me port forward?”

Thanks to a video by [EmbraceTheRed](https://www.youtube.com/watch?v=ZOGYnDW8_OY&ab_channel=EmbraceTheRed) on YouTube, I learned what commands to run and gained an understanding of what it does and how it works. Just a little. During this process, I learned more about how to configure Firewalls, reintroduced myself to the different functions of WAN and LAN, how to set and check firewall rules on the web interface and command line for Windows and Kali Linux, how to listen to, force open and close ports, port forwarding and redirection, specific ports and their functions, how to find server data in the Inspector tab, and a bunch of other commands that I saved in my notes. I heard recently that Pen Testers and Cyber professionals don't memorize commands, they are just great at using Google to find the solutions they are looking for. That lit a fire under me to start taking notes instead of memorizing every command I come across. I am still green, but I’m feeling a new hue developing. Now I think I can pick up where I left off.

Day Seven – June 11th

Two weeks left till I return to work. I really pray I can find all the flags by then. If not, well, that’s okay. As long as I complete the box, I will be pleased with my efforts and growth. As will my wife. 

