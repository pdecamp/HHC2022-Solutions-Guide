# Suricata Regatta

!!! summary "Suricata Regatta<br>*Difficulty*: :fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}"
    Help detect this kind of malicous activity in the future by writing some Suricata rules.  Work with Dusty Giftwrap in the Tolkien Ring to get some hints.


## Elf Introduction

??? quote "Talk to Fitzy Shortstack"
    Hm?.. Hello...<br>
    Sorry, I don't mean to be uncharaceristically short with you.<br>
    There's just this abominable Snowrog here, and I'm trying to comprehend Suricata to stop it from getting into the kitchen.<br>
    I believe that if I can phrase these Suricata incantations correctly, they'll create a spell that will generate warnings.<br>
    And hopefully those warnings will scare off the Snowrog!<br>
    Only... I'm quite baffled. Maybe you can give it a go?<br>


## Hints and Resources

??? hint "Hints from Dusty Giftwrap after completion of the <a href="../03_Windows_Event_Logs/">Windows Event Logs</a> objective"
    **The Tome of Suricata Rules**<br>
    <a href="https://suricata.readthedocs.io/en/suricata-6.0.0/rules/intro.html">This</a> is the official source for Suricata rule creation!

## Solution

Open the terminal next to Fitzy Shortstack.

For this challenge you must add 4 rules to the suricata.rules file meeting the criteria specified.  After each rule has been added, run `./rule_checker` to confirm your answer.

All rules will use `alert` as their action.  Since every rule needs a unique SID value we will number the rules 1-4, though these values could be anything.

<br>

**Rule 1: First, please create a Suricata rule to catch DNS lookups for adv.epostoday.uk.<br>Whenever there's a match, the alert message (msg) should read "Known bad DNS lookup, possible Dridex infection".**

This rule will apply to packets using the DNS protocol that originate from any source and go to any destination.  A destination port is not strictly necessary but as all DNS traffic will go to port 53 including the destination port makes the rule more specific.  DNS keywords will be added (see <a href="https://suricata.readthedocs.io/en/suricata-6.0.0/rules/dns-keywords.html">https://suricata.readthedocs.io/en/suricata-6.0.0/rules/dns-keywords.html</a>) to apply the rule to DNS lookups for the specific domain in question.  

??? success "Answer"
    ```
    alert dns any any -> any 53 (msg:"Known bad DNS lookup, possible Dridex infection"; dns.query; content:"adv.epostoday.uk"; sid:1;)
    ```

<br>

**Rule 2: Develop a Suricata rule that alerts whenever the infected IP address 192.185.57.242 communicates with internal systems over HTTP.<br>When there's a match, the message (msg) should read "Investigate suspicious connections, possible Dridex infection"**

This rule will apply to packets using the HTTP protocol going to or coming from from the infected IP address. 

??? success "Answer"
    ```
    alert http any any <> [192.185.57.242/32] 80 (msg:"Investigate suspicious connections, possible Dridex infection"; sid:2;)
    ```

<br>

**Develop a Suricata rule to match and alert on an SSL certificate for heardbellith.Icanwepeh.nagoya.<br>When your rule matches, the message (msg) should read "Investigate bad certificates, possible Dridex infection"**

This rule will apply to packets using the TLS protocol with any source / destination combination.  TLS keywords will be added (see <a href="https://suricata.readthedocs.io/en/suricata-6.0.0/rules/tls-keywords.html">https://suricata.readthedocs.io/en/suricata-6.0.0/rules/tls-keywords.html</a>) to apply the rule to certificates matching the specified subject.

??? success "Answer"
    ```
    alert tls any any -> any any (msg:"Investigate bad certificates, possible Dridex infection"; tls.cert_subject; content:"CN=heardbellith.Icanwepeh.nagoya"; sid:3;)
    ```

<br>

**Let's watch for one line from the JavaScript: let byteCharacters = atob<br>Oh, and that string might be GZip compressed - I hope that's OK!<br>Just in case they try this again, please alert on that HTTP data with message Suspicious JavaScript function, possible Dridex infection**

This rule will apply to packets using the HTTP protocol with any source / destination combination.  Additional HTTP keywords will be added (see <a href="https://suricata.readthedocs.io/en/suricata-6.0.0/rules/http-keywords.html">https://suricata.readthedocs.io/en/suricata-6.0.0/rules/http-keywords.html</a>) to apply the rule to packets that include the specified content. 

??? success "Answer"
    ```
    alert http any any -> any any (msg:"Suspicious JavaScript function, possible Dridex infection"; file_data; content:"let byteCharacters = atob"; sid:4;)
    ```

## Completion

??? quote "Talk to Fitzy Shortstack"
    Woo hoo - you wielded Suricata magnificently! Thank you!<br>
    Now to shout the final warning of power to the Snowrog...<br>
    YOU...SHALL NOT...PASS!!!
