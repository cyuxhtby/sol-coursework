# Assembly 1

``` solidity 
// SPDX-License-Identifier: MIT
pragma solidity 0.8.6;

contract Deploy1{

    uint256 value1;

    constructor(){
        value1 = 17;
    }

    function read() view public returns (uint256 result){
        return value1;
    }
}

"object": "608060405234801561001057600080fd5b50601160008190555060b6806100276000396000f
3fe6080604052348015600f57600080fd5b506004361060285760003560e01c806357d
e26a414602d575b600080fd5b60336047565b604051603e9190605d565b60405180910
390f35b60008054905090565b6057816076565b82525050565b6000602082019050607
060008301846050565b92915050565b600081905091905056fea264697066735822122
0872b5d4b9f200afddd5ed3c424f6b3b995bf467e212ec4c313f65365aeadf8e964736
f6c63430008060033",     
"opcodes": "PUSH1 0x80 PUSH1 0x40 MSTORE
CALLVALUE DUP1 ISZERO PUSH2 0x10 JUMPI PUSH1 0x0 DUP1 REVERT JUMPDEST
POP PUSH1 0x11 PUSH1 0x0 DUP2 SWAP1 SSTORE POP PUSH1 0xB6 DUP1 PUSH2
0x27 PUSH1 0x0 CODECOPY PUSH1 0x0 RETURN INVALID PUSH1 0x80 PUSH1 0x40
MSTORE CALLVALUE DUP1 ISZERO PUSH1 0xF JUMPI PUSH1 0x0 DUP1 REVERT
JUMPDEST POP PUSH1 0x4 CALLDATASIZE LT PUSH1 0x28 JUMPI PUSH1 0x0
CALLDATALOAD PUSH1 0xE0 SHR DUP1 PUSH4 0x57DE26A4 EQ PUSH1 0x2D JUMPI
JUMPDEST PUSH1 0x0 DUP1 REVERT JUMPDEST PUSH1 0x33 PUSH1 0x47 JUMP
JUMPDEST PUSH1 0x40 MLOAD PUSH1 0x3E SWAP2 SWAP1 PUSH1 0x5D JUMP
JUMPDEST PUSH1 0x40 MLOAD DUP1 SWAP2 SUB SWAP1 RETURN JUMPDEST PUSH1
0x0 DUP1 SLOAD SWAP1 POP SWAP1 JUMP JUMPDEST PUSH1 0x57 DUP2 PUSH1
0x76 JUMP JUMPDEST DUP3 MSTORE POP POP JUMP JUMPDEST PUSH1 0x0 PUSH1
0x20 DUP3 ADD SWAP1 POP PUSH1 0x70 PUSH1 0x0 DUP4 ADD DUP5 PUSH1 0x50
JUMP JUMPDEST SWAP3 SWAP2 POP POP JUMP JUMPDEST PUSH1 0x0 DUP2 SWAP1
POP SWAP2 SWAP1 POP JUMP INVALID LOG2 PUSH5 0x6970667358 0x22 SLT
KECCAK256 DUP8 0x2B 0x5D 0x4B SWAP16 KECCAK256 EXP REVERT 0xDD 0x5E
0xD3 0xC4 0x24 0xF6 0xB3 0xB9 SWAP6 0xBF CHAINID PUSH31
0x212EC4C313F65365AEADF8E964736F6C634300080600330000000000000000 "
```
### 1. Look at the init code above. When we do the CODECOPY operation, what are we overwriting? 

When we do the `CODECOPY` operation we are essentially overwriting the constructor's initialization code with the runtime code. So in this case, the consrtuctor bytcode is discarded and only the function logic remains in the contract storage onchain.

### 2. Could the answer to Q1 allow an optimisation ? 

Yes, this lets us know that a dense constructor will be costly in deployment while runtime costs will not be affected. This motivates deployers to optimize constructor code as much as possible. Also, since `value1` cannot be updated, it would make sense to make `value1` immutable as this moves `value1`'s data from storage to the bytecode itself, therefore removing the storage write cost of deployment.

### 3. Can you trigger a revert in the init code in Remix?

Some ways to cause a revert in the constructor's initialization
- add a revert()
- add a require() that is not met
- perform an invalid operation

### 4. Write some Yul to add 0x07 to 0x08 and store the result at the next free memory location. Then write it again in opcodes.

[Yul code](./addition.yul)
[Opcodes](./addition_opcodes.txt)

### 5. Can you think of a situation where the opcode EXTCODECOPY is used? 

`EXTCODECOPY` is used to copy the bytecode of an external contract into memory. Some use cases could be:

- cloning contracts to emulate calls
- contract verification

### 6. Complete the assembly exercises in [this repo](https://github.com/ExtropyIO/ExpertSolidityBootcamp/tree/main/exercises/assembly)

[Exercise solutions](./excercises/)