# Homework

List of issues found thanks to testing

### Wrong initialization of `initialBalance`

```
function testCorrectPayoutAmount() public {
    defi.addInvestor(address(alice));
    vm.startPrank(address(alice));
    vm.roll(1);
    uint256 payoutAmount = defi.calculatePayout();
    defi.claimTokens();
    uint256 aliceBalance = token.balanceOf(address(alice));
    assert(payoutAmount == 0);
    assertEq(aliceBalance, payoutAmount);
}
```

### Payout never set, leading to no token transfer

```
function testCorrectPayoutAmount() public {
    defi.addInvestor(address(alice));
    vm.startPrank(address(alice));
    vm.roll(1);
    uint256 payoutAmount = defi.calculatePayout();
    defi.claimTokens();
    uint256 aliceBalance = token.balanceOf(address(alice));
    assert(payoutAmount != 0);
    assertEq(aliceBalance, payoutAmount);
}
```

### Investors size bigger that initialAmount -> payout = 0

```
function testAddingManyInvestorsAndClaiming() public {
    for (uint256 i = 0; i < initialAmount*2; i++) {
        defi.addInvestor(vm.addr(i+1));
        assert(defi.investors(i) == vm.addr(i+1));
    }

    address investor = defi.investors(0);
    vm.startPrank(investor);
    vm.roll(1);
    uint256 payoutAmount = defi.calculatePayout();
    defi.claimTokens();
    uint256 investorBalance = token.balanceOf(investor);
    assert(payoutAmount != 0);
}
```

### Anyone can add an investor

```
function testAddingInvestorForNonAdmin() public {
    vm.expectRevert();
    vm.prank(vm.addr(1));
    defi.addInvestor(vm.addr(2));
}
```

### when no investor, we will divide by 0 (investors.length)

### if initialAmount = 0 then no token are given to the contract (transfer will fail)

### `block.number` is divideable by 1000 (no remainder) then the payout = 0

```
function claimingForDifferentInitialAmount() public {
    defi.addInvestor(address(alice));
    defi.addInvestor(address(bob));
    vm.prank(address(alice));
    vm.roll(1000);
    defi.claimTokens();
    uint256 aliceBalance = token.balanceOf(address(alice));
    assert(payoutAmount != 0);
    assertEq(aliceBalance, payoutAmount);
}
```

could be generalize by something like this:
```
function testClaim(uint256 blockNo) public {
    defi.addInvestor(address(alice));
    defi.addInvestor(address(bob));
    vm.prank(address(alice));
    vm.roll(blockNo);
    defi.claimTokens();
}
```

> Can we initialize the contract in the `setUp()` with multiple values