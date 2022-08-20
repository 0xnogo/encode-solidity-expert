# Homework

```
1.
```
Drawbacks of
* Commit-reveal approach: it consist of sending the value to a contract plus a commit (hash that can be verified). This commit doesn't mean anything to an attacker. In the reveal, the user will ask to the contract to perform the operation. The contract will be able to verify and for example send the token. Main issue is the need of sending 2 tx which is making it less gas efficient and also more complex in a way that client will have to deal with 2 tx
* Salmonella: it was very efficient at the beginning, but now all bots have upgraded and are checking that they are receiving the right amount of token.

```
2.
```
Subway contract: the contract is just compose by a fallback that is realizing the frontslice and backslice of the sandwitch by basically sending X ERC to the pair uniswap address and swapping Y.
The backslice will just do the same (opposite).

The code is fully realized in YUL which allows some optimization like the non SSTORE of the owner by pushing it at the end of the code bytecode (datacopy).

The rest of the package is the bot, which is written in js:
* listening on mempool and filtering only the swapExactETHForToken
* getting the right amountOut to swap (front)
* checking profitability
* creating the bundle
* calling flashbots
* (and some other stuff)


```
3.
```
* It looks like the confirmed root was only added into a map (root => timestamp). This alone was enough to prove that the root was ok. The root was not reproven against the Merkle Tree
* Check that the message passed in not 0x0
* Have a map on the root already executed (this could have limit to on malicious tx using this attack vector)

