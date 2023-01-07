# Wireshark Practice

!!! summary "Wireshark Practice<br>*Difficulty*: :fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}"
    Use the Wireshark Phishing terminal in the Tolkien Ring to solve the mysterires around the suspicious PCAP.  Get hints for this challenge by typing hint in the upper panel of the terminal.

## Elf Introduction

??? quote "Talk to Sparkle Redberry"
    Hey there! I’m Sparkle Redberry. We have a bit of an incident here.<br>
    We were baking lembanh in preparation for the holidays.<br>
    It started to smell a little funky, and then suddenly, a Snowrog crashed through the wall!<br>
    We're trying to investigate what caused this, so we can make it go away.<br>
    Have you used Wireshark to look at packet capture (PCAP) files before?<br>
    I've got a <a href="https://storage.googleapis.com/hhc22_player_assets/suspicious.pcap">PCAP</a> you might find interesting.<br>
    Once you've had a chance to look at it, please open this terminal and answer the questions in the top pane.<br>
    Thanks for helping us get to the bottom of this!

## Hints and Resources

??? hint "Hints from the terminal"
    **Wireshark Filters**<br><a href="https://wiki.wireshark.org/DisplayFilters">https://wiki.wireshark.org/DisplayFilters</a><br>
    <br>
    **tshark Filters**<br><a href="https://cheatography.com/mbwalker/cheat-sheets/tshark-wireshark-command-line/">https://cheatography.com/mbwalker/cheat-sheets/tshark-wireshark-command-line/</a><br>
    <br>
    **Misc SANS Cheatsheets**<br><a href="https://www.sans.org/blog/the-ultimate-list-of-sans-cheat-sheets/">https://www.sans.org/blog/the-ultimate-list-of-sans-cheat-sheets/</a>

??? hint "Other resources"
    **SSL Country Codes**<br><a href="https://www.ssl.com/country-codes/">https://www.ssl.com/country-codes/</a><br>
    <br>
    **Wireshark**<br><a href="https://www.wireshark.org/#download">https://www.wireshark.org/#download</a>

## Solution

Open the terminal next to Sparkle Redberry and answer the questions.

This challenge can be completed either by downloading the provided PCAP file to your local computer and opening it with Wireshark, or by using tshark on the in-game terminal.  That said, my tshark skills are very weak, so I used Wireshark exclusively for this objective.

<br>

**Question 1. There are objects in the PCAP file that can be exported by Wireshark and/or Tshark. What type of objects can be exported from this PCAP?**

Wireshark can export several types of objects using the File > Export Objects menu.  To complete this objective simply pick each of the object types available and identify the one that has objects available to export.

??? success "Answer"
    `http`

<br>

**Question 2. What is the file name of the largest file we can export?**

From the File > Export Objects > HTTP window, look for the filename with the largest size

??? success "Answer"
    `app.php`

<br>

**Question 3. What packet number starts that app.php file?**

From the same window used in the previous question, find the starting packet of the largest file

??? success "Answer"
    `687`

<br>

**Question 4. What is the IP of the Apache server?**

From the same window used in the previous two questions we see that the name of the server the objects came from is adv.apostoday.uk.<br>
Conveniently enough, the first two packets in the capture file is a DNS request for adv.epostoday.uk and the reply.<br>
Had this not been so obvious, we could have used the following filter to show just what we were interested in<br>
`dns.qry.name == adv.epostoday.uk`

??? success "Answer"
    `192.185.57.242`

<br>

**Question 5. What file is saved to the infected host?**

From the File > Export Objects > HTTP screen, export the app.php file and open it locally.  At the very end of the file is a function named saveAs that specifies the name of the file that will be created when the file executes.

??? success "Answer"
    `Ref_Sept24-2020.zip`

<br>

**Question 6. Attackers used bad TLS certificates in this traffic. Which countries were they registered to?<br>Submit the names of the countries in alphabetical order separated by a commas (Ex: Norway, South Korea).**


To limit our view of the capture to just TLS certificates use the following filter<br>
`tls.handshake.certificate`<br>
This results in 20 packets to inspect.<br>
For each packet, inspect the packet details and expand TLS > TLS Record Layer > Handshake Protocol: Certificate > Certificates > Certificate.  Most of the certificates in the capture refer to Microsoft, but several more suspect common names are also present.<br>
    
heardbellith.Icanwepeh.nagoya - Packet 808<br>
psprponounst.aquarelle - Packets 3903, 3922, 4747, 5158<br>
    
Expanding  those records further we find that the issuer countryName value for the first one is IL, and SS for the second.
Referring to the list of SSL country codes at <a href="https://www.ssl.com/country-codes/">https://www.ssl.com/country-codes/</a> we find the names of these countries.

??? success "Answer"
    `Israel, South Sudan`

<br>

**Question 7. Is the host infected (Yes/No)?**

From the exportable objects we see that the file app.php was downloaded twice, once as a 754 byte file, and the second time as a 808 kB file.  Exporting both and inspecting them shows that the second one has appended code that will create the file Ref_Sep24-2020.zip.<br>
Further inspection of this zip file reveals that it contains a single .scr file, which when analyzed by Windows Defender or Virus Total indicates that it is the Dridex banking trojan.

??? success "Answer"
    `yes`

<br>

## Completion

??? quote "Talk to Sparkle Redberry to recieve hints for the next objective"
    You got it - wonderful!<br>
    So hey, when you're looking at the next terminal, remember you have multiple filetypes and tools you can utilize.<br>
    Conveniently for us, we can use programs already installed on every Windows computer.<br>
    So if you brought your own Windows machine, you can save the files to it and use whatever method is your favorite.<br>
    Oh yeah! If you wanna learn more, or get stuck, I hear <a href="https://youtu.be/5NZeHYPMXAE">Eric Pursley's talk</a> is about this very topic.
