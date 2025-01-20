Any web-app that asks for connection and approval of transaction from the wallets i have in my extensions is a dApp.
The way they work is that they have wallet adapters in them, that connect to the wallet extensions by finding if the JavaScript injection happened or not, the extensions inject a global variable into the console like `window.backpack` or `window.phantom` etc.
The dApp cant sign transactions because it cant get ur private key from anywhere, but it can tell the extension wallet that i have got a request for this, can u please sign it.
Now, the extension will ask for confirmation from us, and if we confirm, the dApp catches it and does whatever.

#### Add wallet adapters to npm -
```Shell
npm install @solana/wallet-adapter-base \
    @solana/wallet-adapter-react \
    @solana/wallet-adapter-react-ui \
    @solana/wallet-adapter-wallets \
    @solana/web3.js 
```
This is the oldest wallet adapter library, we can use new ones, but the UI only changes, they all do the same thing, the Jupiter one looks good, use that maybe.

Once a user connects to your `dapp`, you usually ask the user to do a few things with their wallet balance, **these projects we will build** -
1. Request Airdrop
2. Show SOL balances (GET data from the blockchain)
3. Send a transaction (Send a transaction to the blockchain)
4. Sign a message (Verify the ownership of the wallet)

### AirDrop Solana faucet -
make a dApp that can request for solana on the devnet, and already knows our address by connecting to our wallet. This type of dApp is called a faucet as it is basically a faucet that is giving me Solana