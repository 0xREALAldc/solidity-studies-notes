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
- Good practice in naming events is to name it with the function name reversed, for example, our function is *enterRaffle* and our event will be named as *RaffleEnter*

`Implementing Chainlink VRF (Randomness in Web3)` 
-  Chainlink VRF is a 2 transaction process
	- this is because it makes it hard for anyone to try and brute force to guess the winner number
	- we'll request the random number in *one function* and get the return of this call *in a second function*
- we'll need to install the package that contains the *chainlink smart contracts* so we're able to import and use in our contract
	- `yarn add --dev @chainlink/contracts` 
- we have added a function called *function fullfillRandomWords() internal override {}* that is a function from *chainlink contracts* that in there is declared as *virtual*, with this meaning that we're supposed to implement it. 
	- this is why we declared our *fullfillRandomWords* function with the word *override*, to override the function in the parent contract with our code
- `Implementing the request` 
	- we'll need to do the call below informing the parameters described 
		- *i_keyHash* it's the gasLane hash that chainkink provides for us, which specifies the maximum gas price to bump to
		- *i_subscriptionId* the subscription ID from chainlink VRF
		- *REQUEST_CONFIRMATIONS* the number of confirmations needed 
		- *i_callbackGasLimit* depends on the number of requested values that you want sent to the *fulfillRandomWords()* function. Storing each word costs about 20k gas
		- *NUM_WORDS* is the number of random numbers that we want
	- the call to *.requestRandomWords* returns a *uint256 request ID* that defines who's requesting this data
```solidity
		i_vrfCoordinator.requestRandomWords(
			i_keyHash, //gasLane
			i_subscriptionId,
			REQUEST_CONFIRMATIONS,
			i_callbackGasLimit,
			NUM_WORDS
		);
```
`Implementing the Fulfill`
- were going to use the *module ( a % b)* function to pick a random winner. This is because were going to get the random number that we receive from Chainlink VRF and take the MOD of this number over the number of players in our lottery.
	- EG: 202 (random number) % 10 (number of players in the lottery) = 2 . The number 2 is the index of the player who will be the winner of our lottery.


`Introduction to Chainlink Automation` 
- enables the ability to set a contract to have a function being performed based in a off chain 
reason, or you can just make it to execute something after it passes 30 seconds or so

- `Implementing Chainlink Automation - checkUpkeep`
	- *checkUpkeep* is the function that the Chainlink node will call in your contract to check if the condition that we've defined is met and we need to run the *performUpkeep* method 
	- it takes a parameter called *checkData* that allows us to specify anything that we want when we call the checkUpkeep function, even another function we can pass it as parameter because of the type *bytes*. 
	- as return we have the *upkeepNeeded* that tells the Chainlink node if the condition was met and it needs to call the *performUpkeep*
	- also returns the *bytes memory performData* that we can specify something that the node can do
- `Enums` 
	- we can use to create custom types with some *constant values* to use as types 
		- EG: `enum State { Created, Locked, Inactive }` 

- `Implementing Chainlink Automation - performUpkeep` 
	- this function will be executed when the *checkUpkeep* returns *true* to the chainlink node
	- 








`Hardhat Shorthand`
- hardhat has a *shorthand (hh) and autocomplete* package to make it easier to us to call the hardhat commands in the command line
- we can install the package with the command below
	- `npm i -g hardhat-shorthand`
- now instead of needing to run `yarn hardhat compile` we can only run `hh compile` 




- Repository with the code developed 
	- https://github.com/0xREALAldc/Y2-PC-lesson-9-hardhat-smartcontract-lottery-
