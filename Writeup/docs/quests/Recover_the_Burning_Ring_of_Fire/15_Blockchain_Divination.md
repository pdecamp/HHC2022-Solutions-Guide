# Blockchain Divination


!!! summary "Blockchain Divination<br>*Difficulty*: :fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: grey;"}"
    Use the Blockchain Explorer in the Burning Ring of Fire to investigate the contracts and transactions on the chain. At what address is the KringleCoin smart contract deployed? Find hints for this objective hidden throughout the tunnels.


### Spork Introduction

??? quote "Slicmer"
	Don't bug me, kid. Luigi needs me to keep an eye on these offers you can't refute.<br>
	The boss told me to watch them for any shifty transactions from wallets that aren't on the pre-sale list.<br>
	He said to use this Block Explo... Exploder... thing.<br>
	With this, I can see all the movement of the uh... non-fungusable tokens.<br>
	Once on the blockchain, it's there forever for the whole world to see.<br>
	So if I spot anything that don't look right, I can let Luigi know, and Palzari will get to the bottom of it.<br>
	She looks sweet, but she's actually the boss' enforcer. Have you talked to her yet? She even scares me!<br>
    It sure would be fun to watch you get on her bad side. Heh heh.


### Hints and Resources

??? hint "Hints from <a href="../../../extras/Hidden_Chests/">Hidden Chests</a>"
    **Cryptostage** - From the NPSL (Outside Tolkien Ring)<br>
    Look at the transaction information. There is a From: address and a To: address. The To: address lists the address of the KringleCoin smart contract.<br>
    <br>
    **A Solid Hint** - From the Hall of Talks<br>
    Find a transaction in the blockchain where someone sent or received KringleCoin! The Solidity Source File is listed as KringleCoin.sol. Tom's Talk might be helpful!

??? hint "Other resources"
    **Tom Liston, A Curmudgeon Looks at Cryptocurrencies | KringleCon 2022**<br>
    <a href="https://www.youtube.com/watch?v=r3zj9DPC8VY&list=PLjLd1hNA7YVy9Xd1pRtE_TKWdzsnkHcqQ&index=5&t=5s">https://www.youtube.com/watch?v=r3zj9DPC8VY&list=PLjLd1hNA7YVy9Xd1pRtE_TKWdzsnkHcqQ&index=5&t=5s</a>
    
### Solution

Open the Blockchain Explorer terminal next to Slicmer

The hardest part of this objective is understanding how transactions involving a blockchain smart contract work.  Once we know that for a smart contract to process a transaction the transaction needs to be sent *to* the contract, we just need to find any transaction in the blockchain where KringleCoin was transferred or earned.  

One such transaction will be recorded in the block created when you bought your hat.

Another place where you can find the address of the contract is in the block that created the contract and added it to the blockchain.  This is usually early in the blockchain's existence.

??? hint "Hint"
    *very* early. 
    
??? hint "Hint Hint"    
    Like say, block #1.

Lastly, since transactions involving KringleCoins are happening all the time, you can just browse the chain a bit and you are likely to come across one.

??? success "KringleCoin smart contract address"
    `0xc27A2D3DE339Ce353c0eFBa32e948a88F1C86554`
