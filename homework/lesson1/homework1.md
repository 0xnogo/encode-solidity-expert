# Homework - lesson 1

## 1. What information is held for an Ethereum account?

An Ethereum account holds the account state which is composed by:
* nonce: if the account is an EOA, this number represents the number of transactions sent from the account's address. If the account is a contract account, the nonce is the number of contracts created by the account
* balance: number of Wei owned by this address
* storageRoot: A hash of the root node of a Merkle Patricia tree. This tree encodes the hash of the storage contents of this account.
* codeHash: hash of the EVM code of this account. For EOA, this field is the hash of an empty string.

## 2. Where is the full Ethereum state held?

Some nodes will hold the state. But a lot of the time the information is produced by recreating state rather storing it. Nodes can also communicate state to each others

## 3. What is a replay attack? Which 2 pieces of information can prevent it?

*Inspired from: https://www.youtube.com/watch?v=jq1b-ZDRVDc*

Signature can be used to reduce the number of transactions.

Example:
* Alice want to withdraw in a Multi-sig
* This will leads to 3 tx:
    * Alice approving the operation
    * Eve approving the operation
    * Alice withdrawing
* With a Multi-sig contract dealing with signature, Eve can simply sign a hash and share it off-chain to Alice. Alice will then submit a tx with the withdraw and the 2 signatures.

The thing is that the tx could be re-executed even though Eve most proabbly signed for only one withdraw. Alice could re-use the same tx to drain the contract, or even use the same signatures for another contract. This is what is call the replay attack

To prevent signature replay attacks, 2 elements can be introduced
* Include the nonce and the address of the contract in hash to sign 
* Keeping track of all the tx hash executed using a mapping of the hashes used.

## 4. What checks are made on transactions for view functions?

`view` functions are not modifying the state and allow only reading the state.
EVM compiler is using STATICCALL when view functions are called, which enforces the state to stay unmodified as part of the EVM execution. This means library view functions do not have runtime checks that prevent state modifications.
