# Homework
Gas optimization work where the following framework will be applied:
1. target one part
2. perform one optimization
3. run the test to verify that logic is working and analyze the gas impact
4. start from step 2

### Initial gas usage

* addToWhitelist 84894
* transfer 194940
* updatePayment 212354
* whiteTransfer 123301
* Deployment 4653697

### Enable optimizer

#### 200 runs

Changes
```
settings: {
    optimizer: {
    enabled: true,
    runs: 200,
    },
},
```

After changes
* addToWhitelist 83573
* transfer 192840
* updatePayment 209453
* whiteTransfer 121323
* Deployment 2747221

### Change into storage var

#### Remove the Constant contract
 
Changes
```
contract GasContract is Ownable {
    uint256 public constant tradeFlag = 1;
    uint256 public constant basicFlag = 0;
    uint256 public constant dividendFlag = 1;
```

After changes
* addToWhitelist 83574
* transfer 192846
* updatePayment 207231
* whiteTransfer 121323
* Deployment 2675033

#### Use bool instead of uint256

Changes
```
bool public constant tradeFlag = true;
bool public constant basicFlag = false;
bool public constant dividendFlag = true;
```

After changes
* addToWhitelist 83574
* transfer 192846
* updatePayment 207231
* whiteTransfer 121319
* Deployment 2675033

#### Clean up storage var
```
removed basicFlag and defaultPayment as never used
```

After changes
* addToWhitelist 83574
* transfer 192912
* updatePayment 207209
* whiteTransfer 121323
* Deployment 2666577

### Modifers

#### Optimize onlyAdminOrOwner
```
modifier onlyAdminOrOwner() {
    require(checkForAdmin(msg.sender) || msg.sender == contractOwner, "Error 1");
    _;
}
```

After changes
* addToWhitelist 82017
* transfer 192912
* updatePayment 205570
* whiteTransfer 121319
* Deployment 2404270

#### Optimize/Remove checkIfWhiteListed
After changes
* addToWhitelist 82017
* transfer 192912
* updatePayment 205570
* whiteTransfer 121305
* Deployment 2303099

### Constructor

#### Fixed size admin array
After changes
* addToWhitelist 82017
* transfer 192912
* updatePayment 205570
* whiteTransfer 121309
* Deployment 2300321

#### Removed the loop
```
constructor(address[5] memory _admins, uint256 _totalSupply) {
    contractOwner = msg.sender;
    totalSupply = _totalSupply;
    administrators = _admins;
    balances[contractOwner] = totalSupply;
    emit supplyChanged(contractOwner, totalSupply);
}
```

After changes
* addToWhitelist 82017
* transfer 192912
* updatePayment 205570
* whiteTransfer 121309
* Deployment 2272664

### getTradingMode

#### Refactor
```
function getTradingMode() public view returns (bool mode_) {
    return tradeFlag || dividendFlag;
}
```
No impact on gas

### History

Several optimization possible:
* making it private
* not returning (as never used)
* emit events instead of saving into array
* Remove the logic involving the for loop on tradePercent

Chosed to completely removed it the data stored is `blockNumber`, `lastUpdate`, `updatedBy`.

After changes
* addToWhitelist 81995
* transfer 192823
* updatePayment 110703
* whiteTransfer 121353
* Deployment 1976989

### transfer

#### Remove the status logic (useless?)
After changes
* addToWhitelist 81995
* transfer 186606
* updatePayment 110703
* whiteTransfer 121353
* Deployment 1928226

#### Not set 0 value
After changes
* addToWhitelist 81995
* transfer 186565
* updatePayment 110703
* whiteTransfer 121353
* Deployment 1924566

### Remove long require messages
After changes
* addToWhitelist 81995
* transfer 186565
* updatePayment 110703
* whiteTransfer 121353
* Deployment 1884108

### updatePayment

#### Remove long require messages
After changes
* addToWhitelist 81995
* transfer 186565
* updatePayment 110703
* whiteTransfer 121353
* Deployment 1831457

#### useless check of tradingMode
After changes
* addToWhitelist 81995
* transfer 186565
* updatePayment 110663
* whiteTransfer 121349
* Deployment 1828649

### addToWhitelist

#### Remove long require messages
After changes
* addToWhitelist 81995
* transfer 186565
* updatePayment 110663
* whiteTransfer 121353
* Deployment 1809621

#### Remove weird logic and only accepting tier <= 3
After changes
* addToWhitelist 80684
* transfer 186565
* updatePayment 110663
* whiteTransfer 121349
* Deployment 1751774

#### Remove the odd logic (useless?)
After changes
* addToWhitelist 57238
* transfer 186565
* updatePayment 110663
* whiteTransfer 121353
* Deployment 1676184

#### Change tier to uint8
After changes
* addToWhitelist 57238
* transfer 186565
* updatePayment 110663
* whiteTransfer 121353
* Deployment 1676184

cost more gas, reverting. Reason being that EVM has to do more operation to downscale from 256 to 8bits: https://ethereum.stackexchange.com/questions/3067/why-does-uint8-cost-more-gas-than-uint256

### whiteTransfer

#### Remove long require messages
After changes
* addToWhitelist 57238
* transfer 186565
* updatePayment 110663
* whiteTransfer 121353
* Deployment 1676196

#### balances calculation
After changes
* addToWhitelist 57238
* transfer 186565
* updatePayment 110663
* whiteTransfer 120747
* Deployment 1630795

#### init struct in one go
After changes
* addToWhitelist 57238
* transfer 186565
* updatePayment 110663
* whiteTransfer 120682
* Deployment 1614779

#### ImportantStruct to calldata
After changes
* addToWhitelist 57238
* transfer 186565
* updatePayment 110663
* whiteTransfer 120682
* Deployment 1626456

#### Use usersTier mem variable
After changes
* addToWhitelist 57238
* transfer 186565
* updatePayment 110663
* whiteTransfer 120205
* Deployment 1604200

### Whitelist

#### Implement a Merkle tree

Implemented a Merkle Tree for each tier stored in a map (root => tier).

After changes
* addToWhitelist 57275
* transfer 186654
* updatePayment 110619
* whiteTransfer 125721
* Deployment 1730742

More gas coslty. Wondering if hashing the address and the tier could help. But in this case, we won't be able to have the tier on chain and it will have to be passed as input.

On the other hand, the number of tx dropped from 2400 to 12. Which is gas saving overall.

### Misc

#### remove all long require
* addToWhitelist 57275
* transfer 186654
* updatePayment 110619
* whiteTransfer 125733
* Deployment 1713232

#### Move at the top the most used functions
Negative impact - reverting

#### private var
* addToWhitelist 57251
* transfer 186565
* updatePayment 110597
* whiteTransfer 125713
* Deployment 1541199

#### Stop using Ownable
* addToWhitelist 57275
* transfer 186609
* updatePayment 110574
* whiteTransfer 125706
* Deployment 1417641

#### Pack better storage var
* addToWhitelist 57273
* transfer 186609
* updatePayment 110574
* whiteTransfer 125694
* Deployment 1417607

#### Removing bool trading flags
* addToWhitelist 57273
* transfer 186606
* updatePayment 110623
* whiteTransfer 125702
* Deployment 1420518

for some reason, it is increasing deployment gas...

#### removing tradePercent var
* addToWhitelist 57275
* transfer 186606
* updatePayment 110620
* whiteTransfer 125702
* Deployment 1397864

#### Stop init of storage var to 0
* addToWhitelist 57275
* transfer 186624
* updatePayment 110623
* whiteTransfer 125683
* Deployment 1394682

#### Return named var
* addToWhitelist 57270
* transfer 186624
* updatePayment 110618
* whiteTransfer 125672
* Deployment 1393794

#### Pack ImportantStruct into bytes32
```
struct ImportantStruct {
    uint256 valueA; // max 3 digits
    uint256 bigValue;
    uint256 valueB; // max 3 digits
}
```

Looking at the value passed in the test we can pack them into a bytes32

* addToWhitelist 57268
* transfer 186646
* updatePayment 110640
* whiteTransfer 81075
* Deployment 1379093

#### Custom Error + bump to 0.8.4
* addToWhitelist 57242
* transfer 186535
* updatePayment 110595
* whiteTransfer 80950
* Deployment 1227138

#### Remove the memory var of msg.sender
* addToWhitelist 57242
* transfer 186495
* updatePayment 110590
* whiteTransfer 80894
* Deployment 1217177

#### Removed optional checks
* addToWhitelist 57238
* transfer 186495
* updatePayment 110590
* whiteTransfer 80898
* Deployment 1209635

#### Payments: changes the Enum and declaration
* addToWhitelist 57242
* transfer 186407
* updatePayment 107671
* whiteTransfer 80891
* Deployment 1173926

## Final proposal

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.4;

import "./MerkleProof.sol";

error Unauthorized();

contract GasContract {
    uint256 public totalSupply;
    uint256 private paymentCounter;
    address private contractOwner;
    mapping(address => uint256) private balances;
    mapping(address => Payment[]) private payments;
    mapping(bytes32 => uint256) private whiteListMap;
    mapping(address => bytes32) private whiteListStruct;
    address[5] public administrators;

    struct Payment {
        uint256 paymentType;
        uint256 paymentID;
        bool adminUpdated;
        string recipientName; // max 8 characters
        address recipient;
        address admin; // administrators address
        uint256 amount;
    }

    event WhiteListRootUpdated(bytes32 root, uint256 tier);

    modifier onlyAdminOrOwner() {
        if (checkForAdmin(msg.sender) || msg.sender == contractOwner) {
            _;
        } else {
            revert Unauthorized();
        }
    }

    event SupplyChanged(address indexed, uint256 indexed);
    event Transfer(address recipient, uint256 amount);
    event PaymentUpdated(
        address admin,
        uint256 ID,
        uint256 amount,
        string recipient
    );
    event WhiteListTransfer(address indexed);

    constructor(address[5] memory _admins, uint256 _totalSupply) {
        contractOwner = msg.sender;
        totalSupply = _totalSupply;
        administrators = _admins;
        balances[contractOwner] = totalSupply;
        emit SupplyChanged(contractOwner, totalSupply);
    }

    function checkForAdmin(address _user) public view returns (bool admin) {
        admin = false;
        for (uint256 i = 0; i < administrators.length; i++) {
            if (administrators[i] == _user) {
                admin = true;
            }
        }
    }

    function balanceOf(address _user) public view returns (uint256 balance) {
        balance = balances[_user];
    }

    function getTradingMode() public pure returns (bool) {
        return true;
    }

    function getPayments(address _user)
        public
        view
        returns (Payment[] memory payments_)
    {
        payments_ = payments[_user];
    }

    function transfer(
        address _recipient,
        uint256 _amount,
        string calldata _name
    ) public {
        if (balances[msg.sender] < _amount || bytes(_name).length >= 9) {
            revert Unauthorized();
        }
        balances[msg.sender] -= _amount;
        balances[_recipient] += _amount;

        payments[msg.sender].push(Payment(
            3,
            ++paymentCounter,
            false,
            _name,
            _recipient,
            address(0),
            _amount));
        emit Transfer(_recipient, _amount);
    }

    function updatePayment(
        address _user,
        uint256 _ID,
        uint256 _amount,
        uint256 _type
    ) public onlyAdminOrOwner {
        if (_ID == 0 || _amount == 0) {
            revert Unauthorized();
        }

        for (uint256 i = 0; i < payments[_user].length; i++) {
            if (payments[_user][i].paymentID == _ID) {
                payments[_user][i].adminUpdated = true;
                payments[_user][i].admin = _user;
                payments[_user][i].paymentType = _type;
                payments[_user][i].amount = _amount;
                emit PaymentUpdated(
                    msg.sender,
                    _ID,
                    _amount,
                    payments[_user][i].recipientName
                );
            }
        }
    }

    function addToWhitelist(bytes32 _root, uint256 _tier)
        public
        onlyAdminOrOwner
    {
        whiteListMap[_root] = _tier;
        emit WhiteListRootUpdated(_root, _tier);
    }

    function whiteTransfer(
        address _recipient,
        uint256 _amount,
        bytes32[] calldata proof,
        bytes32 leaf,
        bytes32 _struct
    ) public {
        uint256 userTier = whiteList(proof, leaf);
        if (
            keccak256(abi.encodePacked(msg.sender)) != leaf || 
            userTier == 0 ||
            balances[msg.sender] < _amount ||
            _amount <= 3) {
            revert Unauthorized();
        }

        balances[msg.sender] -= _amount - userTier;
        balances[_recipient] += _amount - userTier;

        whiteListStruct[msg.sender] = _struct;
        emit WhiteListTransfer(_recipient);
    }

    function whiteList(bytes32[] calldata proof, bytes32 leaf) public view returns (uint256 userTier) {
        bytes32 calculatedRoot = MerkleProof.processProof(proof, leaf);
        userTier = whiteListMap[calculatedRoot];
    }
}
```