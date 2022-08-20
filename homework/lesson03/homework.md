# Homework - lesson 3
```
1. 
```
EVM is a 256-bit word virtual machine.

Pros:
* Facilitates Keccak256 hash scheme and elliptic-curve computation
* Allows to store big values, addresses (20bytes)
* Less chance of getting collisions


Cons:
* For regular math (with small number), 256 bit is an overkill and it is making all the small operation very expensive (bool, chars, array indexes). Natively mainstream hardware support 64-bit words.
* Overflow: if the result doesn't fit into 256 bits, the higher bits are just dropped.

Some good resources:
* [StackExchange](https://ethereum.stackexchange.com/questions/49628/wouldnt-a-64bit-based-evm-be-more-efficient-on-the-ethereum-ecosystem)
* [EVM design mistakes](https://medium.com/coinmonks/10-evm-design-mistakes-7c9a75a77ee9#:~:text=EVM%20is%20a%20256%2Dbit,result%20that%20is%20mathematically%20incorrect.)

```
2.
```
Node running that client will then come out with different values when running tx to the one running on miners. Consensus break at that point that would result in a fork. Depending on how long this is taking to get fixed, but eventually they would all agree after the fix

```
3.
```
The beneficiary is the address that will collect the fee from a successful mining.

Currently any one is allowed to produce blocks and miner are incentivized so their is no need.
```
4.
```
When declaring variables like String or array, `MSTORE` is triggered and data is being save to memory.

But it looks like for types < 32bytes, data is directly pushed to the stack. Is there a way to force a MSTORE?

```
    function test() public pure {
        uint256 firstMemory = 1000; // pushed to the stack - less than 32bytes?
        bool memBool = true; // pushed to the stack - less than 32bytes?
        uint[] memory memArray = new uint[](1);

        memArray[0] = 1000; // MSTORE

        string memory strMem = "Salut toi"; // MSTORE
    }
```