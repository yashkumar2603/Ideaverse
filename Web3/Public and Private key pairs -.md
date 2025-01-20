### Base58
It is similar to Base64 but uses a different set of characters to avoid visually similar characters and to make the encoded output more user-friendly
Base58 uses 58 different characters:
- Uppercase letters: `A-Z` (excluding `I` and `O`)
- Lowercase letters: `a-z` (excluding `l`)
- Numbers: `1-9` (excluding `0`)
Private and Public keys are stored in Base58
```JavaScript
//Encode to Base 58
const bs58 = require('bs58');

function uint8ArrayToBase58(uint8Array) {
  return bs58.encode(uint8Array);
}

// Example usage:
const byteArray = new Uint8Array([72, 101, 108, 108, 111]); // Corresponds to "Hello"
const base58String = uint8ArrayToBase58(byteArray);
console.log(base58String); // Output: Base58 encoded string
```

```JavaScript
// Decode from base 58
const bs58 = require('bs58');

function base58ToUint8Array(base58String) {
  return bs58.decode(base58String);
}

// Example usage:
const base58 = base58String; // Use the previously encoded Base58 string
const byteArrayFromBase58 = base58ToUint8Array(base58);
console.log(byteArrayFromBase58); // Output: Uint8Array(5) [72, 101, 108, 108, 111]
```

# Hashing vs encryption

### hashing
Hashing is a process of converting data (like a file or a message) into a fixed-size string of characters, which typically appears random.
![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F0c8b4e60-b0dd-4807-914b-2604bbce3649%2FScreenshot_2024-08-09_at_12.11.24_PM.png?table=block&id=c52d28a7-dfd8-4cbe-96bd-d8271ea90097&cache=v2)
Common hashing algorithms - SHA-256, MD5

### Encryption
Encryption is the process of converting plaintext data into an unreadable format, called ciphertext, using a specific algorithm and a key. The data can be decrypted back to its original form only with the appropriate key.
**Characteristics of the Key :**
- **Reversible**: With the correct key, the ciphertext can be decrypted back to plaintext.
- **Key-dependent**: The security of encryption relies on the secrecy of the key.
### Symmetric encryption

The same key is used for both encryption and decryption.
![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F2d9f7548-8b80-4195-9716-d269f0ebfbad%2FScreenshot_2024-08-09_at_12.13.24_PM.png?table=block&id=349525cd-683a-4e53-879c-ef62423bcf20&cache=v2)

```JavaScript
const crypto = require('crypto');

// Generate a random encryption key
const key = crypto.randomBytes(32); // 32 bytes = 256 bits
const iv = crypto.randomBytes(16); // Initialization vector (IV)

// Function to encrypt text
function encrypt(text) {
    const cipher = crypto.createCipheriv('aes-256-cbc', key, iv);
    let encrypted = cipher.update(text, 'utf8', 'hex');
    encrypted += cipher.final('hex');
    return encrypted;
}

// Function to decrypt text
function decrypt(encryptedText) {
    const decipher = crypto.createDecipheriv('aes-256-cbc', key, iv);
    let decrypted = decipher.update(encryptedText, 'hex', 'utf8');
    decrypted += decipher.final('utf8');
    return decrypted;
}

// Example usage
const textToEncrypt = 'Hello, World!';
const encryptedText = encrypt(textToEncrypt);
const decryptedText = decrypt(encryptedText);

console.log('Original Text:', textToEncrypt);
console.log('Encrypted Text:', encryptedText);
console.log('Decrypted Text:', decryptedText);

```
### Asymmetric encryption

 Different keys are used for encryption (public key) and decryption (private key).
![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2Fade1a925-bd4e-4041-9cfb-f5c55a5ac0f9%2FScreenshot_2024-08-09_at_12.22.03_PM.png?table=block&id=a002746e-2534-47f2-8409-8973252ca08e&cache=v2)

it is basically like we sign a message with our private key and broadcast it, then everyone can decrypt it using my public key to get the message.