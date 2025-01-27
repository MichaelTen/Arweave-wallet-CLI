# Arweave-wallet-CLI
This is about Arweave Wallet Setup with Node.js. These steps may not work for everyone and you may need to modify some of this some to function as expected. 

# Arweave Wallet Setup with Node.js

Welcome to this concise guide on setting up and managing an Arweave wallet using Node.js. This guide covers creating a wallet, checking your balance, and sending AR tokens. Let's dive in! 🚀
---
## Prerequisites

Make sure you have the following installed on your system:

- Node.js (v18 or higher)
- `arweave` package installed globally or locally
- Sudo privileges (used for most steps)

If Node.js is not installed, you can set it up with:

```bash
sudo apt update && sudo apt install -y nodejs npm
```

---

## Step 1: Create a Wallet 🛠️

1. **Create a Script**
   Use the following command to create the `createWallet.js` file:

   ```bash
   sudo nano /mnt/sda1/arweave_project/createWallet.js
   ```

2. **Add the Code**
   Paste this code into the file and save it:

   ```javascript
   const Arweave = require('arweave');
   const fs = require('fs');

   const arweave = Arweave.init({
       host: 'arweave.net',
       port: 443,
       protocol: 'https'
   });

   async function createWallet() {
       const key = await arweave.wallets.generate();
       const filePath = '/mnt/sda1/arweave_wallet/wallet.json';
       fs.writeFileSync(filePath, JSON.stringify(key));
       console.log(`Wallet saved to ${filePath}`);
   }

   createWallet();
   ```

3. **Run the Script**
   Execute the script to generate the wallet file:

   ```bash
   sudo node /mnt/sda1/arweave_project/createWallet.js
   ```

4. **Verify Wallet Creation**
   Check if the wallet file was successfully created:

   ```bash
   sudo ls /mnt/sda1/arweave_wallet
   ```

---

## Step 2: Check Wallet Balance 💰

1. **Create a Script**
   Create the `checkBalance.js` file:

   ```bash
   sudo nano /mnt/sda1/arweave_project/checkBalance.js
   ```

2. **Add the Code**
   Paste this code into the file and save it:

   ```javascript
   const Arweave = require('arweave');
   const fs = require('fs');

   const arweave = Arweave.init({
       host: 'arweave.net',
       port: 443,
       protocol: 'https'
   });

   async function checkBalance() {
       const key = JSON.parse(fs.readFileSync('/mnt/sda1/arweave_wallet/wallet.json', 'utf-8'));
       const address = await arweave.wallets.jwkToAddress(key);
       const balance = await arweave.wallets.getBalance(address);

       console.log(`Address: ${address}`);
       console.log(`Balance: ${arweave.ar.winstonToAr(balance)} AR`);
   }

   checkBalance();
   ```

3. **Run the Script**
   Check the wallet balance:

   ```bash
   sudo node /mnt/sda1/arweave_project/checkBalance.js
   ```

---

## Step 3: Send AR Tokens 🪙

1. **Create a Script**
   Create the `sendAR.js` file:

   ```bash
   sudo nano /mnt/sda1/arweave_project/sendAR.js
   ```

2. **Add the Code**
   Paste this code into the file and save it:

   ```javascript
   const Arweave = require('arweave');
   const fs = require('fs');

   const arweave = Arweave.init({
       host: 'arweave.net',
       port: 443,
       protocol: 'https'
   });

   async function sendAR() {
       const key = JSON.parse(fs.readFileSync('/mnt/sda1/arweave_wallet/wallet.json', 'utf-8'));
       const targetAddress = 'TARGET_ADDRESS'; // Replace with recipient address
       const amount = arweave.ar.arToWinston('1'); // Replace with amount to send (in AR)

       const transaction = await arweave.createTransaction({
           target: targetAddress,
           quantity: amount
       }, key);

       await arweave.transactions.sign(transaction, key);
       const response = await arweave.transactions.post(transaction);

       console.log('Transaction ID:', transaction.id);
       console.log('Response:', response.status);
   }

   sendAR();
   ```

3. **Run the Script**
   Send AR tokens:

   ```bash
   sudo node /mnt/sda1/arweave_project/sendAR.js
   ```

4. **Replace Placeholder Values**
   Replace `TARGET_ADDRESS` with the actual recipient address and update the amount as needed.

---

## Additional Notes 📜

- **Directory Setup**: Ensure the `/mnt/sda1/arweave_wallet` directory exists:

   ```bash
   sudo mkdir -p /mnt/sda1/arweave_wallet
   ```

- **Permissions**: Ensure the directory is writable:

   ```bash
   sudo chown -R $USER:$USER /mnt/sda1/arweave_wallet
   ```

- **Dependencies**: If the `arweave` package is missing, install it locally:

   ```bash
   sudo npm install arweave
   ```

Enjoy working with your Arweave wallet! 🌟

---

## Step: Install the `arweave` Package (if needed) 🛠️

To use the `arweave` library in your project, you need to install it using npm. Follow these steps:

1. **Navigate to Your Project Directory**  
   Make sure you're in the correct directory where your project is stored. For example:
   ```bash
   cd /mnt/sda1/arweave_project
   ```

2. **Install the `arweave` Package**  
   Use npm to install the `arweave` package locally:
   ```bash
   sudo npm install arweave
   ```

3. **Verify Installation**  
   Ensure the package is installed by checking the `node_modules` folder:
   ```bash
   ls node_modules/arweave
   ```

4. **Global Installation (Optional)**  
   If you'd prefer to install it globally for use across multiple projects, run:
   ```bash
   sudo npm install -g arweave
   ```

   Note: For global usage, you may need to adjust your script to use the global `arweave` package.
---
You're now ready to proceed with creating and managing your wallet! 🎉
---
Once installed, you're ready to work with the `arweave` library! 🚀
---

## Step: Retrieve the Wallet Address 📬

The receive address for an Arweave wallet is derived from the wallet's JSON key file (`wallet.json`). Here's how you can retrieve it:

---

### Step 1: Locate Your Wallet File
Make sure your wallet file is stored in the correct path, for example:
```bash
/mnt/sda1/arweave_wallet/wallet.json
```
---
### Step 2: Use the Following Script to Retrieve the Address
Create a script called `getAddress.js` to fetch the wallet's address:

```bash
sudo nano /mnt/sda1/arweave_project/getAddress.js
```

Add this code to the file:
```javascript
const Arweave = require('arweave');
const fs = require('fs');

const arweave = Arweave.init({
    host: 'arweave.net',
    port: 443,
    protocol: 'https'
});

async function getAddress() {
    const key = JSON.parse(fs.readFileSync('/mnt/sda1/arweave_wallet/wallet.json', 'utf-8'));
    const address = await arweave.wallets.jwkToAddress(key);

    console.log(`Wallet Address: ${address}`);
}

getAddress();
```
---
### Step 3: Run the Script
Run the script to retrieve the address:
```bash
sudo node /mnt/sda1/arweave_project/getAddress.js
```
---
### Step 4: Output
The script will display the wallet's receive address. Copy this address to use for receiving funds.

---

