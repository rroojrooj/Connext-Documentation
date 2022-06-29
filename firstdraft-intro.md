# Introduction

## What are Routers?

Routers are one of the two types of liquidity providers used in the Connext Network. The router’s role is essential; to provide instant liquidity for the user in the destination chain. They are given a fee based on the transaction size in return. Routers then claim the spent fund plus fee from Nomad afterwards; the latency period is around 30 minutes.

## Why do we need them in Connext?

Routers and Stableswap AMMs are the main TVL and the backbone of the Connext Network. The router's roles when users send funds across blockchainis is to provide instant liquidity for the user in the destination chain. 

Then the user's fund on the sender chain will be bridged using Nomad. Nomad is a crosschain communication protcol that used fraud proof ([1 of N watcher](https://blog.connext.network/optimistic-bridges-fb800dc7b0e0)) to relay data across chain. It work by opening a 30 minutes period for "watcher" to prove fraud. If fraud is proved, the "updater" (data sender) bonded funds will be slashed and given to the disputing watcher. If not, the data passed to the desrtination chain will be considered finalised. This is when the router receive back their spent fund.

## How do they work?

## Security Assumptions and Risks

## Business Model