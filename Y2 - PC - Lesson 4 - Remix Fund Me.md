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
				- so in calling `msg.value.getConversionRate()` solidity will compile correctly and understand that is the same as calling `.getConversionRate(msg.value)`. The *REASON* for this is that solidity considers the variable calling the function as being the first parameter of the function that will be called, in our example the `.getConversionRate()` 
					- IF we had *TWO OR MORE* parameters declared and expected in the function, let's say the declaration is `.getConversionRate(uint256 ethAmount, uint256 somethingElse)`, we would then have to specify the second parameter in this way `msg.value.getConversionRate(somethingElse)` and solidity would understand the `msg.value` variable as the first parameter and `somethingElse` as the second parameter

- `SafeMath, Overflow Checking and the "unchecked" keyword` 
	- *SafeMath* usued to be all over the place in contracts, but after *solidity version 0.8.* but now it's almost in no contract
	- "Unchecked" : was the concept before solidity 0.8 that if you passed the biggest number that could possibly fit in a variable that you've declared, it would be reseted to 0
		- let's say you declare a *uint8 public bigNumber = 255*, the biggest number would be 255. If you create a function the *add +1* to bigNumber, it would not go to 256 but would sum up to 0.
		- *SafeMath* would enter here in a way that would help us checking before if we hadn't reached the biggest number that would fit in the variable, otherwise our transaction would fail 
		- *in solidity versions 0.8 and over* the functionality *checked* was already implemented so it will check for us before doing anything, if we will not *overflow* or *underflow* a variable. 
			- we can *undo* this change using the syntax below
				- `unchecked {bigNumber = bigNumber + 1; }` 
			- you could think in newer versions why we would use this *unchecked* keyword, well later in the course we will see that the use of the *unchecked* keyword makes your contract a little more gas efficient, so if you're actually sure that your math will never reach the *top or bottom* limits of a number, you can use the keyword to be more gas efficient

- `Basic Solidity For Loop`
	- basically a way to us to loop through a list of something

- `Basic solidity resetting an Array` 
	- instead of looping through an array and delete the objects we're just gonna use as below
		- `funders = new address[](0);` where we completly reset the `funders` array with a brand new `address` array with `(0)` objects, if you change the number between the parentesis, will be the number of objects that the array will have when start

- `Sending ETH from a contract` 
	- we have three ways to do it
	- transfer
		- the simplest and makes most sense to use
		- we need to typecast the *msg.sender* to *payable* to be able to send the tokens, the address needs to be payable
		- if fails, will error and *automatically* revert the transaction
	- send
		- it won't error, will return a boolean if it was *successful or not* 
		- we need to catch the boolean result in a variable and use a *require* to check if it succeeded or not, because if the transaction didn't send the tokens we need to revert the transaction, and the use of *require* will do that
	- call
		- it's a lower level command in solidity
		- it's similar to how we use *send*
		- with *call* we can use virtually any function in all ethereum without even having to have the abi
		- as *call* allows us to call any functions, if the function called returns some data we can store in the *bytes dataReturned* variable. A use of call is exemplified below
			- `(bool callSuccess, bytes memory dataReturned) = payable(msg.sender).callvalue{address(this).balance}("");` 
				- *bool callSuccess* will hold the result if the transaction was successful
				- *bytes memory dataReturned* will hold any return that the function called can have
				- IF you do something like what we want, that is *not call a function* and only use the *call as a transaction* you can leave the space for the variable *dataReturned* blank, only inform the `, (comma)` 

- Basic solidity constructor
	- it's a function that's immediately called after we deploy a contract
	- where we can set up some things we want to be default when the contract is created

- Basic solidity modifiers
	- we can use to make validations before running some other code
	- we use in the function declaration, like `function withdraw() public onlyOwner {..` where *onlyOwner* is the modifier that will do some validations before the function really ran
	- in the modifier we do the validations that we want, and in the last line we type `_;` and that *underscore* is there to mean *run the rest of the code*, that being, the function using the modifier

- `Advanced solidity concepts`
	- `Advanced solidity immutable & constant` 
		- if we have some variable that is assigned a value only once in our contract, we can do some changes so it becomes more gas efficient
		- `constant` 
			- when we set a variable as constant, the variable no longer takes up storage spot 
			- it's much easier to read too
			- naming convention is ALL CAPS 
				- MINIMUM_USD, 
		- `immutable`
			- variables that we assign a value only *ONCE* but not in the same line as we declare them
			- naming convention to a immutable variable is putting `i_` before the variable, like `i_owner` 
	- the reason that *immutable* and *constant* are more gas efficient is because instead of storing them in *storage slot* we store them in the bytecode of the contract

- `Advanced solidity custom errors` 
	- since *solidity 0.8.4* we can save some gas changing the way we make validations with *require* to declaring a *custom error* and doing a *if* statement to validate and call the custom error 
		- `if (msg.sender != i_owner) { revert NotOwner(); } ` the *NotOwner()* we declare before the declaration of the contract as `error NotOwner();`, just this
			- the *revert* does the same as *require*, but without the conditional after

- `Advanced solidity receive & fallback` 
	- what happens if someone sends ETH to a contract without calling a *payable* function? There's two special functions that we can call when this happens
		- *receive* 
			- when there's *no data (no calldata)* linked to the transaction, the *receive* function will be triggered 
			- 
		- *fallback* 
			- whenever *data (calldata)* is sent with a transaction it will trigger *fallback* function
			- 
