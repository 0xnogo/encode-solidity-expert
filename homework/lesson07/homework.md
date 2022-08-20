# Homework
```
1. 
```

```
pragma solidity ^0.8.4;

contract Ex1 {
    function foo() payable public returns (uint256) {
        assembly {
            let val := callvalue()
            mstore(0x20, val)
            return(0x20, 32)
        }
    }
}
```

```
2.
```
This code represents a contract that self destruct after creating 2 new contracts based on the code (codecopy) of the current contract. For one contract, it is receiving the call value and the second one the remaining balance (with the self destruct).

0x601e8060093d393df3 should represent the initialization code (constructor) where the bytecode is return. `1e` looks like the codesize of the actual runtime code.

Though, I am not sure of the usefulness of this duplicatable contracts.


```
3.
```

```
function allowanceStorageOffset(account, spender) -> offset {
    offset := accountToStorageOffset(account)
    mstore(0, offset)
    mstore(0x20, spender)
    offset := keccak256(0, 0x40)
}
```

This method is returning the offset in the storage of where the allowance per user is stored. When trying to get the allowance, we will sload from the offset returned by the method.
* accountToStorageOffset: adding 0x1000 to the account address
* storing in memory the offset at address 0
* storing in memory the address of the spender at address 0x20
* returning the offset which is the keccak256 of the offset and the spender