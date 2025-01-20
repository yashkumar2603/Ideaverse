We can have accounts on the Eth and Solana blockchains, that can have some code payload, and these payloads can be deployed on the blockchain, this way we can create a token which uses the same Solana and Ethereum blockchain under the hood, but does not suffer the problem of cold start(where less miners are present so it is slow and less miners come in and it gets slower). On Ethereum, we write smart contracts for making our own tokens, on Solana, they have written a smart contract for us known as the Solana Token program. and every  new token made is a account derived from it with 
##### Accounts on Solana - 
On the Solana blockchain, an "account" is a fundamental data structure used to store various types of information.

1. **Data Storage**: Accounts on Solana are used to store data required by programs (smart contracts) or to maintain state
2. **Lamports**: Accounts hold a balance of Solana’s native cryptocurrency, lamports. Lamports are used to pay for transaction fees and to rent the space that the account occupies on the blockchain.![[Pasted image 20240903013009.png]]
3. **Programs:** On Solana, programs are special accounts that contain executable code. These accounts are distinct from regular data accounts in that they are designed to be executed by the blockchain when triggered by a transaction.
##### Rent on Solana -
If an account has some program in it, then it has to pay some rent to the blockchain, to keep that much bytes of data, otherwise people will fill the whole blockchain with data and make it a problem.
So u have to keep some certain amount of SOL in the account, then it is exempted from storage tax, or pay some fee every block for ur data, it depends.

#### Solana CLI -
```Shell
sh -c "$(curl -sSfL https://release.anza.xyz/stable/install)"
```
Solana CLI installing.
Commands -
- `solana-keygen new` -> creates a new keypair in the designated path inside `id.json`, which holds the private key, and a new can be created by overwriting using `--force` flag.
- `solana address` to see the address from the private key generated.

#### RPC, testnet, devnet, mainnet - 
By default the RPC URL, the place from where it fetches the data of the balance and stuff is devnet, 
On devnet, it is a dummy blockchain, we can airdrop us some solana on the devnet and then play with it, which we cant in the mainnet.
To set it to mainnet, run -
`solana config set --url http://api.mainnet-beta.solana.com` and this sets it to mainnet fetching , which is the primary and original solana blockchain.
We can also set this to API URLs from Infura, Alchemy, or anything.

###### Airdrop -
When we are on devnet, we can airdrop us some solana by running `solana airdrop <Amount>` and we get that much solana in our account on devnet.

#### Token-mint 
When ever we create a new token from the token program, it creates an account which is the account that holds the mint for the token, and the data model is -
![[Pasted image 20240904010836.png]]
Creating `your own token` (100x coin lets say) requires understanding the `Token Program` that is written by the engineers at Solana - [https://github.com/solana-labs/solana-program-library](https://github.com/solana-labs/solana-program-library)
Specifically, the way to create a `token` requires you to
1. Create a token mint
2. Create an `associated token account` for this mint and for a specific user
3. Mint tokens to that user.
### Token mint
It’s like a `bank` that has the authority to create more coins. It can also have the authority to `freeze coins`. It is just a minting factory, initially the supply is zero, we can set the `decimals` to some value to specify that how small of a value of this token can be minted/transferred. if decimals is 4, then 0.0001 can be the minimum transferred.

If decimals is kept at 0, then it becomes a `Non-Fungible Token(NFT)`, we can only send 1 of it minimum, nothing less than 1.
When we start a mint, the mint authority is the public key of the private key stored in the `id.json`, when created from the CLI.
### Associated token account
Before you can ask other people to send you a token, you need to create an `associated token account` for that token and your public key
Reference - [https://spl.solana.com/token](https://spl.solana.com/token)
![[Pasted image 20240904011240.png]]
![[Pasted image 20240904011331.png]]
![[Pasted image 20240904011401.png]]
![[Pasted image 20240904011434.png]]

If some-one does not have an account on my created token, but i still send them some of it, then the  account is created for them and the rent exemption is paid by the sender for the new account created.

### Doing all this in JS -
**Create a new cli wallet**
```Shell
solana-keygen new
```

**Set the RPC url**
```Shell
solana config set --url https://api.devnet.solana.com
```

**Create an empty JS file**
```JavaScript
npm init -y touch index.js
```
​
**Install dependencies**
```Shell
npm install @solana/web3.js @solana/spl-token
```
​
**Write a function to airdrop yourself some solana**
```JavaScript
const {Connection, LAMPORTS_PER_SOL, clusterApiUrl, PublicKey} = require('@solana/web3.js'); 
const connection = new Connection(clusterApiUrl('devnet')); 
async function airdrop(publicKey, amount) { 
	const airdropSignature = await connection.requestAirdrop(new PublicKey(publicKey), amount); 
	await connection.confirmTransaction({signature: airdropSignature}) 
} 
airdrop("GokppTzVZi2LT1MSTWoEprM4YLDPy7wQ478Rm3r77yEw",
		LAMPORTS_PER_SOL).then(signature => { 
	console.log('Airdrop signature:', signature); 
});
```

**Check your balance**
```Shell
solana balance
```

**Create token mint**
```JavaScript
const { createMint } = require('@solana/spl-token'); 

const { Keypair, Connection, clusterApiUrl, TOKEN_PROGRAM_ID } = require('@solana/web3.js'); 

const payer = Keypair.fromSecretKey(Uint8Array.from([102,144,169,42,220,87,99,85,100,128,197,17,41,234,250,84,87,98,161,74,15,249,83,6,120,159,135,22,46,164,204,141,234,217,146,214,61,187,254,97,124,111,61,29,54,110,245,186,11,253,11,127,213,20,73,8,25,201,22,107,4,75,26,120])); 

const mintAthority = payer;

const connection = new Connection(clusterApiUrl('devnet')); 
async function createMintForToken(payer, mintAuthority) { 
	const mint = await createMint( 
		connection, 
		payer, 
		mintAuthority, 
		null, 
		6, 
		TOKEN_PROGRAM_ID 
	); 
	console.log('Mint created at', mint.toBase58()); 
	return mint; 
} 
async function main() { 
	const mint = await createMintForToken(payer, mintAthority.publicKey); 
} 
main();
```
![[Pasted image 20240904105035.png]]
**Verify token mint on chain**

**Create an associated token account, mint some tokens**
```JavaScript
const { createMint, getOrCreateAssociatedTokenAccount, mintTo } = require('@solana/spl-token');
const { Keypair, Connection, clusterApiUrl,  TOKEN_PROGRAM_ID, PublicKey } = require('@solana/web3.js');

const payer = Keypair.fromSecretKey(Uint8Array.from([102,144,169,42,220,87,99,85,100,128,197,17,41,234,250,84,87,98,161,74,15,249,83,6,120,159,135,22,46,164,204,141,234,217,146,214,61,187,254,97,124,111,61,29,54,110,245,186,11,253,11,127,213,20,73,8,25,201,22,107,4,75,26,120]));

const mintAthority = payer;

const connection = new Connection(clusterApiUrl('devnet'));

async function createMintForToken(payer, mintAuthority) {
    const mint = await createMint(
        connection,
        payer,
        mintAuthority,
        null,
        6,
        TOKEN_PROGRAM_ID
    );
    console.log('Mint created at', mint.toBase58());
    return mint;
}

async function mintNewTokens(mint, to, amount) { 
    const tokenAccount = await getOrCreateAssociatedTokenAccount(
        connection,
        payer,
        mint,
        new PublicKey(to)
      );

      console.log('Token account created at', tokenAccount.address.toBase58());
      await mintTo(
        connection,
        payer,
        mint,
        tokenAccount.address,
        payer,
        amount
      )
      console.log('Minted', amount, 'tokens to', tokenAccount.address.toBase58());
}

async function main() {
    const mint = await createMintForToken(payer, mintAthority.publicKey);
    await mintNewTokens(mint, mintAthority.publicKey, 100);    
}

main();
```
![[Pasted image 20240904105205.png]]
**Check your balances in the explorer and import in Phantom**

### Token - 22 Program -
Allows to attach Metadata to the token, u created.
The way to do that is -
The Token-2022 Program, also known as Token Extensions, is a superset of the functionality provided by the [Token Program](https://spl.solana.com/token).
**Create a token with metadata enabled**
```Shell
spl-token --program-id TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb create-token --enable-metadata
```

**Create metadata**
```Shell
spl-token initialize-metadata pXfZ6Hg2s78m1iSRVsdzos9TmfkqkQdv5MmQrr77ZQK 100xx 100xxx https://cdn.100xdevs.com/metadata.json
```

**Create ATA**
```Shell
spl-token create-account pXfZ6Hg2s78m1iSRVsdzos9TmfkqkQdv5MmQrr77ZQK
```
​
**Mint**
```Shell
spl-token mint 1000
```

## Assignment
1. Show all the tokens that the user has in our web based wallet
2. Create a token launchpad website that lets users launch tokens (take things like decimals, freeze authority as inputs from the user)