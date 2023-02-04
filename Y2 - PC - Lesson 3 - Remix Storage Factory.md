- `factory contract` : is a contract that's able to deploy and interect with another contracts.
	- we can think of a `factory` like a manager of all the contracts that it creates and interacts

- `composability` : is the hability of contracts seamlessly interact with each other. Smart contracts are `composable` because they can easily interact with each other, something very used in DeFi

- A single file of `solidity` can hold more than *one* smart contract

- `how to use a contract inside another contract` : 
	- `import` : it takes the `path`, `package` or `githib` of another file to import all of it inside the file that you're working now
		- `import "./SimpleStorage.sol"; ` : this will let us be able to access the `SimpleStorage` contract from inside another contract, in this case the `StorageFactory.sol`
		
- `how to deploy a new contract inside another contract` : declaring a variable of the type of the contract that's going to be deployed, and after using the keyword `new` with the name of the contract assigning the result to the variable that we've just created, like below
	- `SimpleStorage public simpleStorage` : first we declare the variable 
	- `simpleStorage = new SimpleStorage();` : then we use the keyword `new` to instantiate the new contract

- `interacting with other contracts` : you're always gonna need *two* things
	- *Address* : of the contract
	- *ABI (Application Binary Interface)* : will tell our code exactly how it can interact with the contract 
		- in Remix, after you *compile* your contract you can go in the option *Compilation Details*  where there's a whole bunch of informations, as well as the *ABI* for the deployed contract
		- when we `import ...` a file, we automatically get the ABI to use, but there are other ways to actually get the ABI too

- `Inheritance`
	- this is what we do when a contract becomes a *child* of another contract, where the child contract will inherit all the functionalities from the contract it inheriting
	- we will need to do two things for this
		- import the contract that will be inherited
		- do the inheritance in the child contract
			- `contract ChildContract is NameOfInheritedContract...`
 - `Overrides`
	 - for us to be able to override a function in solidity, we need to use two keywords, that are
		 - *virtual* for a function to be able to be *overrided* in a child contract, we need to inform in that function the keyword *virtual*
		 - *override* when we're going to implement in a child contract a function that already exists in the inherited contract, we need to use the word *override* in the declaration of the function
