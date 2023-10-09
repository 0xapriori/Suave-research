# Awesome SUAVE
This repository is for research notes and reference materials related to SUAVE. I was maintaining a HackMD but i think this maybe a more flexible substrate. The repository features notes on the Spec proposd by Xyn, discussion with Christopher Goes, notes from the Bell Curve Podcast with Robert Miller, Jon Charb & Hasu, notes from talks by Robert Miller, Andrew Miller, and Daniel Marzec. 

## Definitions 
* **Abstract** - SUAVE is a permissionless credible commitment device (PCCD) that program and privately settle higher-order commitments for other PCCDs. It should look like a faster blocktime version of Ethereum but with SGX to ensure private mempool
* **Concrete** - SUAVE as *market for mechanisms* designed to decentralize the MEV supply chain by enabling centralized infrastructure (builders, relays, centralized RFQ routing, etc.) to be programmed as smart contracts on a decentralized blockchain.
* **Endgame** - SUAVE is designed to be the Mempool and Blockbuilder for all blockchains

*Note that Abstract and Concrete is not Flashbots teminology.*

## Diagrams 

*Note these are my own interpretations. Quote is attributable to Robert Miller at Flashbots* 

### Market for Mechanisms

![image](https://github.com/0xapriori/Suave-research/assets/116141134/d97fc69d-ba29-43ee-9b58-d523da92e6d9)


## Abstract  
* [Xyn Spec](https://hackmd.io/@sxysun/suavespec)
* [The Future of MEV is SUAVE](https://writings.flashbots.net/the-future-of-mev-is-suave)
* [Devcon Talk](https://youtu.be/ACXAzLy3iWY)
* [Apriori slide deck](https://github.com/0xapriori/Suave-research/blob/main/mmFood.pdf)
* [Suave, Anoma, Shared Sequencers, and Super Builders](https://dba.mirror.xyz/NTg5FSq1o_YiL_KJrKBOsOkyeiNUPobvZUrLBGceagg)

## Concrete 
* [Suave-Geth repo](https://github.com/flashbots/suave-geth)
* [Suave.Salon EthCC](https://drive.google.com/file/d/14KD40UV5yQwesnCEkDxd24Bb9tZMrHse/view?pli=1)
    * [Code Demo](https://drive.google.com/file/d/1IHuLtxwjRvRpYjMG3oRuAgS5MUZtmAXq/view)
* [SUAVE smart contract programming model: TEE-based smart contracts for block building - Andrew Miller](https://www.youtube.com/watch?v=DhsDFKnHPa0) 
    * [Slides](https://t.co/xW4KthwFk8)
* [The MEVM, SUAVE Centauri, and Beyond](https://writings.flashbots.net/mevm-suave-centauri-and-beyond)
* [Quintus Q&A](https://twitter.com/0xQuintus/status/1679590263067492353?s=20)
* [Modular Summit - Robert Miller](https://www.youtube.com/watch?v=WYH7n4M016A)
* [MEV in Modular World - Bell curve podcast](https://blockworks.co/podcast/bellcurve/8abf40ae-de03-11ed-bafe-f7f4165ccafd)
* [MEV-Share: programmably private orderflow to share MEV with users](https://collective.flashbots.net/t/mev-share-programmably-private-orderflow-to-share-mev-with-users/1264) 
* [Announcing MEV-share beta](https://collective.flashbots.net/t/announcing-mev-share-beta/1650) 
* [Block Building Inside SGX](https://writings.flashbots.net/block-building-inside-sgx)
* [Convo on FB Forum Hasu, Elijah, Nikete, apriori](https://collective.flashbots.net/t/concerns-with-suaves-effect-on-ethereum-security-and-total-value-extracted/1035/10
)

## Literature
* [CredibleCommitments.wtf](Crediblecommitments.wtf)
* [MEV & Credible Commitments](https://docs.google.com/presentation/d/1BhPNVYzIVkpiQ9dKUqBhPYY7z04tchmlcfhwf5FaWvw/edit#slide=id.g187ff1cf8c1_0_459)
* [Ethereum is game-changing technology, literally](https://medium.com/@virgilgr/ethereum-is-game-changing-technology-literally-d67e01a01cf8)
* [Game theory on the blockchain: a model for games with smart contracts](https://arxiv.org/pdf/2107.04393.pdf)
* [Disarmament Games](https://www.cs.cmu.edu/~conitzer/disarmamentAAAI17.pdf)
* [Computing the Optimal Strategy to Commit to](https://www.cs.cmu.edu/~sandholm/Computing%20commitment%20strategy.ec06.pdf)
* [Reasoning about Knowledge](https://www.cs.rice.edu/~vardi/papers/book.pdf)
* [Program Equllibrium](https://sci-hub.st/https://doi.org/10.1016/j.geb.2004.02.002)
* [Introduction to Multi-Agent Systems](https://sci-hub.st/10.1007/978-3-642-14435-6_1)
* [On Imperfect Recall in Multi-Agent Influence Diagrams](https://arxiv.org/pdf/2307.05059.pdf)
* [TEE/SGX Wiki](https://collective.flashbots.net/t/tee-sgx-wiki/2019) 


# Notes on Spec

Suave is a market place for MEV solutions. Similar how users send their transactions to flashbots rpc endpoint to access the flashbots builder and MEV-Share devices, a user would instead have more choices. The advantage to this is it allows for decentralized MEV solutions as applications on SUAVE. In this way Suave acts as a global mempool and blockbuilder for Ethereum, rollups, and in the future all blockchains.   

For context there exist many competing MEV markets/devices today. All of these devices require users to make trust assumptions about the operators. These can include;   
* Order Flow auctions 
* Block space markets 
* Private mempools  
* Cowswap
* Penumbra
* Solver APIs   

With SUAVE these devices can be instantiated as smart contracts within Suave Chain's version of the EVM. Flashbots calls this the MEVM which is the EVM with custom precompiles for various MEV related actions. (need specifics here).  


## Summary
  
* SUAVE maintains a global state, which consists of accounts. There are two types of accounts: Externally Owned Accounts (EOAs) and Contract Accounts. EOAs are controlled by private keys, while Contract Accounts are controlled by the code they contain. The state is stored in a Merkle Patricia Trie.
* Transactions are the means to transfer value and data between accounts. They contain fields like nonce, gas price, gas limit, recipient, value, data, and signature. Transactions are signed by the sender and submitted to the network.
* SUAVE transactions are private. SUAVE transactions are meant to act as higher-order commitments to other domains such as Ethereum.
* Preference-gas is a measure of computational resources in SUAVE. Every transaction has an associated preference-gas cost. Users pay for preference-gas with Ether when executing transactions or smart contracts. SUAVE validators receive the fees as a reward for validating and including transactions in SUAVE blocks.
* SUAVE’s blockchain is an ordered sequence of blocks. Blocks contain a header and a list of transactions. The block header contains fields like parent hash, beneficiary, state root, transactions root, receipts root, logs bloom, difficulty, number, gas limit, gas used, timestamp, extra data, mix hash, and nonce.
* SUAVE uses some consensus algorithm to agree on the state of the network.
* SUAVE smart contracts are self-executing contracts with the terms of the agreement directly written into code. SUAVE uses the Ethereum Virtual Machine (EVM), a Turing-complete virtual machine.
* Smart contracts on SUAVE should be higher-order commitment constructors (MEV-time applications) for domains like Ethereum. The semantics of those higher-order commitments should also be either smart contracts on SUAVE or higher-order commitments themselves.
* The SUAVE state is updated after each transaction or contract execution. The state consists of account balances, nonces, and contract storage. State transitions follow specific rules, which determine how accounts and storage are updated.
* SUAVE state include settled higher-order commitments for other domains, i.e., the resolved propositional commitments (e.g., a transaction on Ethereum).
* SUAVE relies on a peer-to-peer network for communication and synchronization. Nodes use protocols like devp2p or libp2p to discover and connect to peers, share transactions, and synchronize blockchain data.
* SUAVE nodes expose a JSON-RPC API for interacting with the network. This API allows querying the blockchain, sending transactions, managing accounts, and subscribing to events.



# Older Notes & Comments on Spec

**1.** Overall, the strategic uncertainty MEV of SUAVE originates from people's uncoordinated use of SUAVE's commitment constructors. And those commitments are either about higher order commitments of MEV games on other domains, or commitments of games on top of SUAVE.

**2.** We would expect most of non-SUAVE MEV comes from strategic uncertainty, which SUAVE solves almost completely (per the reasoning above, folk theorem of higher order commitments and the competition between mechanism designers to build better commitment constructors/focal points), while the MEV coming from fundamental uncertainty doesn’t decrease much from lower latency of PCCDs.

**3.** In reality, fundamental uncertainties are not purely random walks, and previledged actors with information/capital advantage can still extract huge MEV. For example, insider information like knowledge of price impacting trades or whitehat hacks are innately discrete events that can never be hedged. More generally, for any kind of fundamental uncertainty that have discrete impacts, the commitment game will devolve into latency races or centralization to private SUAVE relays. The quantification of the relative percentage of this kind of MEV is still an open problem.

**4.** Higher-order commitments eliminates strategic/information uncertainty for a given fundamental uncertainty level, and it mitigates the Stackelberg leader (latency race) effect greatly if the level of fundamental uncertainty can be reduced. In fact, with lower expressivity in the commitment constructors, the strategic uncertainties just become fundamental uncertainties due to the inability to predicate/determine on the strategy space or the game structure.

**Example of higher order commitment** 

``` rust
(* allowing higher-order commitments to condition on real-time binance price *)
Inductive Commitment := 
| mkCommit: Commitment -> Oracles -> Commitment.

(* oracles provide additional information for *)
Inductive Oracles :=
| binancePrice: rational -> Oracles
| NYSEPrice: rational -> Oracles.
```
# Comments
**2.** The literature provides helpful context, but SUAVE doesn't solve strategic uncertainty, neither does Anoma - the most a discrete system can do is reduce strategic uncertainty to an oracle problem. For example, in mutually-assured destruction, with credible commitments (suppose that they were binding to actually controlling nuke launches) you can commit to a strategy, but there's still uncertainty in whether the commitments actually reflect the reality (i.e. whether you don't have some secret nuke which isn't tracked in the system). Participants would still have strategic uncertainty in correspondence to how sure they are that the stated correspondence between the discrete digital representations, commitment constructors, etc. actually represents the real-world actions which they care about. Maybe this counts as "fundamental uncertainty" but it seems to me like a distinct category from what the spec discusses under that term.

**3.** I'm not sure where this "asset price is random walk" theory comes from, I don't see any reason to expect that to be true. Future expected trajectories of asset prices are basically a world modelling problem - e.g. if there is an asset which reflects the price of storing a ton of carbon, w.r.t. gold (say) I would expect this to go down in the next few decades as the technology improves (and probably most people would), it's certainly not random. Probably there are some interesting directions in modelling fundamental uncertainty as a causal structure inference problem or something like this.

**4.** Agree that higher-order commitments are necessary, though I think it's better to avoid the recursive type constructor definitions (as the talk mentioned). I think coordinating to avoid latency races is a very important topic which the SUAVE design provides a thoroughly unsatisfying answer to, they've just gone another step forward in the latency race, someone can just make a "SUAVE 2" with fewer validators and even lower latency. This is part of why I think the "slow game" problem & consensus between users is important.



# Notes on Andrew Miller presentation 

Secret network is a bit like Suave. It only has one mempool and its  a private mempool because all of the transactions in secret network are encrypted and the only way secret network transactions get into a block is by tendermint validators putting them in order but you can't do anything with the ciphertext until the block is already finalized and then you only get to execute them in the next vblock. 

Secret network is already like suave in that they have  uniswap v2 clone which automatically has forntrunning resistance becasue of that built in blind ordering that is applied to every transaction sent through Secret Network. Sienna swap eliminates the nearest possibility of front running, and they have replay prevention that is sort of accurate

## Before

That aloe does not solve the whole problem you want to solve because if you just have the fair ordering then you have messy arbitrage which comes later; e.g., unsophisticated trades create  mess someone must clean up. Messy arbitrage opportunities is the result of shitty user trades where the user trades on one pool but most of the liquidity is fragmented across other pools so the user gets a terrible price. 

## After

You only see the result of the trade after the block is finalized in time for the beginning of the process of the next block. Next A PGA happens rhen because whoever can get a transaction at the top of block gets to take all of the arbitrage opportunity restoring the balance of the pools and the searcher gets to pocket that arbitrage differnce. 

## SUAVE

How this could be done in a more trust-minimized way is MEV-share does today as in secret network in that your transaction is private until it gets settled. With mev-share backrunners get to bid on placing their tx after your's in the same block but when bidding they have to share the larger percentage of the arbitrage they take back with the original user. At a technical level it is implemented as extract and redistribute. Prefer to think of it as your getting a competitive auction to arbitragers for a commission for them to do the backrunning for you automatically upgrading your shitty transaction into a more sophisticated arbitrage free transaction that results. 

## What is Suave as compared to what exists today? 

1. Replace trust in the operator with trust in TEEs, This could be combined and/or replaced cryptography and threshold MPC
2. Make the operation of the service decentralized, geographically and administratively - many different nodes not controlled by Flashbots, lower trust environments
3. User programmable, based on smart contracts. An open, contestable, marketplace for mechanisms - if you don't like the way Mev-share distributes the rules you can just fork the mev-share smart contract and replace it with a different rule. No one can stop you from uploading a different sc to suave. If you can convince users to send their txs to that one instead no one can stop them from picking your contract if they want. Thats what it means to be contestable. This is what makes it an open marketplace because flashbots doesn't have to come up with the perfect auction structure, smart contract developers can do so. 

A starting point of what this could look like is replicating whats in the ecosystem today. Make MEV-share auction, Mev-share matchmaker a backrun smart contract. Make the flashbots builder a blockbuilding contract, etc. 

With Suave Market for mechanisms based on Smart Contracts, we have transparency, user empowerment. Competing contracts can fulfill user orders. Contracts require a privacy oriented programming model. 

Suave's strategy was anticipated by StrategyProof Computing from the NG, Parks, Seltzer 03. It sounds like a modern description of Suave. It didn't forsee smart conracts and it isn't aimed at the preventing private negotiation aspect its jsut concerned about the efficiency of maarket mechanisms. But it says all of the impossibility result of perfect auctions that there is not going to be a perfect mechanism so what this advocates for is you should have an open system where anyone can propose innovative new market design components which are compositional and can interact with each other in that way market designers can innovate on parts of this and you can have an ecosystem of gradually improving market mechanisms. SCs is the only tool for open composition and innovation we have seen so that is kind of a natural fit. They only focus on performance and efficiiency of the market and didn't go into the doomer thesis; If you don't have this open contestable marketplace where the market designs are public smart contracts you can look at analyze and fork. Then the market dominator may become entrwenched through private negotiated bulk deals at which point it becomes impossible to dislodge them. 

* Suave aims to fulfill this vision of Strategy proof computing, in having an open marketplace for mechanisms.... even just for these blockspace/orderflow auctions
* Emphasis on "openness"...Suave ensures market innovation occurs in the open, through published smart contracts... rather than in private arenas with deep moats. 

> The SPC infrastructure must provide suport for multiple users to design and deploy competing LSP mechanisms, in amarket for mechanisms. With this, we achieve the second set of design principles of open and decentralized systems. Our belief is that an open marketplace will naturally lead to mechanisms with the "right scope" and the "right complexity." This decision rpresents a tradeoff b/w providing a large enough scope to suggiciently simplify the game-theoretic decision facing a participant - for example, bringing resources that are complementary for a large number of users into the same scope- while maintaing a small enough scope to build computationlly reasonable resource allocation mechanisms. The degree of which market forces lead to the emergence of mechanisms with the right scope is an important research question. 

## Suave as an Ideal Functionality 

* In cryptography we use ideal functionalities to make a specification. If you skip the TEEs and decentralization and you did a trusted smart contract with a central third party it would look like this. For cryptographers this is understood as a security definition that describes the interface.
* The interface is that there is a service which has smart contracts and private data storage on it. Just like secret network. The different kind of interfaces are market design innovators can make new contracts and push them onto the chain. Users can interact with the smart contract and there is still room for searchers/arbitragers who get explicit hints like mev-share hints or anything they can infer from side channels that are captured in the specification and they can invoke their own sc txs and input arbitrage inputs and so on. The imporitant thing is that this outputs an L1 block to Ethereum. 

## Suave Architecture

There are Scs on suave. There is a suave chain. There is a mempool of pending suave bids as well as bids that are finalized being in a chain. Being committed in suave chain doesn't mean the bid is satisfied yet it means its sequenced and has DA and you cannot ignore it on the suave chain. Not all of the execution happens on suave chain that suggests linear bottleneck and latency bottlenecking. The point is we will have off-suave chain execution. Rollup of a rollup or Sidechain of a sidechain. The execution is off-chain of suave chain but still carried out in trusted hardware enclaves by executors we call TEE kettles. So they can have access through their kettles to the confidential data but only according to the rules of the smart contracts as defined. Each smart contract defines the program rules for interacting with that contract's private data

There is some mempool of pending Suave bids. Suave blocks probably come much faster than L1 blocks, but not infinately fast. The thing that will make this realistically performant is being able to do concurrent execution by lots of different enclave nodes multiple cores on each enclave.  Each one may be doing some simulation. Weather you can do simulation or not is up to the contract rules to define. In secret network it used to be that they allowed open simulation. So you can simulate transactions even before they are finalized in a block. So you can simulate a front-run, the victim, and a backrun. If it succeeded you can increase the front run and try again. This way you can do the MEV on secret network. Now you can only execute them after the block is finalized. It would be useful if smart contrcat authors could approve some functions to be done with local simulation. It just gives more flexibility to the Suave app designers.

-- 

# Notes from Bell Curve Podcast on Modular MEV 

## Fast Block times 

>JC: As far as expressing preferences arbitrage I see and I want certain transactions to close this arb on another chain. If going through this process requires me to express a preference on Suave chain to commounicate this is what I want done. This implies you are bounded by the Suave chain blocktimes. If another chain I want to express a preference for has a realy fast blocktime and maybe that opportunity is going to disappear that means SUave chain would need a fast block time to express that preference in the first place. Does that kind of pressure just lead to having low blocktimes, incentivize Suave chain to have super low blocktimes and have the race to centralization there? 

>RM: I think it is a little different on Suave chain because we are really only posting data about other chains and settling payments. So I think in some ways it is less important than other domains. You raise a good point, I do think that Suave's blocktimes will have to be faster and should be faster than other domains. On the other hand I thoink there are ways to craft your preferences up front that don't require you to communicate them in real-time. As an example, you could offer to an executor on a domain that's really fast, hey if you spam my contract and it emits a log that says success I'm willing to pay you some amount. That gives an incentive up front for an executor to be spaming your contract at the precise moment it needs to be without you needing to communicate that at the moment it needs to, it will just spam your contract apriori. I think you can probably do similar things with latency in that way. There are upfront ways to communicate your preferences to prime executors working on lower blocktimes.  

>Hasu: I would also point out that you can think of Suave almost as a board where basically anyone can submit their transaction execution requests. Once you make such a request and its floating in the Suave mempool then the request is basically out there. And anyone who goes and executes it they can then come and claim their payment later on after the transaction was executed. They need to execute it then an Oracle reports a state change from the domain then the payment can be unlocked from the target chain. SO that means the settlement is not bound to the blocktime of the target chain. The settlement can happen anytime later. Thats why in my opinion Suave Blocktimes do not need to be lower than any participating chains because its okay for executors to claim their payment with a little bit of delay because they know the payment is trustless. 

>JC: The settlement after the fact wasn't the part I was getting at. If it requires a tx on Suave chain to communicate that preference in the first place, if that blocktime is longer than the domain I want to express that preference for I won't be able to express that preference in time. 

>Hasu: I believe you can communicate all your preferences just through pre-signed transactions they do not actually need to be mined in Suave chain in order ot be commitments. 

>RM: That does work. We do have a notion of a special transaction in Suave that will carry your signed commitments if you want to throw it in the Suave mempool which is what you want if you want the maximum number of executors to access your transaction, if you want it to be censorship resistant. If you want to skip all of that and really save on latency you could communicate it directly with executors jsut with this signature model. I do think your touching on something which you can't really get around because of the laws of physics that there will be some cross-domain MEV which is not possible to extract because of latency. There is some kind of pressure like there is today  from latency in cross-domain extraction on domains that are centralized and have faster blocktimes too. I think thats inherent to cross-domain MEV, I don't know that we can do anything about that. This is a reason for us as a community to align on real decentralization. 

## Why do you need a Suave chain? 

- Need for a chain to transmit preferences efficiently: low cost and DoS resistant
- Imposing a fee during network congestion by including preferences on-chain deters attackers
- Suave's design allows low-cost expression of preferences without congestion, requiring the chain to introduce a new transaction type
- Owning the full stack allows for faster iteration on design parameters and tailored optimizations
- Suave may need a faster block time than Ethereum L1, achievable on the Suave chain
- Considered making Suave chain a rollup to access L1 data trustlessly, avoiding the need for oracles with trust assumptions
- Replacing GETH's existing mempool with one optimized for faster communication enhances Suave's domain specificity for MEV
- Neutrality: As Suave expands beyond Ethereum, it should be a neutral layer with ownership and participation crafted from different domains
- Designing for political neutrality can foster adoption, as existing Ethereum ownership might not appeal to other platforms like Cosmos or Solana

>JC: Why do you need a chain in certain cases to express these preferences and settle them after the fact as opposed to having this more p2p layer part of Suave where I can communicate my intent and if you execute it on the other chain you get paid on the other chain. Why do we need to go through this process of communicating that through Suave and having this Oracle problem to go back and settle that payment after the fact. Why can't we have this more global p2p layer and settle on those chains themselves where I'll give you a payment if you get my thing done? 

>RM: Its a good question. The tl;dr is why do you need a chain? There are a few reasons. In order to be economically efficient we need some way of transmitting preferences. We think that is both as low cost as possible and Dos resistant. One way to be DoS reisstant is to force attackers to pay a cost if there is spam within the network. With a blockchain we can impose a fee in periods of network congestion by including preferences on chain and this would deter attackers. We have the designed the mechanism within Suave which we think allows as low cost as possible extraction of preferences when there are not periods of congestion and that requires you to chain itself add a new type of transaction. I don't think this is something we can get through ACD today. This allows us to introduce new mechanisms that we think are more economically efficient to express preferences while still having the property of being DoS resistant. A standalone p2p network thats global wouldn't have the same mechanism. At least I havn't seen any design of one so far. 

>The second more general thing other than this one specific mechanism for achieving low cost but Dos resistant expression of prefrences is the notion of owning the full stack. By being able to change things on the full stack we can iterate more fast on many different design parameters and make tailored optimizations which would be difficult to retrofit on an existing domain and which you may not want on an existing domain. It probably makes sense for Suave to have a faster block time than Ethereum L1 which is a non-starter on Ethereum L1, but we can offer that on Suave chain. Another example is, We thought of making Suave chain a rollup that is using the derivation function of the rollup in order to get trustless access to L1 data and rollup data and to not need Oracles with trust assumptions or at least use trust assumptions of L1. These are examples of optimizations. 

>One final one I'll throw out is replacing the existing mempool within GETH with a different mempool thats optimized for faster communication. These types of optimizations make Suave a better domain and specific for MEV. 

>Hasu: The last thing to point out is nuetrality. As Suave moves beyond Ethereum I think there is the question is it fair that all of these preferences get settled on an actual settlement layer or should this be its own standalone nuetral layer that has ownership and participation and so on thats crafted in an entirely bottom up way from these different domains. Part of getting adoption for a system like this is designing for political nuetrality. And maybe the existing ownership of Ethereum may not be optimized in such a way that it gets buy in from Cosmos, Solana, and other centralized exchanges. 

--

# Robert Miller on Suave

## SUAVE architecture with the MEVM

SUAVE - 3 main components 
- MEMVM chain - modified EVM chian with special precompiles for MEV use cases
    - some of these precompiles call out to an execution Node. 
- Execution Node - this is an off-chain node providing 
    - execution, 
    - similating transactions, 
    - merging them together, 
    - building a block, 
    - inserting new transactions.
        - Its doing this to provide credible and private compute off-chain that you don't want to do on-chain.
        - These run in trusted execution environments so that you can still trust the outputs and the results of this compute even though its not all being performed on-chain
- Confidential data store - for private information you don't want to store on-chain which can be accessed by the execution node. 

![](https://hackmd.io/_uploads/HkU_uoRJa.png)


### 4 main stake holders

- developers who are writing their MEV infrastructure as smart contracts
- users who send private data that they want included on chain and authorizing some contracts saying they accept this auction or block-building contract for their transaction to have access to their private data.
- Executors backrunning, arbitraging, doing MEV things to execute people's bids
- The net result of SUAVE is creating blocks that are included on chain for Ethereum or rollups

### Execution Nodes

* Smart contracts define off-chain execution that is performed inside execution nodes
* Execution nodes have access to private data
* Execution nodes run insdie of trusted execution environments to provide privacy and credibility to compute

![](https://hackmd.io/_uploads/r1S5uj0ya.png)


**Commentary**

You have developers who are defining in smart contracts off-chain execution which is performed by the execution node. Your smart contract says take all of these transactions, simulate them and treat them according to this algorithm. That's not actually performed on-chain. Instead all of that is performed in an Execution Node in a TEE like an SGX where you know the code is runnning with some level of privacy and integrity. This is a really scalable way to get private and confidential compute for MEV use cases. The other thing to note is that Execution Nodes if a user permissions a certain smart contract, have access to private data. 

### Example MEVM Contract

``` go

function EGPBuild (Bundle [] allBundles) external {
// EGP = effective gas price
// Pass #: Estimate EGP of each bundle based on top-of-block sim
SimulatedBundle [] simmedBundles = new SimulatedBundle [] (allBundles. length);
State l1State = getState("EthereumMainnet", "latest") // Get latest mainnet state
for (uint 1 = 0; 1 < allBundles. length; 1++) {
    SimulatedBundle simmedBundle = simulateBundle(L1State, allBundles [i]);
    simmedBundles [i] = simmedBundle;
    
}
    
// Sort the bundles by EGP such that the highest EGP is first
sortBundles (simmedBundles, "effectiveGasPrice");
    
    
Block block;
block.state = l1State;
for (uint 1 = 0; 1 < allBundles. length; i++) {
    Sim simmedBundle = simulatepplyingBundle(block, allBundles [i]);
    
    if (simmedBundle.reverts as false && simmedBundle <= 0.99*simmedBundles[il){
block.txs.push (simmedBundles.txs);
    }
        
}
exportBlock (block);
    
}

```

**Commentary**

This is what an MEVM contract looks like. Take for example the Flashbots builder today - FB blockbuilding algorithm written with MEVM. You can see how it has a couple functions you normally don't have within Ethereum
- We are getting Ethereum main-net state on the 5th line, the latest block state
- We are simultating bundles of transactions getting results
- We have a pending block there and we are simulating adding new transactions to it, seeing if they fail and discarding them
- We are exporting the block (2nd to last line) making it available for Ethereum proposers to include it on chain if they request it from SUAVE

These are the super powers the MEVM gives you that you don't have available on Ethereum. This is jsut one example.  

## Other potential MEVM contracts
- New building algorithms
- Pre-confirmations
- UniswapX
- CowSwap
- MEV-Share
- Distirbuted building
- Composing the above
- ...etc

**Commentary**

Any off-chain MEVM infrastructure we have the ambition of being able to support within the MEVM. 


## Suave as a Market
- Suave is a decentralized platfrom for MEV applications
- Suave will drastically lower the barrier to experimenting in the block building market and open access to orderflow in particular
- MEV applications compete and compose together in an open market for innovation, resulting in better outcomes for users and betetr blocks for validators

Having low barriers for people to deploy their own MEV infrastructure in this environment. Have these different applciations compose together into something that is larger. This is not a monolithic flashbots builder that we are decentralizing. Instead this is a platform where anyone can deply their own applications and all of these can compete and compsoe togetherin this open market. We thing ultimately this positivie sum platform vision will result in better blocks for validators. 

## Other domains and SUAVE

This isn't supposed to be made just for Ethereum but instead a platform to be building blocks for many chains. You can have execution nodes running, EVM, WASM, or any VM you want. The integration is relatively straight forward so long as your chain has some way to listen to Suave for blocks. 

We landed our first block on Goerli test net a couple of days ago. 

![](https://hackmd.io/_uploads/BkuA99AJ6.png)

## Roadmap

Suave Centauri

My thoughts: MEV-Share and SUAVE-GETH devnet, landing blocks on Goerli already

* Privacy-aware orderflow auction to return to users the MEV that their transactions creater. In this auction, searchers competw for the right to back run a user, thereby bidding up the value returned to them. Initially the auction assums trust in Flashbots but is private for users and searchers
* SUAVE Chain devnet for stress testing and community experimentation

**Commentary**

Moving towards the Centauri release which is a devet launching in Q4 where anyone that wants to can deploy their own MEV infrastructure as smart contracts on SUave. We will share examples of what it looks like to take all of our centralized infrastructure and create them as smaert contracts on SUave using the MEVM targetting Q4 of this year

SUAVE Andromeda 

My thoughts: Sweet spot for Suave to start picking up significant volume and begin experimenting with cross-domain MEV for Ethereum and rollups. TEE Execution nodes remove trust in Flashbots 

* SUAVE Chain mainnet will allow users to express preferences and send them to the Execution Market
* SGX-based orderflow auction to remove trust in Flashbots and make the auction efficient for searcehrs
* SGX-based centralized block building to enable open but private orderflow for centralzied builders

Commentary:

- Initially execution nodes won't be in SGX or TEEs so they will require some trust in Flashbots
- The next release Andromeda we will put those nodes inside an SGX and that sysetm will not have any trust in FlashbotsAt that point we will look to move onto other Domains too. If you are a rollup or L1 developer reach out to discuss integration.


# DMarzec Talk at SBC HH

## Overview

You can submit a transaction. With MEV-Share we will release a subset of the information that searchers can then search on and send a transaction back in which will backrun it. MEV-Blocker does something similar except they just reveal the full information and then they, in order to stop probabilistic sandwiching introduce fake transactions. 

With SUAVE what you can actually do is encode that logic for your orderflow auction as smart contract. You can then deploy it onto SUAVE. The key here is that the user will then encrypt their transaction to the SUAVE network and then they will send it into a SUAVE node. Now anyone can run the auction, so you have a verified program that they are running on top of confidential user bids. 

The other cool thing that SC enabled OFAs give you is that you can actually control the privacy model so if you want to leak some of the information as in MEV-share, you can choose to do that - the user specifically allows certain contracts that they send their bids to. 

If I write some contract that will leak all of your information and give you nothing, and as the user you are free to click on that and use it but we think that you can also submit to some really shitty AMM today. People don't do this because their are various network of trust type of scenarios. 

We also envision people/searchers/builders, etc. running alot of these nodes and are incentivized to try and get as much order flow as they can. 

Another cool thing this solves in the builder market is alot of the iteration on block building mechanisms is most heavily constrained based on the order flow you have. If you only have 50% of the orderflow and you want to compete on bundle merging with jaredfromsubway. You can't really meaningfully iterate their if you don't have jared's order flow. This stops a lot of builders from being able to iterate. So we hope that in the base case SUAVE will lower the barriers of entry to the building market because you can get access to the entire order flow of everyone submitting through SUAVE.

## What is SUAVE compared to what exists today? 

Replace trust in operators with TEEs, maybe eventually threshold MPC some type of crypto. I think if we have the exact same block building infrastructure we have today but its on TEEs, I think we are in a radically better place. This isn't the 5-10 years timeline of what this market will actually look like, but we think its way better than trusting some random server that says they won't leak your data or something like that. 

We have very experienced SGX engineers who know how to mitigate exploits and things of that nature. If there is an exploit, and say three blocks are unbundled, that's really bad yes, but typically that's not as bad as Ethereum going down. Also many people finding these exploits are sophisticated labs doing this for academic cloud and not some random 16 year old in their bedroom. 

Make the operation of the service decentralized, geographically and administratively. Ideally one person should not be able to shut down the system. Geographic Decentralization  is a longer term 

User programmable based on smart contracts - a marketplace for mechanisms. Any degen AMM designer or smart contract dev should be able to read a tutorial deploy their own OFA on-top of Uniswap and you could meaningfully compete with orderflow. You don't need to be associated with Uniswap or anything. 

## Off-chain backend

EVM, state DBs, and some configs and contexts. You have this runtime which winds up at the interpreter and this is what translates your byte code. Interpreter is what translates your byte code and runs the execution. 

The MEVM is a new runtime, the SUAVE off-chain backend, and it plugs right into the interpreter so now at execution time your code has access to these fields. You get a confidential back-end that your execution gets access too as well as this off-chain eth backend so you can call functionality tht is used in traditional block building which is simulate a bundle or build a block. You get access to all of those inside your smart contract which is really awesome. 

So you can deploy a program that will take in a transaction and simulate it, do something with it based on the results of the simulation and maybe route it to another smart contract and you can kind of stack these things. You can imagine multiple layers of OFA contracts, Multiple Layers of block building contracts, multiple layers of relay contracts, and they all can sort of plug and play. The point is not to be prescriptive on what the right route through the supply chain is. Its about anyone being able to propose new ways for these mechanisms to interact and operate and then just let the market win for which one is the best.

## MEV supply chain

What are the differences in UX in terms of using SUAVE? Suave does not make any types of opinionated claims about the intent framework portion. That is for anyone to go out and design an intent framework to operate here. We have a new tx type called an off-chain tx type, it basically is like a regular Ethereum tx but the call data is for a function call on SUAVE and there is an additional field called confidential input which is your encrypted bundle. But other than that almost exactly the same one or two field differences. As well you also specify which contracts are allowed to execute over your tx. 

The RPC is also slightly modified. This send raw tx just takes this new off-chain tx type. How the RPC and the block builder communicate is different now. They communicate through the confidential data store. So your OFA can output a combination of the user's bundle with whatever transaction the OFA might have inserted like in MEV-share. Its like a refund transaction. The way to communicate that is passing it through SUAVE internally. Block building and the way it communicates with the relay is through tis confidential data store as well. Typically these have been http, you just do a post request. The portion of relay to proposer will probably stay the same for a while. There is no relay or proposer native to SUAVE so this will always be the last mile of the supply chain. We hope that there will be some cool p2p network standard that will come out so we can just broadcast these commitments to proposers and they can accept, but that's probably a long way to go. 

## What's possible?

1. Solidity based
	- OFAs
	- Builders
	- DEX aggregators
	- RFQs
	- Validity Conditions
	- Redistribution
	- "Intents"
	- Proposer Builder Commitments

	- DEX aggregators - You can do all of this same logic except on SUAVE where you know the computation is guaranteed to be what you allowed as well as knowing your data is private and up to the amount you specified. Same exact experience maybe a bit slower in the beginning. In the limit these things will be very comparable latency wise. 

	- Validity conditions - specify arbitrary conditions that your transaction is allowed to be included on chain. One example is a refund. I am only going to allow my transaction to be included onchain if I am sent half of the MEV extracted back. You can encode this in a smart contract and it won't even give your bundle to a builder unless that condition is satisfied 

	- Intents - if you create one of these languages to express intents then you can easily create an auction on SUAVE for it as well. 

	- Proposer-Builder commitments - you can imagine much more advanced commitments then what we have now 

2. Private Voting - not a use case we are focused on but you have privacy and cheap computation so you could in theory

3. EIP 4844 blob Merging - right now the way 4844 is structured you pay an upfront amount even if you don't use the entire blobgas. Imagine rollups that only need 1/3 of the blob gas in a specific transaction. Because of the properties of the underlying cryptography you can actually combine these transactions into one and it will satisfy everyone's original intent. You can design something on SUAVE which will merge these. I don't know if you need privacy for that case but you would get the decentralized aspect of running that service.  

## SUAVE subnets 
 
 One thing we are playing around with is weather a smart contract can specify the DA guarantees that it provides. 
 - You could have something like where a subset of nodes run the unix auction perhaps. 
	 - That OFA however its designed could maybe specify 200 ms rounds so those nodes have to come to agreement on which transactions are available to all of them and that could be used as inputs to the auction. 
	 
That is one idea but this is certainly an open question that gets into the idea that if the main suave chain is this global bulletin board, one application doesn't care if another application's data is available. If you are the block builder it does but if you are CoWSwap it doesn't matter what uniswap is doing so we are playing around with that. 
 
  

