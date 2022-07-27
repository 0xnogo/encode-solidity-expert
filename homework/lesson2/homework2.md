# Homework - lesson 2

## Try out the Solidity template

> 1. Start a new project using the Solidity Template

Hardhat based template already configured to with Typescript, Github hooks, linting, code coverage tools and gas optimization. Very handy to use.

> 2. Fork the mainnet 

2 options:
* Use the `hardhat` network in the hardhat config file. The `hardhat` network is used when the test command is run for example
* from the CLI `npx hardhat node --fork https://rpc.ankr.com/eth/${ankrKey}`

> 3. Query the mainnet 

Launch the hardhat console on the hardhat network
```bash
npx hardhat console --network hardhat
```

Query the node:
```js
const provider = ethers.getDefaultProvider();
const blockNumber = await this.provider.getBlockNumber();
blockNumber
```

This could have been done using a unit test
```js
import { ethers } from "hardhat";
import { expect } from "chai";

describe("Block", function () {
  before(async function () {
    this.provider = ethers.getDefaultProvider();
  });

  it("should return the latest block number from fork blockchain", async function () {
    const blockNumber = await this.provider.getBlockNumber();
    expect(blockNumber).greaterThanOrEqual(15220409);
  });
});
```
## Solidity

> Write a funcitont at will delete items (one at a time) from a dynamic array without leaving gaps in the array. The order is not important.

As the order is not important, we can save the last index in a variable and remplace it with the index we are trying to remove. Using `pop` on the array we will remove the last index and reduce the size of the array by 1.

Implementation with an array of indexes to remove as input:
```ts
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.8.4;

contract ArrayLib {
    uint[] public arr = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

    function getArr() public view returns (uint[] memory) {
        return arr;
    }

    function removeIndexes(uint[] calldata indexes) public {
        for(uint i = 0; i < indexes.length; i++) {
            uint lastItem = arr[arr.length - 1];
            arr[indexes[i]] = lastItem;
            arr.pop();
        }
    }
}
```

Unit tests:
```ts
  describe("ArrayLib", function () {
    beforeEach(async function () {
      const { arrayLib } = await this.loadFixture(deployArrayLibFixture);
      this.arrayLib = arrayLib;
    });

    it("get the arr", async function () {
      const expectedArr = [];

      for(let i = 0; i < 10; i++) {
        expectedArr.push(BigNumber.from(i));
      }

      const arr = await this.arrayLib.getArr();
      expect(arr).to.eql(expectedArr);
    })

    it("remove last index", async function () {
      const expectedArr = [];

      for(let i = 0; i < 9; i++) {
        expectedArr.push(BigNumber.from(i));
      }

      await this.arrayLib.removeIndexes([9]);
      const arr = await this.arrayLib.getArr();
      expect(arr.length).to.eq(9);
      expect(arr).to.eql(expectedArr);
    })

    it("remove first index", async function () {
      const expectedArr = [];

      for(let i = 1; i < 10; i++) {
        expectedArr.push(BigNumber.from(i));
      }

      await this.arrayLib.removeIndexes([0]);
      const arr = await this.arrayLib.getArr();
      expect(arr.length).to.eq(9);
      expect(arr).to.have.deep.members(expectedArr);
    })

    it("remove 3 indexes", async function () {
      const expectedArr = [];

      for(let i = 1; i < 10; i++) {
        if (![0, 2, 7].includes(i)) {
          expectedArr.push(BigNumber.from(i));
        }
      }

      await this.arrayLib.removeIndexes([0, 2, 7]);
      const arr = await this.arrayLib.getArr();
      expect(arr.length).to.eq(7);
      expect(arr).to.have.deep.members(expectedArr);
    })
  });
});
```