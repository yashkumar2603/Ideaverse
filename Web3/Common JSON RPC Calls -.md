# Common RPC calls on Solana
## Get account info
Retrieves information about a specific account.
```jsx
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getAccountInfo",
  "params": ["Eg4F6LW8DD3SvFLLigYJBFvRnXSBiLZYYJ3KEePDL95Q"]
}
```
### Get Balance
Gets the balance for a given account
```jsx
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getBalance",
    "params": ["Eg4F6LW8DD3SvFLLigYJBFvRnXSBiLZYYJ3KEePDL95Q"]
}
```
### Get Transaction count
```jsx
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getTransactionCount"
}
```


# Common RPC calls on ETH
### Get balance
```jsx
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "eth_getBalance",
    "params": ["0xaeaa570b50ad00377ff8add27c50a7667c8f1811", "latest"]
}
```
### Get latest block

```jsx
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_blockNumber"
}
```
### Get block by number

```jsx
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_getBlockByNumber",
  "params": ["0x1396d66", true]
}
```