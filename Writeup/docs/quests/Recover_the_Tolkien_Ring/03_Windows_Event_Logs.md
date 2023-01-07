# Windows Event Logs

!!! summary "Windows Event Logs<br>*Difficulty*: :fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}"
    Investigate the Windows event log mystery in the terminal or offline.  Get hints for this challenge by typing hint in the upper panel of the Windows Event Logs terminal.

## Elf Introduction

??? quote "Talk to Dusty Giftwrap"
    Hi! I'm Dusty Giftwrap!<br>
    We think the Snowrog was attracted to the pungent smell from the baking lembanh.<br>
    I'm trying to discover which ingredient could be causing such a stench.<br>
    I think the answer may be in these suspicious logs.<br>
    I'm focusing on Windows Powershell logs. Do you have much experience there?<br>
    You can work on this offline or try it in this terminal.<br>
    Golly, I'd appreciate it if you could take a look.

## Hints and Resources

??? hint "Hints from Sparkle Redberry after completion of the <a href="../02_Wireshark_Practice/">Wireshark Practice</a> objective"
    **Eric Pursley's Talk**<br><a href="https://youtu.be/5NZeHYPMXAE">https://youtu.be/5NZeHYPMXAE</a>

??? hint "Hints from the terminal"
    **grep command**<br><a href="https://linuxcommand.org/lc3_man_pages/grep1.html">https://linuxcommand.org/lc3_man_pages/grep1.html</a>

??? hint "Other resources"
    **Eric Pursley, Log Analyzing off the Land | KringleCon 2022**<br>
    <a href="https://www.youtube.com/watch?v=5NZeHYPMXAE&list=PLjLd1hNA7YVy9Xd1pRtE_TKWdzsnkHcqQ&index=2&t=6s">https://www.youtube.com/watch?v=5NZeHYPMXAE&list=PLjLd1hNA7YVy9Xd1pRtE_TKWdzsnkHcqQ&index=2&t=6s</a>

## Solution

Open the terminal next to Dusty Giftwrap and answer the questions.

For this objective you can either download the event log from <a href="https://storage.googleapis.com/hhc22_player_assets/powershell.evtx">https://storage.googleapis.com/hhc22_player_assets/powershell.evtx</a> to a Windows computer and open it with the Windows Event Log application, or use grep with the text formatted version of the event log on the terminal.

For the purposes of this investigation, there are two Windows Event Log IDs that are relevant. 

Event 4104 lists commands that are being submitted for processing by PowerShell.  
Event 4103 displays the execution of those commands.  

While it's not strictly necessary to include event 4103, these can be helpful during our investigation by providing us the results of the commands executed (for example, the contents of a directory after the command 'ls' is used).

Setup the Windows Event Viewer by opening the log file and setting up a filter to include only event IDs 4103 and 4104.

It is also important to remember that when using the text formatted version the records are stored in reverse time order.

<br>

**Question 1: What month/day/year did the attack take place?  For example, 09/05/2021.**

For this question both Event Viewer and grep are equally effective.

=== "Event Viewer"
    As we are looking for log entries related to the Lembanh recipe, use the Find action to locate records containing the word 'recipe'.
=== "grep"
    Search the text file for lines containing the word 'recipe'.  Since lines are not individually time stamped include a few lines before the one with the matching value to get time stamp records.
    `grep -i recipe powershell.evtx.log -B 3 | more`

??? success "Answer"
    `12/24/2022`

<br>

**Question 2. An attacker got a secret from a file. What was the original file's name?**

For this question Event Viewer is far simpler than grep.

=== "Event Viewer"
    Presumably the secret ingredient is labeled as such, so use the Find action to locate records containing the word 'secret'.  An event that lists the contents of the recipe file is found at 12/24/2022 03:01:03.  Look a few records prior to that one for the remote command that displayed the file.
=== "grep" 
    Not advisable

??? success "Answer"
    `Recipe`

<br>

**Question 3. The contents of the previous file were retrieved, changed, and stored to a variable by the attacker. This was done multiple times. Submit the last full PowerShell line that performed only these actions.**

=== "Event Viewer"
    Use the Find action to locate records containing the word 'Recipe'.  Look through them until we find commands that assign a value to a variable, in this case, $foo.  Now that we have the variable name, use Find to locate records that assign this variable (use the string '$foo ='), and locate the last one.
=== "grep"
    Use the following command to find records including the word 'Recipe'<br>
    `grep Recipe powershell.evtx.log`<br>
    This will show that the varialbe being assigned is $foo.  Use the following command to show all records that assign a value to this variable.<br>
    `grep '^$foo =' powershell.evtx.log`<br>
    Remember that this file is in reverse time order.

??? success "Answer"
    `$foo = Get-Content .\Recipe| % {$_ -replace 'honey', 'fish oil'}`

<br>

**Question 4. After storing the altered file contents into the variable, the attacker used the variable to run a separate command that wrote the modified data to a file. This was done multiple times. Submit the last full PowerShell line that performed only this action.**

The most common way for PowerShell to write content to a file is with either the Out-File, Add-Content, or Set-Content commands, so search the log for instances where one of these commands are used with the variable.

=== "Event Viewer"
    Modify the Find action to search just for '$foo' (since we are looking for any use of this variable).  Find the last instance where this variable is used with one of the commands to write content to a file.
=== "grep"
    Use the following grep command to display all uses of the variable $foo, and look for the first record (remember that they are in reverse time order) in the output that uses a PowerShell command to write to a file.<br>
    `grep '$foo' powershell.evtx.log`<br>

??? success "Answer"
    `$foo | Add-Content -Path 'Recipe'`

<br>

**Question 5. The attacker ran the previous command against a file multiple times. What is the name of this file?**

=== "Event Viewer"
    Use the Find action to search for the following string and make note of each file name written to and the number of times it was.<br>
    `$foo | Add-Content -Path`
=== "grep"
    Use the following grep command and examine the output for the file written to the most times.<br>
    `grep '$foo | Add-Content -Path' powershell.evtx.log`

??? success "Answer"
    `Recipe.txt`

<br>

**Question 6. Were any files deleted? (Yes/No)**

=== "Event Viewer"
    Modify the filter being used to only show event ID 4104, then use the Find action to search for any instance of the following string.<br>
    `del`
=== "grep"
    Use the following grep command to see if there are any instances where the delete command is used
    `grep '^del' powershell.evtx.log`

??? success "Answer"
    `yes`

<br>

**Question 7. Was the original file (from question 2) deleted? (Yes/No)**

The result of the previous question will answer this one as well.

??? success "Answer"
    `no`

<br>

**Question 8. What is the Event ID of the log that shows the actual command line used to delete the file?**

=== "Event Viewer"
    If we've been using Event Viewer then we've been using this event ID throughout the objective.
=== "grep"
    Modify the grep command used to determine if a file was deleted to show the preceding line, one of which will have the event ID.<br>
    `grep '^del' -B 1 powershell.evtx.log`

??? success "Answer"
    `4104`

<br>

**Question 9. Is the secret ingredient compromised (Yes/No)?**

Refer to the answer to Question 3

??? success "Answer"
    `yes`

<br>

**Question 10. What is the secret ingredient?**

Refer to the answer to Question 3

??? success "Answer"
    `honey`

<br>

## Completion

??? quote "Talk to Dusty Giftwrap to receive hints for the next objective"
    Say, you did it! Thanks a million!<br>
    Now we can mix in the proper ingredients and stop attracting the Snowrog!<br>
    I'm all set now! Can you help Fitzy over there wield the exalted Suricata?<br>
    It can be a bit mystifying at first, but this Suricata Tome should help you fathom it.<br>
    I sure hope you can make it work!
