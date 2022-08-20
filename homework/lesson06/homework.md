# Homework
```
1. 
```

```
contract Intro {
    function intro() public pure returns (uint16) {   

        uint256 mol = 420;  
          
        // Yul assembly magic happens within assembly{} section
        assembly {            
            // stack variables are instantiated with 
            // let variable_name := VALUE            

            // instantiate stack variable that holds value of mol            
            let stackVar := mol

            // To return it needs to be stored in memory
            // with command mstore(MEMORY_LOCATION, STACK_VARIABLE)
            mstore(0x0, stackVar)
            
            // to return you need to specify address and the size from the starting point                    
            return(0x0, 32)
        }
    }       
}
```

```
2.
```

```
contract Add {
    function addAssembly(uint x, uint y) public pure returns (uint) {        

        // Intermediate variables can't communicate 
        assembly {            

            let result := add(x, y)
            mstore(0x11, result)        
        }
        // But can be written to memory in one block        
        // and retrieved in another        
        assembly {            
            return(0x11, 32)            
        }
    }
    
    function addSolidity(uint x, uint y) public pure returns (uint) {
        return x + y;
    }
}
```

```
3.
```

```
contract SubOverflow {

    // Modify this function so on overflow it sets value to 0
    function subtract(uint x, uint y) public pure returns (uint) {        

        // Write assembly code that handles overflows        
        assembly {                        
            if lt(x, y) { 
                mstore(0x11, 0)
                return (0x11, 32)
            }
            let result := sub(x, y)
            mstore(0x11, result)
            return (0x11, 32)
        }
    }    
}
```

```
4.
```

```
contract Scope {

    uint public count = 10;
    
    function increment(uint numb) public {        

        // Modify state of the count from within 
        // the assembly segment
        assembly {
            let countTemp := sload(0x0)
            let result := add(countTemp, numb)

            sstore(0x0, countTemp)
        }
    }    
}
```
