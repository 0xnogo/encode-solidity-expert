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

* Scenario 1: the implementation provide the same result but with a different time/space complexity. Each operation in precompiled contract is associated to an OPCODE with a predefined gas cost. If the complexity of the execution of OPCODEs varies, then miners will have different mining cost for the same tx. Also if it takes longer to execute one operation, then some miners will have a disadvantages when competing for the next block mining.
* Scenario 2: the implementation provide different result. This will provoke massive inconsistencies in the blockchain. Some contracts won't be able to execute properly the code or we can have even issue when node will try to verify mined blocked.

```
3.
```
The beneficiary is the address that will collect the fee from a successful mining.

I am guessing that a miner is putting it's own address in the beneficiary field and then mining the block.

If the beneficiary is not validated, then a malicious actor could just take the same block and change the beneficiary field and try to get the block accepeted before the original one.

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