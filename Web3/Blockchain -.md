Check Blue diary for notes 
This is nice 
![[Blockchain - 2024_08_05.excalidraw]]
Also this 
[Blockchain Demo](https://andersbrownworth.com/blockchain/)
[But how does bitcoin actually work? - YouTube](https://www.youtube.com/watch?v=bBC-nXj3Ng4)
[[Public and Private key pairs -]]
[[Solana vs. Ethereum]]
[[Public-Private keys and Wallets]]
[[Wallet Project]]
[[JSON RPC]]

### User side
1. User first creates a `public/private` keypair
2. They create a `transaction` that they want to do (send Rs 50 to Alice). The transaction includes all necessary details like the recipient's address, the amount and some blockchain specific parameters (for eg - latestBlockHash in case of solana)
3. They hash the `transaction`
4. They `sign` the transaction using their private key
5. They send the `raw transaction` , `signature` and their `public key` to a node on the blockchain.
#### Miner
1. Hashes the original message to generate a `hash`
2. Verifies the `signature` using the users `public key` and the `hash` generated in step 1
3. Transaction validation - The miner/validator checks additional aspects of the transaction, such as ensuring the user has sufficient funds
4. If everything checks out, adds the transaction to the block