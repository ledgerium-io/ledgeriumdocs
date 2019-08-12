# **Development Ecosystem**

## **GoLang**
Go, also known as Golang, is a statically typed, compiled programming language designed at Google. Go is syntactically similar to C, but with memory safety, garbage collection, structural typing,and CSP-style concurrency. [^1] Ledgerium Blockchain's Geth client is written in Golang.

## **Node.js**
Node.js® is an open-source, cross-platform JavaScript runtime built on Chrome's V8 JavaScript engine. Node.js executes JavaScript code outside of a browser. Node.js lets developers use JavaScript to write command line tools and for server-side scripting—running scripts server-side. Many of the software written and adopted by Ledgerium Blockchain is written in Node.js. [^2]

## **Solidity**
Solidity is an object-oriented, high-level language for implementing smart contracts. Smart contracts are programs which govern the behaviour of accounts within the Ledgerium state. Solidity was influenced by C++, Python and JavaScript and is designed to target the Ethereum Virtual Machine (EVM). Solidity is statically typed, supports inheritance, libraries and complex user-defined types among other features.

With Solidity you can create contracts for uses such as voting, crowdfunding, blind auctions, and multi-signature wallets. [^3]  Detailed working is available @ [Solidity](/docs/Ledgerium/LedgeriumDAppDevelopment/Solidity.md#Solidity.md) 

## **LedgeriumWeb3.js**
web3.js is a collection of libraries which allow you to interact with a local or remote ethereum node, using a HTTP or IPC connection. However, Ledgerium Blockchain uses XLG as its underlying currency equivalent to ether. In order to Ledgerium JavaScript API to work with XLG denomination, Web3JS library has to be extended to use with XXXX library. 

The following documentation will guide you through installing and running web3.js, as well as providing a API reference documentation with examples. [^4]
https://web3js.readthedocs.io/en/v1.2.0/

## **ledethjs-unit.js**
A simple module for handling Ledgerium units. LedgeriumWeb3.js uses this library to deal with Ledgerium Blockchain units.  [^5]

## **Truffle**
Truffle is a development environment, testing framework and asset pipeline for Ethereum & Ledgerium, aiming to make life as an Ledgerium developer easier. With Truffle, developer get
- Built-in smart contract compilation, linking, deployment and binary management. [^6]
- Automated contract testing with Mocha and Chai.
- Configurable build pipeline with support for custom build processes.
- Scriptable deployment & migrations framework.
- Network management for deploying to many public & private networks.
- Interactive console for direct contract communication.
- Instant rebuilding of assets during development.
- External script runner that executes scripts within a Truffle environment.

## **Remix**
Remix is a powerful, open source tool that helps you write Solidity contracts straight from the browser. Written in JavaScript, Remix supports both usage in the browser and locally.
Remix also supports testing, debugging and deploying of smart contracts and much more. [^7]

## **References**
[^1]: [Golang](https://en.wikipedia.org/wiki/Go_(programming_language) "GoLang")  
[^2]: [Node.js](https://en.wikipedia.org/wiki/Node.js "Node.js")  
[^3]: [Solidity](https://solidity.readthedocs.io "Solidity")  
[^4]: [Web3.js](https://web3js.readthedocs.io "Web3.js")  
[^5]: [ledethjs-unit.js](https://github.com/ledgerium-io/ledethjs-unit "ledethjs-unit.js")  
[^6]: [Truffle](https://www.npmjs.com/package/truffle "Truffle")  
[^7]: [Remix](https://remix.readthedocs.io "Remix")  
