# Introduction

## What are Routers?

Routers are one of the two types of liquidity providers used in the Connext Network. The router’s role is essential; to provide instant liquidity for the user in the destination chain. They are given a fee based on the transaction size in return. Routers then claim the spent fund plus fee from Nomad afterwards; the latency period is around 30 minutes.

## Why do we need them in Connext?

Routers and Stableswap AMMs are the main TVL and the backbone of the Connext Network. The router's roles when users send funds across blockchainis is to provide instant liquidity for the user in the destination chain. 

Then the user's fund on the sender chain will be bridged using Nomad. Nomad is a crosschain communication protcol that used fraud proof ([1 of N watcher](https://blog.connext.network/optimistic-bridges-fb800dc7b0e0)) to relay data across chain. It work by opening a 30 minutes period for "watcher" to prove fraud. If fraud is proved, the "updater" (data sender) bonded funds will be slashed and given to the disputing watcher. If not, the data passed to the destination chain will be considered finalised. This is when the router receive back their spent fund.

## How do they work?

1. The user initiates the transaction by calling the `xcall` function on the Connext contact, which will include: passing in funds, gas details, arbitrary data, and a target address  (including the chain info, and this can be a contact). The Connext contact will, if needed, swap the user’s fund to match the Nomad’s token type and call the Nomad contact to start the 30-60 minutes message latency across chains.
2. Observing routers with funds on the user destination chain will simulate the transaction. If passed, they will use their funds to sign the transaction and send it to an auctioneer (Sequencer); this is a kind of bidding. The auctioneer collects the router’s bids in every X block and selects the correct router(s) to fill in the user’s transaction.
3. Then the auctioneer will group all bids and send them to a relayer network for submitting them on-chain. The contract then 1) checks the whether the fund is enough for the transaction, 2) if needed, swaps the Nomad’s token type to match the canonical asset of the chain, and 3)  lastly, sends the fund to the correct target (including executing `calldata` against the target contact). At this point, the user will receive their fund on the destination chain.
4. For routers, this is not finished. After  30-60 minutes and the Nomad message arrives, the heavily batched transaction hashes will be checked for their corresponding router addresses. If matched, Nomad assets are minted and paid back to the routers.

## Security Assumptions and Risks

Optimistic bridges differs from externally verified bridges. After the data is posted to a the destination, 30 minutes fraud proof window is started; during this period, if any watcher can prove fraud on the origin chain, the original funds will be transferred to the disputing watcher instead.

Optimistic bridges use a single honest verifier assumption. This means that it requires 1 of N verifiers in the system to correctly verify and update cross-chain data. 
In other words, the cost to attack an optimistic bridge is with N verifiers is equal to the cost to corrupt or hack N verifiers. As a result, assuming the underlying chains are secure, no amount of money can guarantee that a hack or an attack will be sucessful. 

## Business Model

## To understand more about Connext, please read:

