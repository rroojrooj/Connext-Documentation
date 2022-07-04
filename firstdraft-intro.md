# Introduction

## What are Routers?

Routers are one of the two liquidity providers used in the Connext Network. The router’s role is essential; to provide instant liquidity for the user in the destination chain. Rouetrs are given a fee based on the transaction size in return. Routers then claim the spent fund plus the fee from Nomad afterwards; the latency period is around 30 minutes.

## Why do we need them in Connext?

Routers and Stableswap AMMs are the main TVL and the backbone of the Connext Network. The router’s role when users send funds across blockchain is to provide instant liquidity for the user in the destination chain. 

Then the user’s fund on the sender chain will be bridged using Nomad. Nomad is a cross-chain communication protocol that used fraud-proof ([1 of N watcher](https://blog.connext.network/optimistic-bridges-fb800dc7b0e0)) to relay data across the chain. It works by opening 30 minutes window for the “watcher” to prove fraud. If fraud is proved, the “updater” (data sender) bonded funds will be slashed and given to the disputed watcher. If not, the data passed to the destination chain will be considered finalized. This is when the router receives back its spent fund.

## How do they work?

1. The user initiates the transaction by calling the `xcall` function on the Connext contract, which will include: passing in funds, gas details, arbitrary data, and a target address  (including the chain info and this can be a contract). The Connext contract will, if needed, swap the user’s fund to match the Nomad’s token type and call the Nomad contract to start the 30-60 minutes message latency across chains.
2. Observing routers with funds on the user destination chain will simulate the transaction. If passed, they will use their funds to sign the transaction and send it to an auctioneer (Sequencer), a kind of bidding. The auctioneer collects the router’s bids in every X block and selects the correct router(s) to fill in the user’s transaction.
3. Then, the auctioneer will group all bids and send them to a relayer network for on-chain submission. The contract then 1) checks whether the fund is enough for the transaction, 2) if needed, swaps the Nomad’s token type to match the canonical asset of the chain, and 3)  lastly, sends the fund to the correct target (including executing `calldata` against the target contract). At this point, the user will receive their fund on the destination chain.
4. For routers, they will not received their fund yet. After  30-60 minutes and the Nomad message had arrived, the heavily batched transaction hashes will be checked for their corresponding router addresses. If matched, Nomad assets are minted and paid back to the routers.

## Security Assumptions and Risks

### Security Assumptions:

Optimistic bridges differ from externally verified bridges. After the data have been posted to the destination chain, there will be a 30 minutes window for any watcher to prove the validity of the transaction. If any watcher can prove fraud on the origin chain, the original funds will be slashed and transferred to the disputed watcher instead.

Optimistic bridges use a single honest verifier assumption. This means that it requires only 1 of N verifiers in the system to verify and update cross-chain data correctly. 
In other words, the cost to attack an optimistic bridge with N verifiers equals the cost to corrupt or hack N verifiers. As a result, assuming the underlying chains are secure, no amount of money can guarantee that a hack or an attack will be successful. 

### Risks:

1. Brute force attack on the router's Admin Token. (The Admin Token is used by routers to authenticate requests made to the Router's REST API endpoint.)
2. If the Docker Router image and VM has a direct connection to the Internet, the router's API endpoint is susceptible to being exposed externally.
3. The sequencer collects bids from the routers and choose a router to fulfill the transaction request. The system will be down if sequencer downtime occurs.
4. With fraud-proof, funds can be delayed if any watcher can prove a fraudulent transaction within the 30 minutes window. So, the router will have to wait for the funds until the disputed watcher is resolved.
5. Router's private key is leaked and liquidity withdrawn from router's wallet.

Refer to [security.md](https://github.com/connext/documentation/blob/main/docs/routers/security.md) and [router community call](https://www.youtube.com/watch?v=rjNcdm1mjCQ) for best practices to mitigate these risks.

According to [rekt.news](https://rekt.news/leaderboard/), the top three of the leaderboard are all related to bridge exploit. All of the exploits would not have been possible if they had used an optimistic bridge, even if all of the keys were compromised.

## Business Model

The router’s primary business model is to provide liquidity in exchange for a trading fee. Currently, the trading fee is set at five basis points (0.05%) per transaction.

Formula: `Trading fee = Volume * 0.0005` 

From the beginning of March to late April, the Connext Network made around $20,000,000 of volume per week; this gave the routers around $10,000 in trading fees. If in the future, as Connext’s userbase increases, the volume will also increase, which will benefit the routers directly. There is no impermanent loss for providing active LPs.

## To understand more about Connext, please read:

1. [Solving the Liquidity Problem (Medium)](https://blog.connext.network/solving-the-liquidity-problem-88bde201501)
2. [The Interoperability Trilemma (Medium)](https://blog.connext.network/the-interoperability-trilemma-657c2cf69f17)
3. [Connext has partnered with Nomad (Medium)](https://blog.connext.network/connext-has-partnered-with-nomad-e20cd8e62e31)
4. [Optimistic Bridges (Medium)](https://blog.connext.network/optimistic-bridges-fb800dc7b0e0)
5. [Announcing the Amarok Network Upgrade (Medium)](https://blog.connext.network/announcing-the-amarok-network-upgrade-5046317860a4)
6. [A summary of how the Connext Network work. (Twitter Thread)](https://mobile.twitter.com/ConnextNetwork/status/1530611831785541632)
7. [Modular Interoperability - Layne Haber (Youtube)](https://www.youtube.com/watch?v=pnw6x_v0iiY)
8. [Trustless Two-Way Bridges With Side Chains By Halting (ETHResearch)](https://ethresear.ch/t/trustless-two-way-bridges-with-side-chains-by-halting/5728)
