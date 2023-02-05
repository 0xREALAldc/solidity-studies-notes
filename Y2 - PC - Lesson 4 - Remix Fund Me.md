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
		- Address
