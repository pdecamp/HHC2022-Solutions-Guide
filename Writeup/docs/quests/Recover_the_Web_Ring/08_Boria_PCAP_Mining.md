# Boria PCAP Mining

This segment of the Web Ring quest consists of 4 separate objectives tracked in your badge.

??? hint "Other resources"
    **Wireshark**<br>
    <a href="https://www.wireshark.org/#download">https://www.wireshark.org/#download</a>
    
## Naughty IP


!!! summary "Naughty IP<br>*Difficulty*: :fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}"
    Use the artifacts from Alabaster Snowball to analyze this attack on the Boria mines. Most of the traffic to this site is nice, but one IP address is being naughty! Which is it? Visit Sparkle Redberry in the Tolkien Ring for hints.

### Elf Introduction

??? quote "Alabaster Snowball"
    Hey there! I'm Alabaster Snowball<br>
	And I have to say, I'm a bit distressed.<br>
	I was working with the dwarves and their Boria mines, and I found some disturbing activity!<br>
	Looking through <a href="https://storage.googleapis.com/hhc22_player_assets/boriaArtifacts.zip">these artifacts</a>, I think something naughty's going on.<br>
	Can you please take a look and answer a few questions for me?<br>
	First, we need to know where the attacker is coming from.<br>
    If you haven't looked at Wireshark's Statistics menu, this might be a good time!

### Hints and Resources

??? hint "Hint after speaking with Alabaster"
    **Wireshark Top Talkers**<br>
    The victim web server is 10.12.42.16. Which host is the next <a href="https://protocoholic.com/2018/05/24/wireshark-how-to-identify-top-talkers-in-network/">top talker</a>?

??? hint "Other resources"
    **Wireshark**<br>
    <a href="https://www.wireshark.org/#download">https://www.wireshark.org/#download</a>

### Solution

Download the artifacts zip file that you get from either Alabaster or your badge objective.  Contained in it are two files, `victim.pcap` and `weberror.log`.

Alabaster hints that we should look at the statistics menu in Wireshark to determine where the attacker is coming from.  

Open the pcap file in Wireshark and go to Statistics > Conversations.  Select the IPv4 tab and sort the list by the number of packets sent.  From this we can see that one address in particular has sent an abnormal number of packets to our web server.

??? success "Answer"
    `18.222.86.32`

### Completion

??? quote "Alabaster Snowball"
    Aha, you found the naughty actor! Next, please look into the account brute force attack.<br>
    You can focus on requests to /login.html

## Credential Mining

!!! summary "Credential Mining<br>*Difficulty*: :fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}"
    The first attack is a brute force login. What's the first username tried?

### Hints and Resources

??? hint "Hint from Alabaster after completion of Naughty IP"
    **Wireshark String Searching**<br>
    The site's login function is at `/login.html`. Maybe start by <a href="https://www.wireshark.org/docs/wsug_html_chunked/ChWorkFindPacketSection.html">searching</a> for a string.

### Solution

For this challenge we need to identify the first username attempted in the brute force attack.  As Alabaster mentions, the username would be submitted to the /login.html page.

The hint from Alabaster suggests that we use the Find Packet feature of Wireshark to search for packets containing a reference to the login page.  However, this requires us to sift through a large number of packets looking for the right one, which might be easy to miss.

A better solution is to recognize that any login attempts will likely involve a POST request to the login.html page.  The following Wireshark filter will limit our view to just post requests originating from the naughty IP we identified earlier.<br>
`ip.addr == 18.222.86.32 and http.request.method == POST`

Inspecting the HTML form items in the first packet's details gives us the answer.

??? success "Answer"
    `alice`

### Completion

??? quote "Alabaster Snowball"
	Alice? I totally expected Eve! Well how about forced browsing? What's the first URL path they found that way?<br>
    The misses will have HTTP status code 404 and, in this case, the successful guesses return 200.

## 404 FTW

!!! summary "404 FTW<br>*Difficulty*: :fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}"
    The next attack is forced browsing where the naughty one is guessing URLs. What's the first successful URL path in this attack?

### Hints and Resources

??? hint "Hint from Alabaster after completing Credential Mining"
    **HTTP Status Codes**<br>
    With forced browsing, there will be many 404 status codes returned from the web server. Look for 200 codes in that group of 404s. This one can be completed with the PCAP or the log file.

### Solution

Identifying the first URL found by a brute force attack requires us to first find a large number of GET requests resulting in HTTP status code 404, and then following those until we identify one that returns status code 200.  This can be performed either in Wireshark or by using the supplied weberror.log file.

=== "Wireshark Method"
    For this objective we are only interested in HTTP status codes, which we can create a filter for.  Since we want to find the first 200 code that will be accompanied by a large number of 404 codes we need to include both of them, and as before, we will  limit our investigation to the naughty IP with the following Wireshark filter.<br>
    `ip.addr == 18.222.86.32 and (http.response.code == 404 or http.response.code == 200)`

    After applying this filter we can scroll through the resulting list looking for the flurry of 404 statuses.  Then it's just a matter of looking through that list to find the first instance of code 200, then inspecting the request URI in the packet details.
=== "weberror.log File Method"
    This log file contains all requests to the web server along with their resulting status codes.  This includes successful requests as well, so the 'error' in the filename is a bit misleading.

    We can extract from this file all the requests sent by the naughty IP using either grep on a Linux machine or Powershell on Windows.
    ``` 
    $ grep 18.222.86.32 weberror.log > attacker.log
    ```
    ``` 
    PS> cat weberror.log | select-string '18.222.86.32' > attacker.log
    ```

    After this, open the attacker.log file in a a text editor and search for the first instance of the string ` 404 ` (note the spaces around the status code).  After finding it, search now for the next instance of ` 200 `, to find the first successful request.


??? success "Answer"
    /proc


### Completion

??? quote "Alabaster Snowball"
    Great! Just one more challenge! It looks like they made the server pull credentials from IMDS. What URL was forced?<br>
    AWS uses a specific IP address for IMDS lookups. Searching for that in the PCAP should get you there quickly.

## IMDS, XXE, and Other Abbreviations

!!! summary "IMDS, XXE, and Other Abbreviations<br>*Difficulty*: :fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}"
    The last step in this attack was to use XXE to get secret keys from the IMDS service. What URL did the attacker force the server to fetch?
    
### Hints and Resources

??? hint "Hint from Alabster after completing 404 FTW"
    **Instance Metadata Service**<br>
    AWS uses a specific IP address to access <a href="https://www.sans.org/blog/cloud-instance-metadata-services-imds-/">IMDS</a>, and that IP only appears twice in this PCAP.

### Solution

For this challenge we need to find the IDMS URL that was accessed.  IDMS always uses the IP address 169.254.169.254, so we can use the Find Packet feature in Wireshark to find any packets containing that address in the packet details.

We still want to limit our investigation to the naughty IP, otherwise we may catch legitimate use of IMDS, so setup the display filter as we have before.
`ip.addr == 18.222.86.32'

Then, open the Find Packet toolbar and search for the string `169.254.169.254`.  Make sure that the search is setup to search on Packet details.

Use of this IP will be in a POST request to a vulnerable URL.  For each one found (the hint for this challenge says that this IP appears two times, but it actually appears four times) look at the HTTP status 200 packet following it.  The one that returned data to the attacker will have interesting things in the packet data.

??? success "Answer"
    `http://169.254.169.254/latest/meta-data/identity-credentials/ec2/security-credentials/ec2-instance`


### Completion

??? quote "Alabaster Snowball"
    Fantastic! It seems simpler now that I've seen it once. Thanks for showing me!<br>
	Hey, so maybe I can help you out a bit with the door to the mines.<br>
	First, it'd be great to bring an Elvish keyboard, but if you can't find one, I'm sure other input will do.<br>
	Instead, take a minute to read the HTML/JavaScript source and consider how the locks are processed.<br>
	Next, take a look at the Content-Security-Policy header. That drives how certain content is handled.<br>
    Lastly, remember that input sanitization might happen on either the client or server ends!
