- `SPDX-License-Identifier: MIT` : this line is always at the top of you're solidity files and just state the license type for your code, it's not obligatory but some compilers will flag you with warnings if you don't use
- at the top of a file in solidity, we always have to state what version of it we're using
	- `pragma solidity <version>`  eg : `pragma solidity 0.8.7` 
	- `pragma solidity ^0.8.7` : writing this way using the `^` before the version, you're stating that you're fine using version 0.8.7 and newer versions
	- `pragma solidity >=0.8.7 <0.9.0` : this is how we could use to say that our contract can run in this range of versions for the compiler
- `contract` you can think of something like an class or object in java or javascript

- BASIC TYPES:
	- `boolean` : true of false 
		- eg: `bool hasFavoriteNumber = true;` 
	- `uint` : *unsigned* integer, so it's only a positive whole integer. 
		- We can also say how many bits we want to allocate in this integer, and basically it's how many storage or memory, how big this integer can be, we can go as `uint8` all the way up to `uint256`. Default it's `uint256` .  
		- It's a good practice in solidity to be very specific, so normally we say how many bits, usually `uint256` even if it's default
		- eg: `uint favoriteNumber = 123;`
	- `int` : *positive* or *negative* whole number
		- we can also set the bits for the int, default `int256` 
		- `int256 favoriteInt = -5` or `int256 favoriteInt = 10` 
	- `string favoriteNumberInText = "Five";` 
		- *strings* are secreatly *bytes*, but only for text
	- `address`: it's a address, like a from a *wallet* or *contract*
		- `address myAddress = 0x646509E1b4Df6563848FcdBe0444f96aCc35f534;`
	- `bytes` : variable representing how many bytes we want it to be, being `bytes32` the biggest
		- eg: `bytes32 favoriteBytes = "cat";` 
		- byte objects typically looks like something like `0x1233124asdfagaf` 
	- IF you declare a variable and don't assign a value, it'll be assigned their default value from their type
		- `uint256 favoriteNumber` : will be the equal to NULL that in solidity it's 0


- VISIBILITY SPECIFIERS:
	- `public` : visible externally and internally (creates a getter function for storage/state variables). Anybody who interacts or sees the contract can see what is stored in the variable
	- `private` : only visible in the current contract, for functions means that only this contract can call that function
	- `external` : only visible externally (only for functions). Somebody outside the actual contract can call the functions of another contract that are `external`. can only be message-called (via `this.func`)
	- `internal` : only visible internally, by the contract of the function and it's children contracts. 
	- `uint favoriteNumber` : when we declare a variable in this way, only setting the type and name the visibility it's default `internal`, so you won't be able to see it's value from outside the contract
	
- SCOPE : the *curly brackets {...}* are what determine the scope 
	- `global`: everything declared inside the `contract NameOfContract {...}` can be access the variable that's declared outside a function
	- `inside a function`: variables declared inside a function will only be visible inside that function

- BASIC SOLIDITY FUNCTIONS
	- `view` : when called, it'll cost 0 
		- when we use `view` we mean that were just going to read something from the blockchain, it'll never change state in the blockchain
	- `pure` : when called, it'll cost 0 gas.
		- just like `view functions` the `pure` also disallows any modification of state but they also disallow READING from the blockchain state,  so you may use a `pure` function to something that you're going to use inside your contract, like some math calculation or something
		- eg: `function add() public pure returns (uint256) { return (1 + 1); }`

- ARRAYS: a way to create a list or sequence of objects
	- `People[] public people;`
	- `uint256[] public favoriteNumbersList` 
	- `uint256[]` : this is known as a *dynamic array* because it's size is unkown
	- `uint256[3]` : this is a array with 3 of length

- STRUCTS : we create a new type that's called `struct` where we declare a object with properties inside
	- `struct People {uint256 favoriteNumber; string name; }` 

- EVM Overview : There's six places where you can storage data in solidity. Even though we have six places that we can access and store information, we *CANNOT* say that a variable it's `stack`, `code` or `logs`. Variable are only defined as `memory`, `storage` or `calldata` . 
	1. `Memory` : mean that the variable is only going to exist temporarily that *can* be modified. 
	2. `Storage` : exists even outside the function that's using it . When a variable it's not explicitly defined as other type, it is automatically `storage` . It's permanent variables that *can* be modified 
	3. `Calldata` : mean that the variable is only going to exist temporarily that *can't* be modified . You can use `calldata` to a parameter that you're not going to change afterwards 
	4. `Code` 
	5. `Stack`
	6. `Logs` 
	- There's some types that are *NEEDED* to be given the `memory` or `calldata` type, and they are `structs`, `mappings` and `arrays` when adding them as parameters to different functions.
	- function parameters can't be `storage`  variables because they only exist 

- MAPPINGS : it's a `key -> value` type of object. We declare one variable of `mapping` type as below
	- `mapping(string => uint256) public nameToFavoriteNumber` : now we bave a dictionary that everysingle name will map to a specific number

- EVM - *Ethereum Virtual Machine* : blockchains that are compatible with *ethereum* blockchain, some of them are: `Avalanche`, `Fantom` and `Polygon` 

Git repository for the contract developed: https://github.com/0xREALAldc/Y2-PC-lesson-2-simple-storage-
