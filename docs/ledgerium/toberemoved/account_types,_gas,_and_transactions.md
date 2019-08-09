


### **What is a message?**
Contracts have the ability to send "messages" to other contracts. Messages are virtual objects that are never serialized and exist only in the Ledgerium execution environment. They can be conceived of as function calls.

A message contains:
-   the sender of the message (implicit).
-   the recipient of the message
-   `VALUE` field - The amount of wei to transfer alongside the message to the contract address,
-   an optional data field, that is the actual input data to the contract
-   a `STARTGAS` value, which limits the maximum amount of gas the code execution triggered by the message can incur.

Essentially, a message is like a transaction, except it is produced by a contract and not an external actor. A message is produced when a contract currently executing code executes the `CALL` or `DELEGATECALL` opcodes, which produces and executes a message. Like a transaction, a message leads to the recipient account running its code. Thus, contracts can have relationships with other contracts in exactly the same way that external actors can.

