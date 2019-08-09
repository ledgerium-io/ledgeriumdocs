# **What is gas?**
Ledgerium adopts the classic Ethereum execution environment on the blockchain called the Ethereum Virtual Machine (EVM). Every node participating in the network runs the EVM as part of the block verification protocol. They go through the transactions listed in the block they are verifying and run the code as triggered by the transaction within the EVM. Each and every full node in the network does the same calculations and stores the same values. Clearly Ledgerium is not about optimising efficiency of computation. Its parallel processing is redundantly parallel. This is to offer an efficient way to reach consensus on the system state without needing trusted third parties, oracles or violence monopolies. But importantly they are not there for optimal computation. The fact that contract executions are redundantly replicated across nodes, naturally makes them expensive, which generally creates an incentive not to use the blockchain for computation that can be done offchain.

When you are running a decentralized application (dapp), it interacts with the blockchain to read and modify its state, but dapps will typically only put the business logic and state that are crucial for consensus on the blockchain.

When a contract is executed as a result of being triggered by a message or transaction, every instruction is executed on every node of the network. This has a cost: for every executed operation there is a specified cost, expressed in a number of gas units.

Gas is the name for the execution fee that senders of transactions need to pay for every operation made on an Ledgerium blockchain. The name **gas** is inspired by the view that this fee acts as cryptofuel, driving the motion of smart contracts. Gas is purchased for XLG from the miners that execute the code. Gas and XLG are decoupled deliberately since units of gas align with computation units having a natural cost, while the price of XLG generally fluctuates as a result of market forces. The two are mediated by a free market: the price of gas is actually decided by the miners, who can refuse to process a transaction with a lower gas price than their minimum limit. To get gas you simply need to add XLG to your account. The Ledgerium clients automatically purchase gas for your XLG in the amount you specify as your maximum expenditure for the transaction.

The Ledgerium protocol charges a fee per computational step that is executed in a contract or transaction to prevent deliberate attacks and abuse on the Ledgerium network. Every transaction is required to include a gas limit and a fee that it is willing to pay per gas. Miners have the choice of including the transaction and collecting the fee or not. If
the total amount of gas used by the computational steps spawned by the transaction, including the original message and any sub-messages that may be triggered, is less than or equal to the gas limit, then the transaction is processed. If the total gas exceeds the gas limit, then all changes are reverted, except that the transaction is still valid and the fee can still be collected by the miner. All excess gas not used by the transaction execution is reimbursed to the sender as Ether. You do not need to worry about overspending, since you are only charged for the gas you consume. This means that it is useful as well as safe to send transactions with a gas limit well above the estimates.

## **Denominations**  
Ledgerium has a metric system of denominations used as units of XLG. Each denomination has its own unique name (some bear the family name of seminal figures playing a role in evolution of computer science and cryptoeconomics). The smallest denomination aka *base unit* of XLG is called Wei. Below is a list of the named denominations and their value in Wei. Following a common (although somewhat ambiguous) pattern, XLG also designates a unit (of 1e18 or one quintillion Wei) of the currency.

  -------------------------------------------------------------------------- ------------------------------------------
  **Unit**                       **Wei Value**                                        **Wei**
  -------------------------------------------------------------------------- ------------------------------------------
  **wei:**                   1 wei                                            1  
  **Kwei  (babbage)**        1000 wei                                         1,000  
  **Mwei  (lovelace)**       1000,000 wei                                     1,000,000  
  **Gwei  (shannon)**        1000,000,000 wei                                 1,000,000,000  
  **Microether (szabo)**     1000,000,000,000 wei                             1,000,000,000,000  
  **Milliether (finney)**    1000,000,000,000,000 wei                         1,000,000,000,000,000  
  **XLG**                    1000,000,000,000,000,000 wei                     1,000,000,000,000,000,000  
  **Kether (grand)**         1000,000,000,000,000,000,000 wei                 1,000,000,000,000,000,000,000  
  **Mether**                 1000,000,000,000,000,000,000,000 wei             1,000,000,000,000,000,000,000,000  
  **Gether**                 1000,000,000,000,000,000,000,000,000 wei         1,000,000,000,000,000,000,000,000,000  
  **Tether**                 1000,000,000,000,000,000,000,000,000,000 wei     1,000,000,000,000,000,000,000,000,000,000
  
  ------------------------------------------------------------------------------------------------------------------------

Ether can also be transferred using the **geth console**.

```
var sender = eth.accounts[0];
var receiver = eth.accounts[1];
var amount = web3.toWei(0.01, "XLG")
eth.sendTransaction({from:sender, to:receiver, value: amount})
```

For more information on XLG transfer transactions, see account-types-gas-and-transactions.

Ledgerium is unique in the realm of cryptocurrencies in that XLG has utility value as a cryptofuel, commonly referred to as "gas". Beyond transaction fees, gas is a central part of every network request and requires the sender to pay for the computing resources consumed. The gas cost is dynamically calculated, based on the volume and complexity of the request and multiplied by the current gas price. Its value as a cryptofuel has the effect of increasing the stability and long-term demand for XLG and Ledgerium as a whole. For more information, see account-types-gas-and-transactions.

## **Gas and XLG**
-   <https://www.reddit.com/r/ethereum/comments/271qdz/can_someone_explain_the_concept_of_gas_in_ethereum/>
-   <https://www.reddit.com/r/ethereum/comments/3fnpr1/can_someone_possibly_explain_the_concept_of/>
-   <https://www.reddit.com/r/ethereum/comments/49gol3/can_ether_be_used_as_a_currency_eli5_ether_gas/>

Gas is supposed to be the constant cost of network
resources/utilisation. You want the real cost of sending a transaction
to always be the same, so you can't really expect Gas to be issued,
currencies in general are volatile.

So instead, we issue XLG whose value is supposed to vary, but also
implement a Gas Price in terms of Ether. If the price of XLG goes up,
the Gas Price in terms of XLG should go down to keep the real cost of
Gas the same.

Gas has multiple associated terms with it: Gas Prices, Gas Cost, Gas
Limit, and Gas Fees. The principle behind Gas is to have a stable value
for how much a transaction or computation costs on the Ledgerium
network.

-   Gas Cost is a static value for how much a computation costs in terms
    of Gas, and the intent is that the real value of the Gas never
    changes, so this cost should always stay stable over time.
-   Gas Price is how much Gas costs in terms of another currency or
    token like Ether. To stabilise the value of gas, the Gas Price is a
    floating value such that if the cost of tokens or currency
    fluctuates, the Gas Price changes to keep the same real value. The
    Gas Price is set by the equilibrium price of how much users are
    willing to spend, and how much processing nodes are willing to
    accept.
-   Gas Limit is the maximum amount of Gas that can be used per block,
    it is considered the maximum computational load, transaction volume,
    or block size of a block, and miners can slowly change this value
    over time.
-   Gas Fee is effectively the amount of Gas needed to be paid to run a
    particular transaction or program (called a contract). The Gas Fees
    of a block can be used to imply the computational load, transaction
    volume, or size of a block. The gas fees are paid to the miners (or
    bonded contractors in PoS).

### **Estimating transaction costs**
The total XLG cost of a transaction is based on 2 factors:
- `gasUsed` is the total gas that is consumed by the transaction
- `gasPrice` price (in XLG) of one unit of gas specified in the transaction

**Total cost = gasUsed \* gasPrice**

### **gasUsed**
Each operation in the EVM was assigned a number of how much gas it consumes. `gasUsed` is the sum of all the gas for all the operations executed. There is a [spreadsheet](http://Ledgerium.stackexchange.com/q/52/42) which offers a glimpse to some of the analysis behind this.

For estimating `gasUsed`, there is an [estimateGas
API](http://Ledgerium.stackexchange.com/q/266/42) that can be used but
has some caveats.

### **gasPrice**
A user constructs and signs a transaction, and each user may specify whatever `gasPrice` they desire, which can be zero. However, the Ledgerium clients launched at Frontier had a default gasPrice of 0.05e12 wei. As miners optimize for their revenue, if most transactions are being submitted with a gasPrice of 0.05e12 wei, it would be difficult to convince a miner to accept a transaction that specified a lower, or zero, gasPrice.

### **Example transaction cost**
Let’s take a contract that just adds 2 numbers. The EVM OPCODE `ADD` consumes 3 gas.

The approximate cost, using the default gas price (as of Setember 2019), would be:

3 \* 0.05e12 = 1.5e11 wei

Since 1 XLG is 1e18 wei, the total cost would be 0.00000015 XLG.

This is a simplification since it ignores some costs, such as the cost
of passing the 2 numbers to contract, before they can even be added.

-   [question](http://Ledgerium.stackexchange.com/q/324/42)
-   [gas fees](http://docs.ledgerium.io/tool/gas-fees)
-   [gas cost calculator](http://docs.ledgerium.io/tool/calculator)
-   [Ledgerium Gas Prices](https://docs.google.com/spreadsheets/d/1m89CVujrQe5LAFJ8-YAUCcNK950dUzMQPMJBxRtGCqs)

  Operation Name      Gas Cost   Remark
  ------------------- ---------- ----------------------------------------------
  step                1          default amount per execution cycle
  stop                0          free
  suicide             0          free
  sha3                20         
  sload               20         get from permanent storage
  sstore              100        put into permanent storage
  balance             20         
  create              100        contract creation
  call                20         initiating a read-only call
  memory              1          every additional word when expanding memory
  txdata              5          every byte of data or code for a transaction
  transaction         500        base fee transaction
  contract creation   53000      changed in homestead from 21000

### **Account interactions example - betting contract**
As previously mentioned, there are two types of accounts:

-   **Externally owned account (EOAs)**: an account controlled by a
    private key, and if you own the private key associated with the EOA
    you have the ability to send XLG and messages from it.
-   **Contract**: an account that has its own code, and is controlled by
    code.

By default, the Ledgerium execution environment is lifeless; nothing happens and the state of every account remains the same. However, any user can trigger an action by sending a transaction from an externally owned account, setting Ledgerium's wheels in motion. If the destination of the transaction is another EOA, then the transaction may transfer some XLG but otherwise does nothing. However, if the destination is a contract, then the contract in turn activates, and automatically runs its code.

The code has the ability to read/write to its own internal storage (a database mapping 32-byte keys to 32-byte values), read the storage of the received message, and send messages to other contracts, triggering their execution in turn. Once execution stops, and all sub-executions triggered by a message sent by a contract stop (this all happens in a deterministic and synchronous order, ie. a sub-call completes fully before the parent call goes any further), the execution environment halts once again, until woken by the next transaction.

Contracts generally serve four purposes:

-   Maintain a data store representing something which is useful to either other contracts or to the outside world; one example of this is a contract that simulates a currency, and another is a contract that records membership in a particular organization.
-   Serve as a sort of externally-owned account with a more complicated access policy; this is called a "forwarding contract" and typically involves simply resending incoming messages to some desired destination only if certain conditions are met; for example, one can have a forwarding contract that waits until two out of a given three private keys have confirmed a particular message before resending it (ie. multisig). More complex forwarding contracts have different conditions based on the nature of the message sent. The simplest use case for this functionality is a withdrawal limit that is overrideable via some more complicated access procedure. A wallet contract is a good example of this.
-   Manage an ongoing contract or relationship between multiple users. Examples of this include a financial contract, an escrow with some particular set of mediators, or some kind of insurance. One can also have an open contract that one party leaves open for any other party to engage with at any time; one example of this is a contract that automatically pays a bounty to whoever submits a valid solution to some mathematical problem, or proves that it is providing some computational resource.
-   Provide functions to other contracts, essentially serving as a software library.

Contracts interact with each other through an activity that is alternately called either "calling" or "sending messages". A "message" is an object containing some quantity of XLG, a byte-array of data of any size, the addresses of a sender and a recipient. When a contract receives a message, it has the option of returning some data, which the original sender of the message can then immediately use. In this way, sending a message is exactly like calling a function.

Because contracts can play such different roles, we expect that contracts will be interacting with each other. As an example, consider a situation where Alice and Bob are betting 100 GavCoin that the temperature in San Francisco will not exceed 35ºC at any point in the next year. However, Alice is very security-conscious, and as her primary account uses a forwarding contract which only sends messages with the approval of two out of three private keys. Bob is paranoid about quantum cryptography, so he uses a forwarding contract which passes along only messages that have been signed with Lamport signatures alongside traditional ECDSA (but because he's old fashioned, he prefers to use a version of Lamport sigs based on SHA256, which is not supported in Ledgerium directly).

The betting contract itself needs to fetch data about the San Francisco weather from some contract, and it also needs to talk to the GavCoin contract when it wants to actually send the GavCoin to either Alice or Bob (or, more precisely, Alice or Bob's forwarding contract). We can show the relationships between the accounts thus:

![image](../../images/contract_relationship.png)

When Bob wants to finalize the bet, the following steps happen:

1.  A transaction is sent, triggering a message from Bob's EOA to his forwarding contract.
2.  Bob's forwarding contract sends the hash of the message and the Lamport signature to a contract which functions as a Lamport signature verification library.
3.  The Lamport signature verification library sees that Bob wants a SHA256-based Lamport sig, so it calls the SHA256 library many times as needed to verify the signature.
4.  Once the Lamport signature verification library returns 1, signifying that the signature has been verified, it sends a message to the contract representing the bet.
5.  The bet contract checks the contract providing the San Francisco temperature to see what the temperature is.
6.  The bet contract sees that the response to the messages shows that the temperature is above 35ºC, so it sends a message to the GavCoin contract to move the GavCoin from its account to Bob's forwarding contract.

Note that the GavCoin is all "stored" as entries in the GavCoin contract's database; the word "account" in the context of step 6 simply means that there is a data entry in the GavCoin contract storage with a key for the bet contract's address and a value for its balance. After receiving this message, the GavCoin contract decreases this value by some amount and increases the value in the entry corresponding to Bob's forwarding contract's address. We can see these steps in the following diagram:

![image](../../images/contract_relationship2.png)