- `Sending ETH Through a function & Reverts`
	- `payable` functions are the ones that you can send *ETH* in them, in remix they appear as *red buttons*
	- contract addresses can hold funds just like any wallet can
	- `msg.value` is how you can know how much ETH someone has sent to a *payable* function
- `require` 
	- it's a manner that we have to make some validations in solidity, if the condition it's not met it will *revert* with an *error message* 
	- `revert` 
		- undo any action before, and send remaining gas back

- `Chainlink & Oracles`
	- chainlink is a oracle that's how we can get decentralized data from the real world into blockchain with security and trust

- `External contract`
	- to interact with a external contract we need two things
		- ABI
			- in the example with our contract `SimpleStorage` we imported the contract to get access to the ABI, but for a *data feed* it's a lot of code, so we can do in other way
				- `Interface` 
					- It's a contract that has declared all the methods that the contract have but none of the logic, so we know *how to and what to* call
					- if we declare this in a file in *OUR* project and compile, we will get the ABI OR we can import this directly from `github or NPM package` 
						- `importing from GitHub & NPM package` 
							- *npm* is a package manager that can keep different versions of contracts that we can import into our contract
							- using this kind of importing makes it easier for us and we don't have the necessity to copy and paste the Interface of the contract we want from github and compile locally
		- Address

- `calling functions with many variabled being returned` 
	- we'll use a function called *.latestRoundData* in our contract that is from chainlink v3 interface, and it has more than one return parameter, as it shows below
		- `(uint80 roundID, int256 price, uint256 startedAt, uint256 timeStamp, uint80 answeredInRound) = priceFeed.latestRoundData();`
	- since we just care about the `price` return, we can remove the other return variables only leaving the `,(comma)` to represent the others, and in this way get only the return we need.
		- `(,int256 price,,,) = priceFeed.latestRoundData();` 

- `Floating point math in solidity` 
	- one thing that we have to have in mind when comparing values is that `msg.value` has 18 decimals and the `price` that we get from the *data feed* has only 8 decimals, so before doing any comparison we need to get them to be equal, so we have to add 10 decimals to the `price` variable, as below
		- `price * 1e10 (equal to 1**10, that is elevated 10 times)` 
	- also the `msg.value` is a uint256 and `price` is a `int256`, so we can convert the `price` to be equal type of `msg.value` 
		- `typecasting` 
			- it's how we can convert a type of a variable, using the syntax `uint256(variable)` for example
				- `uint256(price)` 
			- you *CAN'T* typecast any variable, but there's some variables that we can do easily as this above
	- *ALWAYS* multiply, sum or decrease before doing a *DIVIDE* operations in solidity

- `Basic Solidity Arrays & Structs II`
	- 

- `msg.value` : global variable that tells us how much ETH or the native blockchain token was sent
- `msg.sender` : global variable that tells us the address of *WHO* sent the transaction 

- `Libraries` 
	- libraries are similar to contracts, but you *CAN'T* declare any state variable and you *CAN'T* sen ether.
	- *INSTEAD* of using `contract` to declare in the beginning of the file, we will use the keyword `library` 
	- *ALL* the functions in a library are going to be *INTERNAL* 
		- to remember, internal functions are the ones that can only be called *internally within the contract* that they're declared or *by the contracts that inherits* from them
	- `use a library for a type` 
		- we can use a library that we develop to kind like *attach* to a type, so we can call our functions as if it we're native to that variable type, example below
			- `msg.value` is a *uint256*
			- if we use the line `using PriceConverter for uint256`, remembering that *PriceConverter* is the name of the library that you're goint to create, will make us have the ability to call the functions declared in *PriceConverter* from variables that are *uint256*
				- `msg.value.getConversionRate()` this one for example, in the native *uint256* we don't have a function named *.getConversionRate*, but in out library we do
			- one thing to notice is that, in our function we declare that we receive a parameter in the function `.getConversionRate(uint256 ethAmount)`. But when we use the function *FROM* the variable that was going to be passed as parameter, we don't need to send as parameter because solidity already understands that should use the value from the variable that's calling the funcion in the function
				- so in calling `msg.value.getConversionRate()` solidity will compile correctly and understand that is the same as calling `.getConversionRate(msg.value)` 
