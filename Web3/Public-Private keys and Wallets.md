# Creating a public/private keypair

Let’s look at various ways of creating public/private keypairs, signing messages and verifying them
![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2Fc2230636-09dd-4eb0-8d66-40756714a0b1%2FScreenshot_2024-08-16_at_3.58.01_AM.png?table=block&id=e442e515-1fa9-41d8-87c2-c9f27e8f0dd4&cache=v2)

#### **EdDSA -** Edwards-curve Digital Signature Algorithm  - ED25519
Using `@noble/ed25519`
```javascript
import * as ed from "@noble/ed25519";

async function main() {
  // Generate a secure random private key
  const privKey = ed.utils.randomPrivateKey();

  // Convert the message "hello world" to a Uint8Array
  const message = new TextEncoder().encode("hello world");

  // Generate the public key from the private key
  const pubKey = await ed.getPublicKeyAsync(privKey); //can always get the public key from private, but not reverse

  // Sign the message
  const signature = await ed.signAsync(message, privKey);

  // Verify the signature
  const isValid = await ed.verifyAsync(signature, message, pubKey);

  // Output the result
  console.log(isValid); // Should print `true` if the signature is valid
}

main();
```

Using `@Solana/Web3.js`
Uses the same ed25519 library underneath, but provides good high level functions
```JavaScript
import { Keypair } from "@solana/web3.js";
import nacl from "tweetnacl";

// Generate a new keypair
const keypair = Keypair.generate();

// Extract the public and private keys
const publicKey = keypair.publicKey.toString();
const secretKey = keypair.secretKey;

// Display the keys
console.log("Public Key:", publicKey);
console.log("Private Key (Secret Key):", secretKey);

// Convert the message "hello world" to a Uint8Array
const message = new TextEncoder().encode("hello world");

const signature = nacl.sign.detached(message, secretKey);
const result = nacl.sign.detached.verify(
  message,
  signature,
  keypair.publicKey.toBytes(),
);

console.log(result);
```

# **Hierarchical Deterministic (HD) Wallet**
Hierarchical Deterministic (HD) wallets are a type of wallet that can generate a tree of key pairs from a single seed. This allows for the generation of multiple addresses from a single root seed, providing both security and convenience.
![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2Fb2ea8c0f-e0d8-41d5-8caf-1f0305f89eb7%2FScreenshot_2024-08-09_at_6.39.43_PM.png?table=block&id=64d200a7-cdb2-46e5-a1cc-51b63a4d6ac5&cache=v2)
#### Problem
You have to maintain/store multiple `public private` keys if you want to have multiple wallets.
#### **Solution - BIP-32**
[Bitcoin Improvement Proposal](https://www.ledger.com/academy/what-is-a-bitcoin-improvement-proposal-bip) 32 (BIP-32) provided the solution to this problem in 2012. It was proposed by Pieter Wuilla, a Bitcoin Core developer, to simplify the recovery process of crypto wallets. BIP-32 introduced a hierarchical tree-like structure for wallets that allowed you to manage multiple accounts much more easily than was previously possible. It’s essentially a standardized way to derive private and public keys from a master seed.

### Seed phrase
The seed is a binary number derived from the mnemonic phrase.
```javascript
import { generateMnemonic, mnemonicToSeedSync } from "bip39";

const mnemonic = generateMnemonic();
console.log("Generated Mnemonic:", mnemonic);
const seed = mnemonicToSeedSync(mnemonic);
```

#### Derivation paths
![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F1460affc-075a-4a46-81b8-b120d96abad5%2FScreenshot_2024-08-09_at_6.39.43_PM.png?table=block&id=7768a115-f096-4d85-a3f3-3f4a063a64b3&cache=v2)
- Derivation paths specify a systematic way to derive various keys from the master seed.
- They allow users to recreate the same set of addresses and private keys from the seed across different wallets, ensuring interoperability and consistency. (for example if you ever want to port from Phantom to Backpack)
- A derivation path is typically expressed in a format like `m / purpose' / coin_type' / account' / change / address_index`.
- `**m**`: Refers to the master node, or the root of the HD wallet.
- `**purpose**`: A constant that defines the purpose of the wallet (e.g., `44'` for BIP44, which is a standard for HD wallets).
- `**coin_type**`: Indicates the type of cryptocurrency (e.g., `0'` for Bitcoin, `60'` for Ethereum, `501'` for solana).
- `**account**`: Specifies the account number (e.g., `0'` for the first account).
- `**change**`: This is either `0` or `1`, where `0` typically represents external addresses (receiving addresses), and `1` represents internal addresses (change addresses).
- `**address_index**`: A sequential index to generate multiple addresses under the same account and change path.

```javascript
import nacl from "tweetnacl";
import { generateMnemonic, mnemonicToSeedSync } from "bip39";
import { derivePath } from "ed25519-hd-key";
import { Keypair } from "@solana/web3.js";

const mnemonic = generateMnemonic();
const seed = mnemonicToSeedSync(mnemonic);
for (let i = 0; i < 4; i++) {
  const path = `m/44'/501'/${i}'/0'`; // This is the derivation path
  const derivedSeed = derivePath(path, seed.toString("hex")).key;
  const secret = nacl.sign.keyPair.fromSeed(derivedSeed).secretKey;
  console.log(Keypair.fromSecretKey(secret).publicKey.toBase58());
}
```
Ref SOL - [https://github.com/coral-xyz/backpack/blob/master/packages/secure-background/src/blockchain-configs/solana/config.ts#L38](https://github.com/coral-xyz/backpack/blob/master/packages/secure-background/src/blockchain-configs/solana/config.ts#L38)
[https://github.com/coral-xyz/backpack/blob/master/packages/secure-background/src/services/svm/util.ts#L22](https://github.com/coral-xyz/backpack/blob/master/packages/secure-background/src/services/svm/util.ts#L22)

