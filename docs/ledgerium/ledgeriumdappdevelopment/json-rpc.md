# **JSON RPC API**
An Ledgerium node offers a [RPC](https://wikipedia.org/wiki/Remote_procedure_call) interface. This interface gives Dapp's access to the Ledgerium blockchain and functionality that the node provides, such as compiling smart contract code. It uses a subset of the [JSON-RPC 2.0](http://www.jsonrpc.org/specification) specification (no support for notifications or named parameters) as the serialisation protocol and is available over HTTP and IPC (unix domain sockets on linux/OSX and named pipe's on Windows). it's time to dive into the details of communicating with the Ledgerium network and smart contracts.

JSON-RPC is a stateless, light-weight remote procedure call (RPC) protocol. Primarily this specification defines several data structures and the rules around their processing. It is transport agnostic in that the concepts can be used within the same process, over sockets, over HTTP, or in many various message passing environments. It uses JSON (RFC 4627) as data format.

## **JavaScript API**
If you are not interested in the details but are looking for an easy to use javascript library you can skip the following sections and continue with looking up ledgeriumWeb3.js and quorum.js. 

## **JSON-RPC Endpoint**
Default JSON-RPC endpoints:

### **Client URL**
Go	http://testnet.ledgeium.net:8545

### **Go Interface**
You can start the HTTP JSON-RPC with the --rpc flag

```
geth --rpc
```

change the default port (8545) and listing address (localhost) with:

```
geth --rpc --rpcaddr <ip> --rpcport <portnumber>
```

If accessing the RPC from a browser, CORS will need to be enabled with the appropriate domain set. Otherwise, JavaScript calls are limit by the same-origin policy and requests will fail:

```
geth --rpc --rpccorsdomain "http://testnet.ledgeium.net:3000"
```

The JSON RPC can also be started from the geth console using the admin.startRPC(addr, port) command.

## **JSON-RPC methods**
- web3_clientVersion
- web3_sha3
- net_version
- net_peerCount
- net_listening
- eth_protocolVersion
- eth_syncing
- eth_coinbase
- eth_mining
- eth_hashrate
- eth_gasPrice
- eth_accounts
- eth_blockNumber
- eth_getBalance
- eth_getStorageAt
- eth_getTransactionCount
- eth_getBlockTransactionCountByHash
- eth_getBlockTransactionCountByNumber
- eth_getUncleCountByBlockHash
- eth_getUncleCountByBlockNumber
- eth_getCode
- eth_sign
- eth_sendTransaction
- eth_sendRawTransaction
- eth_call
- eth_estimateGas
- eth_getBlockByHash
- eth_getBlockByNumber
- eth_getTransactionByHash
- eth_getTransactionByBlockHashAndIndex
- eth_getTransactionByBlockNumberAndIndex
- eth_getTransactionReceipt
- eth_pendingTransactions
- eth_getUncleByBlockHashAndIndex
- eth_getUncleByBlockNumberAndIndex
- eth_getCompilers
- eth_compileLLL
- eth_compileSolidity
- eth_compileSerpent
- eth_newFilter
- eth_newBlockFilter
- eth_newPendingTransactionFilter
- eth_uninstallFilter
- eth_getFilterChanges
- eth_getFilterLogs
- eth_getLogs
- eth_getWork
- eth_submitWork
- eth_submitHashrate
- eth_getProof
- db_putString
- db_getString
- db_putHex
- db_getHex
- shh_post
- shh_version
- shh_newIdentity
- shh_hasIdentity
- shh_newGroup
- shh_addToGroup
- shh_newFilter
- shh_uninstallFilter
- shh_getFilterChanges
- shh_getMessages

## **Additional Istanbul RPC API** 
Istanbul package provides ways of interacting with the consensus mechanism, it allows new validators to be added and existing validators to be removed. It allows managing the number of validators by a voting mechanism which involves at least 2F+1 nodes. The details of active nodes can be found in the extra data of each block that has been committed using the istanbul-tools. One can decode the data in the following way

### **istanbul extra decode extradata**
- This extra data contains one seal and 2F+1 committed seals, these committed seals are omitted when the block hash is calculated
- The committed seals maybe different in different blocks, but as this is not included in the blockhash, the hash can be used to verify across other nodes

```
./istanbul extra decode --extradata "0xd98301080c846765746889676f312e31312e3131856c696e7578000000000000f90164f85494645cab2686477cd244562d4bd95a75f157a4c05594a79f57a6884a71a25fd17c21549796a53c06737694f232a4bf183cf17d09bea23e19ceff58ad9dbfed94f7dce95345c5a5917360571165260cf0e8d98918b84155bbcc22fd38db62a4fe98b685cd91b51157787fedd63038ccfb76bafd6055093ada24bd77d9123e14d3e96f49cb6fa642b53eeae5260d71fabcbd4045ce09be01f8c9b841faeb5803370b2f7d774ea5dcf730efbe9c600627c6baba7764336c39f5950300180bcb180b61cd1c47b323ddccce3f6fbac6d1ad8685ec91cb2ca51744e7ac2100b841075136cd9e6f48456a9ef209182370372314f9140d96618df400a87f76f4ee0b055087bb3be76bee24433d714ed9671fd21d586019b0178e10502baa12f5c79101b84147ba9486c1c2d9a4dc1e933c61a573d9ec41c7d70b16c15644c48e9a61537bae33e4566f66374fd5b8e1838c849de19d9a9ec930eff05f647ace37b3b4c3dcb201"
vanity:  0xd98301080c846765746889676f312e31312e3131856c696e7578000000000000
validator:  0x645cab2686477Cd244562D4BD95A75F157A4c055
validator:  0xA79f57A6884a71A25fD17C21549796a53c067376
validator:  0xF232A4BF183cF17D09Bea23e19CEfF58Ad9dbFED
validator:  0xf7dcE95345c5a5917360571165260CF0e8d98918
seal: 0x55bbcc22fd38db62a4fe98b685cd91b51157787fedd63038ccfb76bafd6055093ada24bd77d9123e14d3e96f49cb6fa642b53eeae5260d71fabcbd4045ce09be01
committed seal:  0xfaeb5803370b2f7d774ea5dcf730efbe9c600627c6baba7764336c39f5950300180bcb180b61cd1c47b323ddccce3f6fbac6d1ad8685ec91cb2ca51744e7ac2100
committed seal:  0x075136cd9e6f48456a9ef209182370372314f9140d96618df400a87f76f4ee0b055087bb3be76bee24433d714ed9671fd21d586019b0178e10502baa12f5c79101
committed seal:  0x47ba9486c1c2d9a4dc1e933c61a573d9ec41c7d70b16c15644c48e9a61537bae33e4566f66374fd5b8e1838c849de19d9a9ec930eff05f647ace37b3b4c3dcb201
```

### **Block header with Ledgerium Blockchain** 
```
> eth.getBlock(100)
{
  difficulty: 1,
  extraData: "0xd98301080c846765746889676f312e31312e3131856c696e7578000000000000f90164f85494645cab2686477cd244562d4bd95a75f157a4c05594a79f57a6884a71a25fd17c21549796a53c06737694f232a4bf183cf17d09bea23e19ceff58ad9dbfed94f7dce95345c5a5917360571165260cf0e8d98918b84155bbcc22fd38db62a4fe98b685cd91b51157787fedd63038ccfb76bafd6055093ada24bd77d9123e14d3e96f49cb6fa642b53eeae5260d71fabcbd4045ce09be01f8c9b841faeb5803370b2f7d774ea5dcf730efbe9c600627c6baba7764336c39f5950300180bcb180b61cd1c47b323ddccce3f6fbac6d1ad8685ec91cb2ca51744e7ac2100b841075136cd9e6f48456a9ef209182370372314f9140d96618df400a87f76f4ee0b055087bb3be76bee24433d714ed9671fd21d586019b0178e10502baa12f5c79101b84147ba9486c1c2d9a4dc1e933c61a573d9ec41c7d70b16c15644c48e9a61537bae33e4566f66374fd5b8e1838c849de19d9a9ec930eff05f647ace37b3b4c3dcb201",
  gasLimit: 9007199254740000,
  gasUsed: 0,
  hash: "0xb2dc0d9e6328a82a65a5ed8d3a59e5a236eb681178197886a088f79d2b8b8741",
  logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  miner: "0xa79f57a6884a71a25fd17c21549796a53c067376",
  mixHash: "0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365",
  nonce: "0x0000000000000000",
  number: 100,
  parentHash: "0x71676cf741e3ed0c1c1ab86ad173acb482ec9f6499a285ed9e0a2fc4413ea9fc",
  receiptsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
  sha3Uncles: "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
  size: 905,
  stateRoot: "0x8fb6b60f26cd0ab16eabf7a2e39b27f43f43e6031da5e31825b0940c310c3ba0",
  timestamp: 1564032229,
  totalDifficulty: 101,
  transactions: [],
  transactionsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
  uncles: [],
  validators: ["0xa79f57a6884a71a25fd17c21549796a53c067376", "0xf7dce95345c5a5917360571165260cf0e8d98918", "0xf232a4bf183cf17d09bea23e19ceff58ad9dbfed"]
}
```
The extra data doesn’t include the committed seals as these can vary from block to block, so the only extra data is the rlpHash of the validators and seal. When a block is sealed the block seal is computed as ECDSA(rlpHash(Block_Header), private_key).

### **istanbul.getSnapshot()**
This call shows the current state of the blockchain and gives the below structure as output.
-   epoch : Indicates the number of blocks after which the proposal which hasn't been resolved is cleared
-   hash : The current block hash at the given block number
-   tally : The object maintaining the current state of votes of all the validators
-   validators : The list of all the addresses which can act as validators
-   votes : The individual votes casted by validators as an array
-   authorize : true - proposal to add as a validator, false - proposal to remove as a validator

```  
> istanbul.getSnapshot()
{
  epoch: 30000,
  hash: "0x35e7085891327d0054122df314f261a95d479271123f38f3de9684de37a5c14a",
  number: 261306,
  policy: 0,
  tally: {},
  validators: ["0x2296365c074db8bece7d8443d1ece22384ae1ee7", "0x645cab2686477cd244562d4bd95a75f157a4c055", "0x7cdf4118fd559efae92c0876fcee2afe767f0956", "0xa79f57a6884a71a25fd17c21549796a53c067376", "0xbefa0195759586a328502339f9420b8a6603bc82", "0xc7a89e9c2d5acd30cf78f8a9ef4777c10bb69a07", "0xd25df157d2a64e5eba4bc1363f5a386ec36154f3", "0xf232a4bf183cf17d09bea23e19ceff58ad9dbfed", "0xf7dce95345c5a5917360571165260cf0e8d98918"],
  votes: []
}
```  


### **istanbul.getSnapshotAtHash()**
Used to get the current state of the voting and active validators as of a particular block hash, takes as argument a block hash.
```  
> istanbul.getSnapshotAtHash("0x35e7085891327d0054122df314f261a95d479271123f38f3de9684de37a5c14a")
{
  epoch: 30000,
  hash: "0x35e7085891327d0054122df314f261a95d479271123f38f3de9684de37a5c14a",
  number: 261306,
  policy: 0,
  tally: {},
  validators: ["0x2296365c074db8bece7d8443d1ece22384ae1ee7", "0x645cab2686477cd244562d4bd95a75f157a4c055", "0x7cdf4118fd559efae92c0876fcee2afe767f0956",   "0xa79f57a6884a71a25fd17c21549796a53c067376", "0xbefa0195759586a328502339f9420b8a6603bc82", "0xc7a89e9c2d5acd30cf78f8a9ef4777c10bb69a07", "0xd25df157d2a64e5eba4bc1363f5a386ec36154f3", "0xf232a4bf183cf17d09bea23e19ceff58ad9dbfed", "0xf7dce95345c5a5917360571165260cf0e8d98918"],
  votes: []
}
```  

### **istanbul.getValidators()**
Returns the list of active validators who can commit a block. 
```
> istanbul.getValidators() 
["0x2296365c074db8bece7d8443d1ece22384ae1ee7", "0x645cab2686477cd244562d4bd95a75f157a4c055", "0x7cdf4118fd559efae92c0876fcee2afe767f0956", "0xa79f57a6884a71a25fd17c21549796a53c067376", "0xbefa0195759586a328502339f9420b8a6603bc82", "0xc7a89e9c2d5acd30cf78f8a9ef4777c10bb69a07", "0xd25df157d2a64e5eba4bc1363f5a386ec36154f3", "0xf232a4bf183cf17d09bea23e19ceff58ad9dbfed", "0xf7dce95345c5a5917360571165260cf0e8d98918"]
```

### **istanbul.getValidatorsAtHash()**
Returns the list of active validators who can commit a block as of a particular block hash, takes as argument a block hash
``` 
> istanbul.getValidatorsAtHash("0x35e7085891327d0054122df314f261a95d479271123f38f3de9684de37a5c14a")
["0x2296365c074db8bece7d8443d1ece22384ae1ee7", "0x645cab2686477cd244562d4bd95a75f157a4c055", "0x7cdf4118fd559efae92c0876fcee2afe767f0956", "0xa79f57a6884a71a25fd17c21549796a53c067376", "0xbefa0195759586a328502339f9420b8a6603bc82", "0xc7a89e9c2d5acd30cf78f8a9ef4777c10bb69a07", "0xd25df157d2a64e5eba4bc1363f5a386ec36154f3", "0xf232a4bf183cf17d09bea23e19ceff58ad9dbfed", "0xf7dce95345c5a5917360571165260cf0e8d98918"]
```

### **istanbul.candidates**
Gives all the addresses with the respective votes casted.

``` 
> istanbul.candidates
{
  0x2296365c074db8bece7d8443d1ece22384ae1ee7: true,
  0x7cdf4118fd559efae92c0876fcee2afe767f0956: true,
  0xbefa0195759586a328502339f9420b8a6603bc82: true,
  0xc7a89e9c2d5acd30cf78f8a9ef4777c10bb69a07: true
}
```   

### **istanbul.getCandidates(callback)**
Gives all the addresses with the respective votes casted by that particular node as a error first callback

### **istanbul.discard()**
It discards the vote casted for the respective address.
```  
> istanbul.discard("0x7cdf4118fd559efae92c0876fcee2afe767f0956")
null
```  

### **istanbul.propose()**
This option is disabled in Ledgerium Blockchain. It can only be performed through masternode's [Governance DApp](http://testnet.ledgerium.net:3545). 
