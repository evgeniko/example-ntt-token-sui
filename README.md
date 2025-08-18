# My Coin - Sui Move Project

This is a Sui Move smart contract project that implements a custom coin.

## Prerequisites

- [Sui CLI](https://docs.sui.io/build/install) installed
- Basic understanding of Move language

## Setup and Testing Steps

### 1. Create Testnet Environment
```bash
sui client new-env --alias testnet --rpc https://fullnode.testnet.sui.io:443
```

### 2. Switch to Testnet Environment
```bash
sui client switch --env testnet
```

### 3. Generate New Address (save the recovery phrase for your ntt deployment!)
```bash
sui client new-address ed25519
```

### 4. View Account Information
```bash
sui client active-address
```

### 5. Switch to Generated Address
```bash
sui client switch --address YOUR_GENERATED_ADDRESS
```

### 6. Get Test Tokens
```bash
sui client faucet
```

### 7. Check Balance
```bash
sui client balance
```

Before deploying, you can modify the coin properties in `sources/my_coin.move`:

### **Coin Properties:**
```move
let decimals: u8 = 9;                    // Number of decimal places
let symbol = b"MYC";                     // Token symbol (3-4 characters)
let name = b"My Coin";                   // Token name
let description = b"";                    // Token description
let icon = option::some(url::new_unsafe_from_bytes(b"https://example.com/icon.png"));
```

### **What to Change:**
- **`decimals`**: Change from `9` to your preferred precision (e.g., `6` for 6 decimals)
- **`symbol`**: Change `"MYC"` to your token symbol (e.g., `"BTC"`, `"ETH"`)
- **`name`**: Change `"My Coin"` to your token name (e.g., `"Bitcoin"`, `"Ethereum"`)
- **`description`**: Add a description for your token
- **`icon`**: Change the URL to your token's icon image

### 8. Build the Project
```bash
sui move build
```

### 9. Deploy the Coin to Testnet
```bash
sui client publish --gas-budget 20000000
```

**⚠️ Important: After deployment, look for the "Published Objects" section and save the PackageID:**

```
PackageID: 0x67ad495772e4cf74ef6bf911708973fbad23c7646129dea62ec82492a220a40d
```

**Save the PackageID - you'll need it for the minting step!**

### 10. View Your Deployed Objects
```bash
sui client objects 
```

**This command shows:**
- **UpgradeCap**: Look for `0x0000..0002::package::UpgradeCap` (for package upgrades)
- **Coin Metadata**: Look for `0x0000..0002::coin::CoinMetadata` (your token metadata)
- **TreasuryCap**: Look for `0x0000..0002::coin::TreasuryCap` (for minting/burning)

**Find the TreasuryCap ID here - you'll need it for the minting step!**


### 11. Mint MY_COIN Tokens
```bash
    sui client call
        --package YOUR_DEPLOYED_PACKAGE_ID_STEP10 
        --module MODULE_NAME 
        --function mint 
        --args TREASURYCAP_ID_STEP10 AMOUNT_WITH_DECIMALS RECIPIENT_ADDRESS 
        --gas-budget 5000000
```

**Mint Command Arguments Explained:**
- **`--package`**: Your deployed package ID (contains the my_coin module)
- **`--module`**: The module name within your package
- **`--function`**: The function to call (mint)
- **`--args`**: Three arguments separated by spaces:
  1. **TreasuryCap ID**: `TREASURYCAP_ID_STEP9` (minting authority object)
  2. **Amount**: `1000000000` (1 MY_COIN with 9 decimals = 1 * 10^9)
  3. **Recipient**: `RECIPIENT_ADDRESS` (recipient/your address)
- **`--gas-budget`**: Maximum gas to spend (in MIST)


### 12. Check MY_COIN Balance
```bash
sui client balance --coin-type YOUR_PACKAGE_ID::MODULE_NAME::MY_COIN
```

**Balance Command Explained:**
- **`--coin-type`**: The full type identifier for your MY_COIN token
- **Format**: `YOUR_PACKAGE_ID::MODULE_NAME::MY_COIN` (replace with actual values from step 9)
- **Shows**: Balance of MY_COIN tokens for the currently active address
- **Format**: Amount in smallest units (divide by 10^9 to get actual MY_COIN amount)



## Understanding Placeholder Values

Throughout this README, you'll see placeholder values that need to be replaced with your actual values:

- **`YOUR_GENERATED_ADDRESS`**: The address from step 3 (`sui client new-address ed25519`)
- **`YOUR_DEPLOYED_PACKAGE_ID_STEP9`**: The package ID from step 9 deployment output
- **`MODULE_NAME`**: Your module name (default: `my_coin`)
- **`TREASURYCAP_ID_STEP9`**: The TreasuryCap ID from step 9 deployment output
- **`RECIPIENT_ADDRESS`**: The address to receive minted tokens
- **`AMOUNT_WITH_DECIMALS`**: Amount in smallest units (e.g., `1000000000` for 1 token with 9 decimals)

## Project Structure

- `Move.toml` - Project configuration and dependencies
- `sources/my_coin.move` - Main Move module containing the coin implementation