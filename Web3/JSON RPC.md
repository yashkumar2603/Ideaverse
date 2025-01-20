# RPC, JSON-RPC
![[Pasted image 20240822003434.png]]
JSON-RPC is a remote procedure call (RPC) protocol encoded in JSON (JavaScript Object Notation). It allows for communication between a client and a server over a network. JSON-RPC enables a client to invoke methods on a server and receive responses, similar to traditional RPC protocols but using JSON for data formatting.
As a user, you interact with the blockchain for two purposes -
1. To send a `transction`
2. To fetch some details from the blockchain (balances etc)

In both of these, the way to interact with the blockchain is using JSON-RPC
JSON RPC Spec - [https://www.jsonrpc.org/specification](https://www.jsonrpc.org/specification)
<aside> 💡 There are other ways to do RPC’s like GRPC, TRPC

</aside>


# Wei, Lamports
## ETH
Wei:
Definition: Wei is the smallest unit of cryptocurrency in the Ethereum network. It's similar to how a cent is to a dollar.
Value: 1 Ether (ETH) = 10^18 Wei.

Gwei
Definition: A larger unit of Ether commonly used in the context of gas prices.
Conversion: 1 Ether = 10^9 gwei
### Lamports
In the Solana blockchain, the smallest unit of currency is called a lamport. Just as wei is to Ether in Ethereum, lamports are to SOL (the native token of Solana).
1 SOL = 10 ^ 9 Lamports
```JavaScript
const { LAMPORTS_PER_SOL } = require("@solana/web3.js") 
console.log(LAMPORTS_PER_SOL)
```
You can grab your own RPC server from one of many providers -
1. Quicknode
2. Alchemy
3. Helius
4. Infura

[[Common JSON RPC Calls -]]

![[Pasted image 20240822003043.png]]

