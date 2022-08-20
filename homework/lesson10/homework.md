# Homework

```
3.
```
How to mitigate MEV?
* have a third party which is ordering the tx (not incentivized based on tip or auction). MEV is only possible because miners are choosing the txs
* have the mem pool being a FIFO queue
* for sandwitch attack: reduce the slippage allowed

MEV should not be mitigated has it is an important part of the Eth ecosystem. Whitout MEV:
* there is not defi (no one would land their fund to Protocol like AAVE, if the borrower could not be liquidated after being under-collateralized)
* MEV and project like flashbots allowed mining activity to remain sustainable. Without it, we could see a concentration of miners mining a higher risk of bandit attack.

```
4.
```
* It looks like the confirmed root was only added into a map (root => timestamp). This alone was enough to prove that the root was ok. The root was not reproven against the Merkle Tree
* Check that the message passed in not 0x0
* Have a map on the root already executed (this could have limit to on malicious tx using this attack vector)
* Better unit testing (fuzzing?)
