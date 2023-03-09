`Connecting the browser to Metamask and our Contracts`
- when we have the extensions for wallets installed, we get the possibility to connect a wallet to our website
- this is possible because when the *extensions* for the wallets are installed, they get automatically injected into a *window object* of JavaScript
- this *window* object represents our window in the browser, and it has a bunch of objects and properties inside it 
- when we have the *metamask or a metamask like brower* in the object *window* we'll have available the object *ethereum*, being able to access it using *window.ethereum*
- if we don't have a extension like *metamask* installed, we won't have access to interactions on the blockchain. 
- the importance of this wallets are so importante is that they have built in them a *blockchain node* connected to them, and to interact with the blockchain we always need to have a node

`Connecting HTML to Metamask`
- to connect to the metamask we can use the *eth_requestAccounts* and automatically pops up a window showing all your wallets available to connect. We'll leave below a example of how to call the method
```html
	<script>
		if (typeof window.ethereum !== 'undefined') {
			window.ethereum.request({ method: 'eth_requestAccounts' })
		}
	</script>
```

`Interacting with the blockchain`
- to send a transaction, the things that we *ABSOLUTELY* need are
	- *provider / connection to the blockchain*
		- to get our provider we're going to use *ethers* but in this example as we only going to have the frontend of our application, we'll be grabing *ethers* in a different way using  the CDN to copy the ethers library compiled and server in our local project
	- *signer / wallet / someone with some gas*
	- *contract that we are interacting with*
		- *ABI*
		- *address* 

`Sending a Transaction from a Website`
- *web3Provider* is an object in ethers that allows us to wrap around a existing web3-compatible, like a web3HttpProvider or web3WsProvider or metamask, and expose it as an ethers.js Provider. 
	- it's similar to the JsonRpcProvider that we used before where we put in our Alchemy url endpoint, and when using metamask it'll take whatever endpoint that is configured in our network on metamask and use it
	- the *Web3Provider* automatically stick this endpoint in the ethers provider so we can use it
	- we show in the code below the declaration of the provider
	```javascript
		// find the http endpoint in the metamask and make the one that we use
		// to send our transactions
		const provider = new ethers.providers.Web3Provider(window.ethereum)
```
- Now we need a wallet (signer) and we can grab one using the following code
```javascript
		// this line will return the wallet that's connected to the provider (metamask)
		const signer = provider.getSigner()
```
- Now we declare our contract. 
	- as we need our Abi and contract address, we can spin up locally a node using *yarn hardhat node* and get the contract address
	- the ABI we can go in our previous project and grab the ABI in *Artifacts/contracts/FundMe.sol/FundMe.json* and copy the section *abi* 
	- then after we can create a *constants.js* file and declare both the *contractAddres* and *abi* in there as constants and export them as below, to after import them in the *index.js*
	```javascript 
	export const contractAddress = "0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512"
	export const abi = ....
	```
	- then back in our *index.js* we can import them and declare the contract using the code below
	```javascript
		import { abi, contractAddress } from './constants.js'
		...
		// now we need our contract and ABI
		const contract = new ethers.Contract(contractAddress, abi, signer)
	```

`Reseting metamask account` 
- we will need to do this a lot because when we start a local node and do transactions on it, it'll save in the metamask the *nonce* for the account, and when we stop and run again our local node, it's going to give us an error
- we need to *reset* our metamask account to solve this, or use the *custom nonce* for the account and this automatically fix this 
- to reset the account we'll go in our metamask, click in the *My Accounts* in metamask
	- go to *Settings* and then *Advanced* and then *Reset Account* 

`Listening for Events and Completed Transactions`
- we can use *contract.once* or *provider.once* both will work
- we will use this functions to listen to when the transaction finishes and we can do something 
- we'll be using *Promises* to be able to wait and listen for the transaction actually be mined and then go ahead in the process.
- as showed below, in our *listenForTransactionMine* function we will return a *promise* using the *resolve()* INSIDE the *provider.once(...)* function, so the only way that the application go on is when the *provider.once* finishes and call the *resolve()* inside.
	- just to remember, *promises* only finishes when *resolve* or *reject* functions are called.
```javascript
function listenForTransactionMine(transactionResponse, provider) {
	console.log(`Mining ${transactionResponse.hash}...`)
	
	return new Promise((resolve, reject) => {
	// listen for this transaction to finish
		provider.once(transactionResponse.hash, (transactionReceipt) => {
			console.log(`Completed with ${transactionReceipt.confirmations} confirmations`)
			resolve()
		})		
	})
}
```

