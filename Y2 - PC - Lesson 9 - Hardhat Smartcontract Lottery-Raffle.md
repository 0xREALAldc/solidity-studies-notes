`hardhat Setup`
- `yarn add --dev hardhat` to set up our project
- `yarn hardhat` to start our new project 
	- and select *create an empty hardhat.config.js* 
- `dependencies` then well install all the dependencies at once
	- `yarn add --dev @nomiclabs/hardhat-ethers@npm:hardhat-deploy-ethers ethers @nomiclabs/hardhat-etherscan @nomiclabs/hardhat-waffle chai ethereum-waffle hardhat hardhat-contract-sizer hardhat-deploy hardhat-gas-reporter prettier prettier-plugin-solidity solhint solidity-coverage dotenv` 


- `Raffle.sol - Setup` 
	- what we're going to be able to do with this contract?
		- Enter the lottery (paying some amount)
		- Pick a random winner (verifiably random)
		- Winner to be selected every X minutes -> completly automated
		- Use Chainlink Oracle -> Randomness from out of blockchain, Automated Execution (Chainlink Automation (former Keepers))

`Introduction to Events` 
- `EVM can emit logs` 
	- when things happen in a blockchain, the EVM writes this changes in logs
	- we can read this logs from a blockchain node that we run 
	- if you run a local node or connect to one, you can run a method called *eth_getLogs* to get these logs
- `Events` 
	- They allow us to *print* informations to this *log structure* in a way that is more gass efficient than saving in somewhere like storage variables
	- These *events* and *logs* live in this space that isn't accesible to smart contracts]
		- this is why it's cheaper to read from there, because smart contracts cannot access them
	- Each event is *tied* to the smart contract that emitted the event
		- we can listen to these events being emitted to like, *do something* everytime someone calls a *transfer* function or something else
		- instead of needing to keep watching a variable change it's value or something like this, all we need to do is tell our application to *listen to this event* 
	- An event looks like the example below:
		- *indexed* this keyword is very important because this tells that a parameters is INDEXED. 
			- when we emit a event, there are two types of parameters, a *indexed* and the *non-indexed* parameters 
			- we can have up to *THREE* indexed parameters in a event
			- *indexed parameters* are also called *Topics*
			- *indexed parameters* are parameters that are much easier to search for
	```solidity
		event storedNumber(
			uint256 indexed oldNumber,
			uint256 indexed newNumber,
			uint256 addedNumber,
			address sender
		);
	```
	- when we emit a event, it'll be like the example below
		- looks very similar to calling a function
		- looking in Etherscan, in a transaction we can see the events emitted in the *Logs* section
			- First we'll see the *Address* property
			- then the *Name* that is the declaration of the event
			- after it comes *Topics* that is the *indexed parameters* 
			- and in *Data* are the *non-indexed parameters* of the event
	```solidity
		emit storedNumber(
			favoriteNumber,
			_favoriteNumber,
			_favoriteNumber + favoriteNumber,
			msg.sender
		);
	```
- I got the above example deployed at `0x89E2F6A5b45A878Dec5d1d1a26bE70A68661f64e` in Goerli
	- also the code available in the repo 
		- https://github.com/0xREALAldc/Y2-PC-extra-lesson-hardhat-events-logs-

`Events in Raffle.sol` 



- Repository with the code developed 
	- https://github.com/0xREALAldc/Y2-PC-lesson-9-hardhat-smartcontract-lottery-
