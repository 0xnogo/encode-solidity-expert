# Homework

```
1.
```

Token Checklist
[] Double withdrawal (front-running): if Alice allow bob (suing `approve`) twice and Bob notice it, we could front-run the approve tx by calling twice `transferFrom`
[x] The decimals can be more than 18
[] Not accounting for the tokens that try to prevent multiple withdrawal attack	
[] Unprotected ‍‍‍‍‍‍‍transferFrom(): looks like the `transferFrom` we are not using the sender and recipient for the allowance but `msg.sender` -> basically the reverse
[x] Unchecked Call Return Value: no call made to contracts
[x] DoS with unexpected revert
[x] Might return False instead of Revert: no false returned
[x] Missing return value: true is returned
[x] Internal Accounting discrepancy with the Actual Balance: LGTM
[x] Blacklisted addresses cannot receive or send tokens: no blacklist
[] TotalSupply can change by trusted actors: cannot be changed (called one in constructor)
[] All functionalities can be paused by trusted actors: no pauseable
[x] Internal Accounting discrepancy with the Actual Balance	
[x] Internal Accounting discrepancy with the Actual Balance	

```
2.
```

Scribble allows you to annotate some function. With a combination oh Mythril, you can detect some security vulnerabilities.

```
/// #if_succeeds {:msg "The sender has sufficient balance at the start"} old(_balances[msg.sender] <= _value);
/// #if_succeeds {:msg "The sender has _value less balance"} msg.sender != _to ==> old(_balances[msg.sender]) - _value == _balances[msg.sender]; 
/// #if_succeeds {:msg "The receiver receives _value"} msg.sender != _to ==> old(_balances[_to]) + _value == _balances[_to]; 
/// #if_succeeds {:msg "Transfer does not modify the sum of balances" } old(_balances[_to]) + old(_balances[msg.sender]) == _balances[_to] + _balances[msg.sender];
function transfer(address _to, uint256 _value) external returns (bool) {
address from = msg.sender;
require(_value <= _balances[from]);


uint256 newBalanceFrom = _balances[from] - _value;
uint256 newBalanceTo = _balances[_to] + _value;
_balances[from] = newBalanceFrom;
_balances[_to] = newBalanceTo;

emit Transfer(msg.sender, _to, _value);
return true;
}
```