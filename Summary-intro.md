# Introduction

## What are Routers?

Routers are one of two types of liquidity providers used in the Context network. The role of the router is very important, as it should provide instant liquidity for the user in the chain. In return, they receive a reward in the form of commissions based on the size of the transaction. After that, the routers receive the funds spent from Nomad together with the commission, but only after a certain period of time, since the delay is about 30 minutes.

## Why do we need them in Connext?

Why are they needed in Connext?

Routers and Stableswap AMMs are the backbone of TVL and the foundation of the Connext Network. The role of the router is to provide instant liquidity for the user in the destination chain, when users send funds over the blockchain.

The user funds in the sender chain will then be bridged using Nomad, which is an inter-chain communication protocol that uses fraud proof ([1 in N number of observers](https://blog.connext.network/optimistic-bridges-fb800dc7b0e0)) to transfer data between chains. It works by creating a 30-minute period for the "observer" to prove fraud. If fraud is proven, the "updater" (the sender of the data) debits the bonded funds and passes them on to the observer. If not, the data transmitted to the destination chain will be considered complete. In this case, the router gets its spent fund back.

### Algorithm of action

1. The user initiates the transaction by calling the `xcall` function on the Connext contract, which includes specific data: transfer of funds, gas details, arbitrary data and target address (including chaining information, and this can be a contract). The Connext contract will, if necessary, swap user funds according to the Nomad token type and invoke the Nomad contract to trigger a 30-60 minute message delay between chains.

2. Observing routers with funds on the user's destination chain will simulate the transaction. If the transaction passes, they use their funds to sign the transaction and send it to the auctioneer (Sequencer). The auctioneer collects router bids in each X-block and selects one or more routers to execute the user transaction.

3. The auctioneer then groups all bids and sends them to the relay network for confirmation in the chain. A succession of contract actions then takes place. First, it starts by checking if there are enough funds for the transaction (swaping the Nomad token type for the corresponding canonical asset in the chain if necessary), and then the funds are sent to the pre-specified recipient (including performing `calldata` against the target contract). At this stage, the user will receive their fund on the destination chain.

4. At this stage, for the router, the transaction is not yet considered completed. After 30-60 minutes and the Nomad message arrives, the heavily batched transaction hashes will be checked for their respective router addresses. If matched, Nomad assets are minted and paid back to the routers.

## Security Assumptions and Risks

 - Problems with the sequencer, which has some deficiencies with its speed of perception and processing of information (the team works on this)

 - There is a possibility of a power drop due to some percentage of the routers malfunctioning. Even in spite of this, the service will still work, albeit not very fast.

 - A malfunction of one of the networks supported by the bridge may result in difficulties in operating such a network (this is a factor independent of Connext that you should be aware of anyway).

 - Compromise by one or more routers ([Ronin Bridge](https://www.youtube.com/watch?v=MrBTzNvx14c) and [Harmony Bridge](https://twitter.com/RugDocIO/status/1540151956076802048?s=20&t=7I9WcEWl-4FImI8xwqVHXQ) are prime examples). This is almost impossible to foresee, but it cannot stop the whole service from working.

## Business Model

The main business model of the router is to provide liquidity in exchange for a trading commission. The trading fee is currently set at five basis points (0.05%) per transaction. The formula for the calculation is as follows: `Trading fee = Volume * 0.0005` 
From the beginning of March to the end of April, Connext Network processed around $20,000,000 of funds per week. Accordingly, an amount of $10,000 has been made available to routers. If trading volume also increases in the future as Connext's user base grows, there will be a direct benefit for routers. Also worth considering, 
that there is no non-permanent loss at all in granting the bot passive and active LPs.
