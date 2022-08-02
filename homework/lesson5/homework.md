# Homework
```
1. 
```
CODECOPY is overritting the free memory pointer (pushed into the 0th space of the memory).

```
2.
```

An optimization could be to not set the pointer as it is going to be erased in any case.

```
3.
```
Adding a revert in the constructor.

Or access a property (like a function) in the constructor. As the contract is not yet deployed, it does not make any sense to access it during the init phase (constructor).

```
4.
```
```
Verify that a contract is matching the type of code we are expecting (code can be trusted)