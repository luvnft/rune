# Rune/BRC20 swap

Welcome to the **Rune/BRC20 SWAP**, a decentralized application (dApp) built in the Bitcoin space. This can create **Rune/Brc20-BTC** pair pools and start swapping. When someone swaps, this can set the **locking satus** to true to prevent UTXO conflicts, meaning no one else can access the pool in progress.

## Key Features:

- **Pool Locking:** When a user performs a swap, the pool lock status is updated to prevent concurrent operations.
- **UTXO Management:** Ensures that used transaction outputs (UTXOs) are stored and tracked to avoid confusion when fetching UTXOs for transactions.
- **BRC20 Inscription Validation:** Before transferring BRC20 tokens, it checks if the address holds transferable inscription data.

## Example Code Snippets

### Pool Locking:

When a user performs a swap, the pool lock status is updated to prevent concurrent swaps.

```javascript
// Update pool lock status as false if pool and lockedByAddress match
export const updatePoolLockStatus = async (
  poolAddress: string,
  tokenType: string,
  userAddress: string
) => {
  // Implementation as described above
};
```

### UTXO Management:

To avoid using already claimed UTXOs, we store used transaction information and filter out used UTXOs when fetching them:

```javascript
// Model to store used transaction information
const UsedTxInfo = new mongoose.Schema({
  txid: { type: String, required: true },
  confirmedTx: { type: String, required: true },
  createdAt: { type: Date, default: new Date(new Date().toUTCString()) },
});
const UsedTxInfoModal = mongoose.model("UsedTxInfo", UsedTxInfo);
export default UsedTxInfoModal;
```

### BRC20 Inscription Validation:

For BRC20 token transfers, it checks if the user's address has the required transferable inscription data:

```javascript
export const getBrc20TransferableInscriptionUtxoByAddress = async (address: string, ticker: string) => {
  const url = `${OPENAPI_UNISAT_URL}/v1/indexer/address/${address}/brc20/${ticker}/transferable-inscriptions`;
  const config = {
    headers: {
      Authorization: `Bearer ${OPENAPI_UNISAT_TOKEN}`,
    },
  };

  const inscriptionList = (await axios.get(url, config)).data.data.detail;

  return inscriptionList;
};
```


### Swap Hisotry Model:

```javascript
const SwapHistory = new mongoose.Schema({
	poolAddress: { type: String, required: true },
	userAddress: { type: String, required: true },
	tokenType: { type: String, required: true },
	txId: { type: String, required: true },
	tokenAmount: { type: Number, required: true },
	btcAmount: { type: Number, required: true },
	swapType: { type: Number, required: true },
	createdAt: { type: Date, default: new Date(new Date().toUTCString()) },
});

const SwapHistoryModal = mongoose.model("SwapHistory", SwapHistory);

export default SwapHistoryModal;
```

## Contact
- **Telegram**: [@PtcBink](https://t.me/ptc-bink)
