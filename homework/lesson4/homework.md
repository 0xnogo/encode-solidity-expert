# Homework
```
1. 
```
`tx.origin` is the address where the contract was originated (EOA).

Alice -> A -> B. In B, msg.sender = A and tx.origin = Alice

`tx.origin` can be vulnerable to phishing attack. If for example we have a check on
`tx.origin == Alice`, then we can trick Alice to call a contract C that will call A. The check will pass as `tx.origin` will be equal to Alice's address.

It's also worth mentioning that by using tx.origin you're limiting interoperability between contracts because the contract that uses tx.origin cannot be used by another contract as a contract can't be the tx.origin.

To check if `tx.origin == msg.sender` we are in fact checking that the call to our contract was made by EOA directly and not a contract.

```
2.
```

Event spoofing? Spoofing in general means to pretend to be someone to win a person's trust. 

Around project like the Graph (takes events and put them in DB and query...). But it is relying upon data received. We could imagine the situation that a malicious person create a token and use that contract that would send out more event or change details in the events arguments. Like Vitalik address has bought some of that token and be fooled by that (and buy it)

```
3.
```
The EVM is determinitic making true randomness impossible.

```
contract Example1{

    uint256 private rand1 =  block.timestamp;

    function random(uint Max) view public returns (uint256 result){

        uint256 rand2 = Max / rand1;
        return rand2 % Max ;
    }
}
```

In this contract, we are using the `block.timestamp` as random factor. This is easily attackable as anyone could implement the same function and use it to get the same random number. For example, if this is used for a lottery, it will be possible to get exactly the right number and win the prize.

Also if rand1 is big enough, we might get rand2 = 0.

Solution: use an oracle

```
4.
```
```
contract Course {
    // In this contract the students add themselves via the joinCourse function.
    // At a later time the teacher will via a front end call the welcomeStudents function
    // to send a message to the students and get the number of students starting the course.
    address[] students;
    address teacher = 0x94603d2C456087b6476920Ef45aD1841DF940475;

    event welcome(string,address);
    uint startingNumber = 0;

    function joinCourse()public{
        students.push(msg.sender);
    }

    function welcomeStudents() public{
        require(msg.sender==teacher,"Only the teacher can call this function");
        for(uint x; x < students.length; x++) {
        emit welcome("Welcome to the course",students[x]);
        startingNumber++;
        }
    }
}
```

First, the `joinCourse()` is public, meaining every one can add be part of the course.

Also, this code is vulnerable to DoS attack. As anyone could add an entry in the students array, `welcomeStudents()` might be very expensive to call due to the for loop.

Solution: 
* Emit the Welcome event in the `joinCourse` and use it in the front end to get the list of all the students
* Make the `joinCourse` only accessible by the admin/whitelist