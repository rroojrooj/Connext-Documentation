# Introduction

## What are Routers?

### rroojrooj:
  Routers are one of the two types of liquidity providers in the Connext Network. They are called “active” LP providers, whereas Stableswaps AMMs are called “passive”. The router’s role is essential; to provide instant liquidity for the user in the destination chain. They are given a fee based on the transaction size. Routers then claim the spent fund from Nomad afterwards; to specify, they can claim after Nomad’s latency period ends ([30 minutes](https://medium.com/offchainlabs/fighting-censorship-attacks-on-smart-contracts-c026a7c0ff02)).
  
### Tung#6913:
With the concept of "Trustlessness", Connext implements Router to make no one rely on person or people. They do not need to trust anyone which might be the safe way to move the asset from one chain to another. 
Therefore, the main major reason is "Truslessness" and its strong security”. It is also undeniable that routers helps provide liquidity on destination chain, which can help to increase the efficiency of Connext.

- Now, Let’s talk about router after the Amarok upgrade, in which this change can be considered as one of the important step and make Routers even more important and a must than before to Connext.
With Amarok Upgrade and a close partnership with Nomad, Modular Interoperability has been introduced which help Connext move closer to the ideal outcome by modularizing the protocol stack 
(Ref :https://www.youtube.com/watch?v=pnw6x_v0iiY&t=80s). 

### VArt#9432:
  Routers in the Connext project are the key link after the user. They can be categorised as an active LP provider because the router is able to adapt to the current market realities. The main task of the router is to provide liquidity to the different networks so that Connext users can make cross-chain swaps quickly and cheaply. The only way for them to earn money is through commissions, which depend on the total amount of the transaction. But they can't get them right away, because thanks to the partnership with Nomad, users don't have to worry about things like shunning attacks(regular users won't be able to face a conspiracy between routers because of a 30 minute delay in getting commission rewards).

## Why do we need them in Connext

### rroojrooj:
1.  Routers are the backbone of the Connext Network. After the Amarok Network upgrade, how routers operate changed significantly. In the previous version, routers will lock up their funds on the destination chain and be rewarded with the user’s fund on the sender chain. This mechanism causes [problems](https://blog.connext.network/announcing-the-amarok-network-upgrade-5046317860a4) such as liquidity rebalancing and strict liveness. Here comes the Amarok upgrade; modular Interoperability can be achieved with the help of Nomad, a cross-chain communication protocol.
  
2.  The user initiates the transaction by calling the `xcall` function on the Connext contact, which will include: passing in funds, gas details, arbitrary data, and a target address  (including the chain info, and this can be a contact). The Connext contact will, if needed, swap the user’s fund to match the Nomad’s token type and call the Nomad contact to start the 30-60 minutes message latency across chains.
  
3.  Observing routers with funds on the user destination chain will simulate the transaction. If passed, they will use their funds to sign the transaction and send it to an auctioneer (Sequencer); this is a kind of bidding. The auctioneer collects the router’s bids in every X block and selects the correct router(s) to fill in the user’s transaction. 
  
4.  Then the auctioneer will group all bids and send them to a relayer network for submitting them on-chain. The contact then 1) checks the whether the fund is enough for the transaction, 2) if needed, swaps the Nomad’s token type to match the canonical asset of the chain, and 3)  lastly, sends the fund to the correct target (including executing `calldata` against the target contact). At this point, the user will receive their fund on the destination chain. For routers, this is not finished. After  30-60 minutes and the Nomad message arrives, the heavily batched transaction hashes will be checked for their corresponding router addresses. If matched, Nomad assets are minted and paid back to the routers.

### VArt#9432:
There are so many reasons why a router system is used.

 - The main reason is the fact that, thanks to the concept of routers, the user does not have to worry that their tokens might get lost. The router has no ability to think. Its main task is to receive tokens and redirect the requested token when possible. It is incapable of doing much of anything.
 - The next but not least important factor is the liquidity that Router provides. With assets in its account, users can redirect their tokens to any network that the bridge supports. This saves a large amount of time and also allows the user to stay in the DeFi zone.
 - It is also worth noting that with more than 100 routers, the project is not prone to a complete shutdown due to evenly distributed power.
  
### Tung#6913:
  
  - Now, Let’s talk about router after the Amarok upgrade, in which this change can be considered as one of the important step and make Routers even more important and a must than before to Connext.
With Amarok Upgrade and a close partnership with Nomad, Modular Interoperability has been introduced which help Connext move closer to the ideal outcome by modularizing the protocol stack 
(Ref :https://www.youtube.com/watch?v=pnw6x_v0iiY&t=80s). 

- How Amarok Upgrade helps Router to operate in a more efficient and effective way?
There are 2 changes that help to enhance the operation of Router which are:
--1). 1-of-N-Routing : The user’s transaction can be completed by any routers themselves, unlike before the Amarok Upgrade, routers need to available 24/7 during a transaction; otherwise, the user’s fund can be locked;
therefore, this can help to remove the possibility of user fun lockups as well as significantly reduce the liveness requirements for routers.
--2). Simpler Liquidity : Routers can receive liquidity in the destination chain of a transaction directly where they provide it. This can massively create and improve more capital efficiency and availability as well as 
eliminate the possibility of liquidity rebalancing and fragmentation. 
Therefore, with the Amarok Upgrade, it enables a much better flow and improve security as well.


## Security Assumptions and Risks

### rroojrooj:

### MAG#0106:
- **Seqencer downtime** (Connext's team will improve soon)
  
  The seqencer is responsible for collect the attempted transactions (bids) from all routers and choose which router fullfill the request So, if seqencer downtime the system will be down.
- **Funds delayed**

  The connext use Optimistic system that use fraud proofs to verify funds tranfer one chain to another so there is 30 minute window. During this time anyone watching the chain can prove fraud on the origin chain and disconnect to the destination chain. So the router has to wait for their funds until the disputing watcher be resovled.
- **Compromised router private key**

  If somebody can access to router's private key they can withdraw the liquidity to their wallet.

### Tung#6913:
- There are a lot of cases that they implement node validator(People who hold the liquidity and are the one who approve the transaction to move the liquidity to another chain)and Multi-sig instead of Routers, and one of the clearest case is Ronin Bridge
where the bridge got hacked because the seed phase of Node Validator leaked. In the case of Ronin Bridge, there are 5 validators out of 9 got hacked, so hackers transfer the money our of the bridge about $600 Millions. 
Another case is Harmony Bridge where they also use Multi-sig and got hack for about $100 Millions.
Therefore, using Routers instead of Node Validator is one of the most efficiency and security that users can trust.

### VArt#9432:
All kinds of situations and problems are possible in life. Let's look at some of the problems.

 - Problems with the explorer, which has some deficiencies with its speed of perception and processing of information (the team works on this)
 - There is a possibility of a power drop due to some percentage of the routers malfunctioning. Even in spite of this, the service will still work, albeit not very fast.
 - A malfunction of one of the networks supported by the bridge may result in difficulties in operating such a network (this is a factor independent of Connext that you should be aware of anyway).
 - Compromise by one or more routers. This is almost impossible to foresee, but it cannot stop the whole service from working.


## Business Model

### rroojrooj:
  The router’s primary business model is to provide liquidity in exchange for a trading fee. Currently, the trading fee is set at five basis points (0.05%) per transaction. `Trading fee = Volume * 0.0005` From the beginning of March to late April, the Connext Network made around $20,000,000 of volume per week; this gave the routers around $10000 in trading fees. If in the future, as Connext’s userbase increases, the volume will also increase, which will benefit the routers directly. There is no impermanent loss for providing bot passive and active LPs.

### VArt#9432:
  The main motivations that are close to the Connext team can be described as follows

- Developing the DeFi sector as well as ensuring security within it
- Attracting developers into cryptocurrency, by promoting their own product
- Implementing cross-chain swaps in order to interact with different networks in the shortest possible time.
