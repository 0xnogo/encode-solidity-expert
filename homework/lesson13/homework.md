# Homework

```
1.
```
Audit DogeCoinGame:
* [WARNING]: the addPlayer function will emit an event for each player above 200. So the UI risk of being called multiple time.
* [WARNING]: no limit in terms of player
* [WARNING]: player would still need to register even without providedin the 1 eth
* [WARNING]: The amount to pay will be equal to 1 wei as (in theory) we should have 100 items in the winners array and we are deviding it by 100. We should send `1ether` or multiply it by 10e18.
* [CRITICAL]: all methods are public. `payout`, `payWinners` and `addWinner` shoul be restricted to the owner of the contract 
* [CRITICAL]: Out of bound index array in `payWinners` -> i < winners.length
* [OPTIMIZATION]: let the user claim its token instead of looping through 100 winners
* [OPTIMIZATION]: inctead of adding the winner in an array one by one, we can think of a merkle tree. By this the contract will only have to store the bytes32 root. At the same time the payWinner can just verify that the winner is part of the tree and send the amount.

```
2.
```

An attacker could call the `acceptBid` by switching the address of the ERC721 and ERC20. The call would work:
* XOR is commutative so we will end up with the same key
* ERC721 of solmate `safeTransfer` and ERC20 `transfer` has the same function signature so methods will be called.

The attacker could then get an NFT from someone already placing a bit from more or less nothing.

https://www.youtube.com/watch?v=JicU29EBaCo