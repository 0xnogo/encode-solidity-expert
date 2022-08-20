# Homework
```
1. 
```
Negative number are more expensive than positive as the leftmost digit will be use to represent the sign (1 for negative and 0 for positive).

Negative numbers are stored with a starting value of fffff. Which is more expensive

```
2.
```

```
function test(uint256 numerator, uint256 denominator) public pure returns (uint result) {
    result = numerator / denominator;
}
```
Costs 22 278

```
function test2(uint256 numerator, uint256 denominator) public pure returns (uint result) {
    assembly {
        result:= div(numerator, denominator)
    }
}
```
Costs 22 105

In solidity there is a zero check. So if we are sure that the value is not 0, we can safely do it.