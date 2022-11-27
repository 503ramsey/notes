# Chapter 1: Foundations

All solidity source code should start with a "version pragma" — a declaration of the version of the Solidity compiler this code should use. In the code below, any compiler version in the range of 0.5.0 (inclusive) to 0.6.0 (exclusive) can compile the code.

An empty contract named HelloWorld would look like this:
```
pragma solidity >=0.5.0 <0.6.0;

contract HelloWorld {
// This will be stored permanently in the blockchain
// uint's must be non-negative
    uint myUnsignedInteger = 100;
    int mySignedInteger = -69;
}
```

Sometimes you need a more complex data type. For this, Solidity provides structs:
```
struct Person {
  uint age;
  string name;
}
```

You can declare an array as public, and Solidity will automatically create a getter method for it. The syntax looks like:

`Person[] public people;`
Other contracts would then be able to read from, but not write to, this array.

# Question: whats the meaning of "memory" keyword in function inputs?

Note: It's convention (but not required) to start function parameter variable names with an underscore (_) in order to differentiate them from global variables. We'll use that convention throughout our tutorial.

Defining, creating, and adding a zombie to the zombies array
```
pragma solidity >=0.5.0 <0.6.0;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    function createZombie(string memory _name, uint _dna) public {
        zombies.push(Zombie(_name, _dna));
    }

}
```

It's good practice to mark your functions as private by default, and then only make public the functions you want to expose to the world. Denote private functions with a preceding `_` symbol.

```
uint[] numbers;

function _addToArray(uint _number) private {
  numbers.push(_number);
}
```

Solidity contains _view_ functions, meaning they're only _viewing_ state data but not modifying it
Solidity also contains _pure_ functions, which means you're not even accessing any data in the app.

Note: Secure random-number generation in blockchain is a very difficult problem. Our method here is insecure, but since security isn't top priority for our Zombie DNA, it will be good enough for our purposes.

Events are a way for your contract to communicate that something happened on the blockchain to your app front-end, which can be 'listening' for certain events and take action when they happen.

```
pragma solidity >=0.5.0 <0.6.0;

contract ZombieFactory {

    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    function _createZombie(string memory _name, uint _dna) private {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
        emit NewZombie(id, _name, _dna);
    }

    function _generateRandomDna(string memory _str) private view returns (uint) {
        uint rand = uint(keccak256(abi.encodePacked(_str)));
        return rand % dnaModulus;
    }

    function createRandomZombie(string memory _name) public {
        uint randDna = _generateRandomDna(_name);
        _createZombie(_name, randDna);
    }

}

```
# Chapter 2: 

