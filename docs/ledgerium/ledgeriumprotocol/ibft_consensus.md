# Consensus
While Quorum supports Raft-based, Clique POA and IBFT Consensus algorithm, Ledgerium Blockchain adopted Quorum's IBFT (Istanbul Byzantine Fault Tolerance) to build it blockchain. 

**IBFT (Istanbul Byzantine Fault Tolerant)** is a consensus mechanism which is an alternative to Proof of Work in an Ethereum network. Like other algorithms, IBFT ensures a single, agreed-upon ordering for transactions in the blockchain, and provides added benefits for enterprises, including settlement finality. IBFT was first implemented in Geth by Amis Technologies, and later adopted in Quorum. [1]

The PoW (Proof of Work) algorithm is famously costly, in both hardware and electricity. This cost is intentional, to prevent anyone from easily taking over the network, and so while PoW has been pioneer for building full decentralization where anyone (including attackers) can participate, the alternate algorithm has to be evaluated.

The PoS (Proof of Stake) algorithm scores over PoW in terms of hardware and electricity consumption. The creator of the next block is chosen via various combinations of random selection and wealth or age (i.e., the stake), however it plagues with one issue i.e. the "nothing-at-stake" problem, wherein block generators have nothing to lose by voting for multiple blockchain histories, thereby preventing consensus from being achieved. 

The PoA (Proof of Authority) emerges as a possible best solution, utilizing a system whereby nodes in the network are allocated the privilege of producing new blocks for the chain using a round-robin or other arbitrary system. IBFT is one of the many flavours of PoA. [^1]

[^1]: [consensys article](https://media.consensys.net/scaling-consensus-for-enterprise-explaining-the-ibft-algorithm-ba86182ea668)

## **Key Advantages**  
- **Immediate block finality**  
  There’s only 1 block proposed at a given chain height. The single chain thus removes forking, uncle blocks, and the risk that a transaction may be “undone” once on the chain at a later time.

- **Reduced time between blocks**  
  The effort needed to construct and validate blocks is significantly reduced (in particular with respect to PoW), greatly increasing the throughput of the chain.

- **High data integrity and fault tolerance**  
  IBFT uses a group of validators to ensure the integrity of each block being proposed. A super-majority (~66%) of these validators are required to sign the block prior to insertion to the chain, making block forgery very difficult. The ‘leadership’ of the group also rotates over time — ensuring a faulty node cannot exert long term influence over the chain.

- **Operationally flexible**  
  The group of validators can be modified in time, ensuring the group contains only full-trusted nodes.

## **Consensus Model**
- IBFT uses a pool of validating nodes (**Validator**s) operating on an Ethereum network to determine if a proposed block is suitable for addition to the chain.

- One node of the **Validator**s is deterministically selected as the **Proposer** and is responsible for constructing a block at the block-interval and sharing said block with the  group. If a super-majority of the **Validator**s deem the block to be valid it is added to the blockchain.

- At the completion of the consensus round, the **Validator**s may select a new **Proposer** which will be responsible for providing the candidate Block at the next block interval.

- The consensus mechanism is a synchronised state machine which is responsible for ensuring all **Validator**s append the same block to the chain at the same height.

- If a block fails to insert, the **Proposer** is changed, and the process starts anew.

- To ensure only one block can be appended to the state machine, IBFT prevents changing the proposed block once a super-majority of validators have agreed to its insertion. This process is referred to as ‘Block Locking’.

- The **IBFT** consensus mechanism offers system stability provided less than 1/3 of the validating nodes are behaving incorrectly (either due to being compromised or due to **faulty** code). I.e. to tolerate F **faulty** nodes the validation group must contain at least 3F + 1 nodes (more than this does not increase system integrity).
Note: Herein F implies the number of **faulty** nodes tolerated by the system. 

!!! note 
    A network using IBFT will always produce blocks at a constant interval, regardless of whether there are pending transactions. This means there can be many blocks with zero transactions if the transaction load is low. Ledgerium Blockchain sets the block interval to 5 seconds for all nodes using IBFT.

## **Validation Consortium Membership**  
- The members of the validation group may change over time through a voting mechanism. Members can be added or removed through a majority (Floor(N/2) + 1) vote; each vote is captured in the Block Header.

- Each node in the network (including non-validating nodes) is responsible for tracking the vote tally for each **validator** to determine the current Validators and ensure signatures on mined blocks fall within the expected group.

- Given each vote is contained in the Block Header, only the **Proposer** for a given round is able to cast a vote. Thus it is important, if nodes are to be added/removed in a timely fashion, that the **Proposer** role be updated on a regular basis.

- Once a node reaches majority votes, they immediately join/leave the **validator** group.

- IBFT recognises a Voting Epoch, which defines a point at which all votes which have not yet reached a majority are removed, forcing the voting tally to be restarted. This implies when tallying votes, Validators need only start at the most recent epoch. By default, the Voting Epoch occurs every 30,000 blocks.

- Votes define a change of state (i.e. candidates get voted in, validators get voted out), not voting for a given node implies the **Validator** does not wish the node to change state (an explicit vote is not required to maintain the status quo).

![](../../images/IBFTConsensusProcess.png)

## **States**

- **Awaiting Proposal** Validator is waiting for a new block to be supplied by the current proposer. If the validator is the proposer for this round, they prepare the proposed block and transmit it in a pre-prepare message.
- **Preparing** Has received a (valid) proposed block and notified validator-peers; is now waiting for validator-peers to notify their acceptance of the block.
- **Ready** Has received validator-peer’s acceptance of block, and is waiting for them to be a in a similar position. At this stage the proposed block has been ‘locked-in’, and cannot be replaced until an attempt at insertion has been conducted.
- **Round Change** The round timed out before consensus was reached or the block failed to insert. Wait for all validators to agree on the next round number.

## **Transitions**

- **Awaiting Proposal → Preparing** On reception of a new block (Preprepare message) from the proposer (i.e. the block is valid in its content, as is its proposed chain insertion point).
- **Awaiting Proposal → Round Change** The received proposal was not a valid block according to a given set of rules (e.g. invalid proposer, incorrect round numbering).
- **Preparing → Ready** On reception of 2F+1 notifications (Prepare message) from validator-peers indicating the proposed block is suitable for insertion.
- **Ready → Awaiting Proposal** On reception of 2F+1 notifications (Commit message) from validator-peers indicating they are ready to append the block to the chain. On transition, the process of appending the block to the chain is performed (success).
- **Ready → Round Change** As per Ready->Awaiting Proposal, however, block insertion has failed.
- **Round Change → Awaiting Proposal** 2F+1 of validators agree on the next round number to be used.

## **Block Locking**

IBFT mandates immediate block finality. That essentially means, there will be only one block will be considered for insertion on a chain height from an identified **proposer** at any point in time. Hence either the proposed block is inserted successfully (once sufficient commit messages are received, either in this or subsequent rounds), or the block fails insertion, is discarded, and a new block is proposed at the current chain height.

## **Validation Consortium Membership**

The members of the validation consortium may change over time through a voting mechanism. Members can be added or removed through a majority (Floor(N/2) + 1) vote; each vote is captured in the Block Header. Each node in the network (including non-validating master nodes) is responsible for tracking the vote tally for each validator to determine the current Validators and ensure signatures on mined blocks fall within the expected consortium.

Given each vote is contained in the Block Header, only the **Proposer** for a given round is able to cast a vote. Thus it is important, if nodes are to be added/removed in a timely fashion, that the **Proposer** role be updated on a regular basis. Once a node reaches majority votes, they immediately join/leave the validator group.

IBFT recognises a **Voting Epoch**, which defines a point at which all votes which have not yet reached a majority are removed, forcing the voting tally to be restarted. This implies when tallying votes, **Validators** need only start at the most recent epoch. By default, the Voting Epoch occurs every 30,000 blocks.

Votes define a change of state (i.e. candidates get voted in, validators get voted out), not voting for a given node implies the Validator does not wish the node to change state (an explicit vote is not required to maintain the status quo).

## **IBFT Block Header**
To support IBFT in classic Ethereum, a number of changes are done to the block headers. These changes include:

- mixHash: afixed magic hex string, to identify this block as being IBFT validated "0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365"  
- extraData: contains IBFT specific data including List of Validator Addresses, ProposerSeal (identifies the proposer), CommittingSeals (list of the validators which reported ‘commit’ on this block).
```

As the list of CommittingSeals for each validator is (potentially) different, it is important that the block hash not include this information — i.e. even though two blocks have different CommittingSeals fields, they represent the same information (i.e. transactions etc. are identical).