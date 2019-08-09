# **Accounts**

Accounts play a central role in Ledgerium Blockchain. They are essential for users to interact with the Ledgerium blockchain via transactions.

Accounts represent identities of external agents (e.g., human role/porfolio, mining computer or computerized agents). Accounts use public key cryptography to sign transactions so that the EVM (Ethereum Virtual Machine) can securely validate the identity of a transaction sender.

There are two types of accounts: _**externally owned accounts**_(EOAs)\* and _**contract accounts**_. Here we focus on externally owned accounts, which will be referred simply as _accounts_ and contract accounts will be referred to as _contracts_.

Both the entities have a state: accounts have balance and contracts have both balance and contract storage. The state of all accounts is reflected as the state of the Ledgerium network which gets updated with every block, post the consensus of blockchain.

If we restrict Ledgerium to only externally owned accounts and allow the transactions between them, we arrive at an "altcoin" system that is less powerful than bitcoin itself and can only be used to transfer XLG.

### **Externally owned accounts (EOAs)**

- has a XLG balance
- can send transactions (XLG transfer or trigger contract code)
- is controlled by private keys
- has no associated code

### **Contract accounts**

- has a XLG balance
- has associated code
- code execution is triggered by transactions or messages (calls) received from other contracts
- during execution:
  - perform operations of arbitrary complexity
  - manipulate its own persistent storage, i.e., can have its own permanent state
  - can contact other contracts

All action on the Ledgerium blockchain is set in motion by transactions fired from externally owned accounts. Every time a contract receives a transaction, its code is executed as instructed by the input parameters sent as part of the transaction. The contract code is executed by the Ethereum Virtual Machine on each participating node in the network as per validation process of new blocks.

This execution needs to be completely deterministic, its only context is the position of the block on the blockchain and all data available. The blocks on the blockchain represent units of time, the blockchain itself is a temporal dimension and represents the entire history of states at the discrete time points designated by the blocks on the chain. [^3]

[^3]: [Deterministic Wikipedia](https://en.wikipedia.org/wiki/Deterministic_system "Deterministic Wikipedia")

All XLG balances and values are denominated in units of wei `1 XLG is 1e18 wei`.

> **Please note**  
> "Contracts" in Ledgerium should not be seen as mandatory, rather, they are more like "autonomous agents" that are part of Ledgerium execution environment. Contracts are always executing a specific piece of code when "poked" by a message or transaction and have direct control over their own XLG balance and key/value store to maintain their permanent state.

## **Key Files**

Every account is accessed by its credentials: private key and public key. Accounts are indexed as per their _address_ derived from the public key made-up of the last 20 bytes. Every private key/address pair is encoded in a _keyfile_.

Keyfiles are JSON text files which can be viewed in any text editor. The critical component of the keyfile, account’s private key, is always encrypted, and it is encrypted with the password you enter when you create the account.

Keyfiles are found in the `keystore` subdirectory of your Ledgerium node’s data directory. Make sure you backup your keyfiles regularly!
See the section backup-and-restore-accounts for more information.

Creating a key is tantamount to creating an account.

- You don't need to tell anybody else you're doing it
- You don't need to synchronize with the blockchain
- You don't need to run a client
- You don't even need to be connected to the internet

Of course your new account will not contain any Ether. But it'll be yours and you can be certain that without your key and your password, nobody else can ever access it.

It is safe to transfer the entire directory or any individual keyfile between Ledgerium nodes.

!!! warning
Note that in case you are adding keyfiles to your node from a different node, the order of accounts may change. So make sure you do not rely or change the index in your scripts or code snippets.

!!! warning
Remember your password.

If you lose the password you use to encrypt your account, you will not be able to access that account.
Repeat: It is NOT possible to access your account without a password and there is no _forgot my password_ option here. Do not forget it.

The ethereum CLI `geth` provides account management via the `account` command:

```
> geth account <command> [options...] [arguments...]
```

Manage accounts lets you create new accounts, list all existing accounts, import a private key into a new account, migrate to newest key format and change your password.

It supports interactive mode, when you are prompted for password as well as non-interactive mode where passwords are supplied via a given password file. Non-interactive mode is only meant for scripted use on test networks or known safe environments.

Make sure you remember the password you gave when creating a new account (with new, update or import). Without it you are not able to unlock your account.

Note that exporting your key in unencrypted format is NOT supported.

Keys are stored under `<DATADIR>/keystore`. Make sure you backup your keys regularly! See [DATADIR backup & restore](https://github.com/ethereum/go-ethereum/wiki/Backup-&-restore)
for more information. If a custom datadir and keystore option are given the keystore option takes preference over the datadir option.

The newest format of the keyfiles is: `UTC--<created_at UTC ISO8601>-<address hex>`. The order of accounts when listing, is lexicographic, but as a consequence of the timespamp
format, it is actually order of creation

It is safe to transfer the entire directory or the individual keys therein between ethereum nodes. Note that in case you are adding keys to your node from a different node,
the order of accounts may change. So make sure you do not rely or change the index in your scripts or code snippets.

And again. **DO NOT FORGET YOUR PASSWORD**

```
COMMANDS:
     list    Print summary of existing accounts
     new     Create a new account
     update  Update an existing account
     import  Import a private key into a new account
```

You can get info about subcommands by `geth account <command> --help`.

```
> geth account list --help
list [command options] [arguments...]

Print a short summary of all accounts

OPTIONS:
  --datadir "/home/bas/.ethereum"  Data directory for the databases and keystore
  --keystore                       Directory for the keystore (default = inside the datadir)
```

Accounts can also be managed via the [Javascript Console](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console)

## Examples

### Interactive use

#### creating an account

```
> geth account new
Your new account is locked with a password. Please give a password. Do not forget this password.
Passphrase:
Repeat Passphrase:
Address: {168bc315a2ee09042d83d7c5811b533620531f67}
```

#### Listing accounts in a custom keystore directory

```
> geth account list --keystore /tmp/mykeystore/
Account #0: {5afdd78bdacb56ab1dad28741ea2a0e47fe41331} keystore:///tmp/mykeystore/UTC--2017-04-28T08-46-27.437847599Z--5afdd78bdacb56ab1dad28741ea2a0e47fe41331
Account #1: {9acb9ff906641a434803efb474c96a837756287f} keystore:///tmp/mykeystore/UTC--2017-04-28T08-46-52.180688336Z--9acb9ff906641a434803efb474c96a837756287f

```

#### Import private key into a node with a custom datadir

```
> geth account import --datadir /someOtherEthDataDir ./key.prv
The new account will be encrypted with a passphrase.
Please enter a passphrase now.
Passphrase:
Repeat Passphrase:
Address: {7f444580bfef4b9bc7e14eb7fb2a029336b07c9d}
```

#### Account update

```
> geth account update a94f5374fce5edbc8e2a8697c15331677e6ebf0b
Unlocking account a94f5374fce5edbc8e2a8697c15331677e6ebf0b | Attempt 1/3
Passphrase:
0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b
Account 'a94f5374fce5edbc8e2a8697c15331677e6ebf0b' unlocked.
Please give a new password. Do not forget this password.
Passphrase:
Repeat Passphrase:
0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b
```

### Non-interactive use

You supply a plaintext password file as argument to the `--password` flag. The data in the
file consists of the raw characters of the password, followed by a single newline.

**Note**: Supplying the password directly as part of the command line is not recommended,
but you can always use shell trickery to get round this restriction.

```
> geth account new --password /path/to/password

> geth account import  --datadir /someOtherEthDataDir --password /path/to/anotherpassword ./key.prv
```

# Creating accounts

## Creating a new account

```
> geth account new
> geth account new --password /path/to/passwdfile
> geth account new --password <(echo $mypassword)

```

Creates a new account and prints the address.

On the console, use:

```
> personal.NewAccount()
... you will be prompted for a password ...

or

> personal.newAccount("passphrase")

```

The account is saved in encrypted format. You **must** remember this passphrase to unlock
your account in the future.

For non-interactive use the passphrase can be specified with the `--password` flag:

```
geth account new --password <passwordfile>
```

Note, this is meant to be used for testing only, it is a bad idea to save your
password to file or expose in any other way.

## Creating an account by importing a private key

```
    geth account import <keyfile>
```

Imports an unencrypted private key from `<keyfile>` and creates a new account and prints
the address.

The keyfile is assumed to contain an unencrypted private key as canonical EC raw bytes
encoded into hex.

The account is saved in encrypted format, you are prompted for a passphrase.

You must remember this passphrase to unlock your account in the future.

For non-interactive use the passphrase can be specified with the `--password` flag:

```
geth account import --password <passwordfile> <keyfile>
```

**Note**: Since you can directly copy your encrypted accounts to another ethereum
instance, this import/export mechanism is not needed when you transfer an account between
nodes.

**Warning:** when you copy keys into an existing node's keystore, the order of accounts
you are used to may change. Therefore you make sure you either do not rely on the account
order or doublecheck and update the indexes used in your scripts.

**Warning:** If you use the password flag with a password file, best to make sure the file
is not readable or even listable for anyone but you. You achieve this with:

```
touch /path/to/password
chmod 700 /path/to/password
cat > /path/to/password
>I type my pass here^D
```

## Updating an existing account

You can update an existing account on the command line with the `update` subcommand with
the account address or index as parameter. You can specify multiple accounts at once.

```
geth account update 5afdd78bdacb56ab1dad28741ea2a0e47fe41331 9acb9ff906641a434803efb474c96a837756287f
geth account update 0 1 2
```

The account is saved in the newest version in encrypted format, you are prompted
for a passphrase to unlock the account and another to save the updated file.

This same command can therefore be used to migrate an account of a deprecated
format to the newest format or change the password for an account.

After a successful update, all previous formats/versions of that same key are removed!

# Importing your presale wallet

Importing your presale wallet is very easy. If you remember your password that is:

```
geth wallet import /path/to/my/presale.wallet
```

will prompt for your password and imports your ether presale account. It can be used
non-interactively with the --password option taking a passwordfile as argument containing
the wallet password in cleartext.

# Listing accounts and checking balances

### Listing your current accounts

From the command line, call the CLI with:

```
> geth account list
Account #0: {5afdd78bdacb56ab1dad28741ea2a0e47fe41331} keystore:///tmp/mykeystore/UTC--2017-04-28T08-46-27.437847599Z--5afdd78bdacb56ab1dad28741ea2a0e47fe41331
Account #1: {9acb9ff906641a434803efb474c96a837756287f} keystore:///tmp/mykeystore/UTC--2017-04-28T08-46-52.180688336Z--9acb9ff906641a434803efb474c96a837756287f
```

to list your accounts in order of creation.

**Note**:
This order can change if you copy keyfiles from other nodes, so make sure you either do not rely on indexes or make sure if you copy keys you check and update your account indexes in your scripts.

When using the console:

```
> eth.accounts
["0x5afdd78bdacb56ab1dad28741ea2a0e47fe41331", "0x9acb9ff906641a434803efb474c96a837756287f"]
```

or via RPC:

```
# Request
> curl -X POST --data '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1} http://127.0.0.1:8545'

# Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": ["0x5afdd78bdacb56ab1dad28741ea2a0e47fe41331", "0x9acb9ff906641a434803efb474c96a837756287f"]
}
```

If you want to use an account non-interactively, you need to unlock it. You can do this on
the command line with the `--unlock` option which takes a comma separated list of accounts
(in hex or index) as argument so you can unlock the accounts programmatically for one
session. This is useful if you want to use your account from Dapps via RPC. `--unlock`
will unlock the first account. This is useful when you created your account
programmatically, you do not need to know the actual account to unlock it.

Create account and start node with account unlocked:

```
geth account new --password <(echo this is not secret!)
geth --password <(echo this is not secret!) --unlock primary --rpccorsdomain localhost --verbosity 6 2>> geth.log
```

Instead of the account address, you can use integer indexes which refers to the address
position in the account listing (and corresponds to order of creation)

The command line allows you to unlock multiple accounts. In this case the argument to
unlock is a comma delimited list of accounts addresses or indexes.

```
geth --unlock "0x407d73d8a49eeb85d32cf465507dd71d507100c1,0,5,e470b1a7d2c9c5c6f03bbaa8fa20db6d404a0c32"
```

If this construction is used non-interactively, your password file will need to contain
the respective passwords for the accounts in question, one per line.

On the console you can also unlock accounts (one at a time) for a duration (in seconds).

```
personal.unlockAccount(address, "password", 300)
```

Note that we do NOT recommend using the password argument here, since the console history
is logged, so you may compromise your account. You have been warned.

### Checking account balances

To check your the etherbase account balance:

```
> web3.fromWei(eth.getBalance(eth.coinbase), "ether")
6.5
```

Print all balances with a JavaScript function:

```
function checkAllBalances() {
    var totalBal = 0;
    for (var acctNum in eth.accounts) {
        var acct = eth.accounts[acctNum];
        var acctBal = web3.fromWei(eth.getBalance(acct), "ether");
        totalBal += parseFloat(acctBal);
        console.log("  eth.accounts[" + acctNum + "]: \t" + acct + " \tbalance: " + acctBal + " ether");
    }
    console.log("  Total balance: " + totalBal + " ether");
};
```

That can then be executed with:

```
> checkAllBalances();
  eth.accounts[0]: 0xd1ade25ccd3d550a7eb532ac759cac7be09c2719 	balance: 63.11848 ether
  eth.accounts[1]: 0xda65665fc30803cb1fb7e6d86691e20b1826dee0 	balance: 0 ether
  eth.accounts[2]: 0xe470b1a7d2c9c5c6f03bbaa8fa20db6d404a0c32 	balance: 1 ether
  eth.accounts[3]: 0xf4dd5c3794f1fd0cdc0327a83aa472609c806e99 	balance: 6 ether
```

Since this function will disappear after restarting geth, it can be helpful to store
commonly used functions to be recalled later. The
[loadScript](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console#loadscript)
function makes this very easy.

First, save the `checkAllBalances()` function definition to a file on your computer. For
example, `/Users/username/gethload.js`. Then load the file from the interactive console:

```
> loadScript("/Users/username/gethload.js")
true
```

The file will modify your JavaScript environment as if you has typed the commands
manually. Feel free to experiment!

# **Backup and restore accounts**

## **Manual backup/restore**

You must have an account’s keyfile to be able to send any transaction
from that account. Keyfiles are found in the keystore subdirectory of
your Ledgerium node’s data directory. The default data directory
locations are platform specific:

- Windows: `%appdata%\Ledgerium\keystore`
- Linux: `~/.Ledgerium/keystore`
- Mac: `~/Library/Ledgerium/keystore`

To backup your keyfiles (accounts), copy either the individual
keyfiles within the `keystore` subdirectory or copy the entire
`keystore` folder.

To restore your keyfiles (accounts), copy the keyfiles back into the
`keystore` subdirectory, where they were originally.

## **Importing an unencrypted private key**

Importing an unencrypted private key is supported by `geth`

```
geth account import /path/to/<keyfile
```

This command imports an unencrypted private key from the plain text
file `<keyfile` and creates a new account and prints the address. The
keyfile is assumed to contain an unencrypted private key as canonical
EC raw bytes encoded into hex. The account is saved in encrypted
format, you are prompted for a passphrase. You must remember this
passphrase to unlock your account in the future.

An example where the data directory is specified. If the `--datadir`
flag is not used, the new account will be created in the default data
directory, i.e., the keyfile will be placed in the `keystore`
subdirectory of the data directory.

```
$ geth --datadir /someOtherEthDataDir  account import ./key.prv
The new account will be encrypted with a passphrase.
Please enter a passphrase now.
Passphrase:
Repeat Passphrase:
Address: {7f444580bfef4b9bc7e14eb7fb2a029336b07c9d}
```

For non-interactive use the passphrase can be specified with the
`--password` flag:

```
geth --password <passwordfileaccount import <keyfile
```

!!! note
Since you can directly copy your encrypted accounts to another
Ledgerium instance, this import/export mechanism is not needed when
you transfer an account between nodes.

#### **warning**

When you copy keys into an existing node's `keystore`, the order of
accounts you are used to may change. Therefore you make sure you
either do not rely on the account order or double-check and update
the indexes used in your scripts.
