# 🛠️ BIP39-RECOVERY-TOOL - Recover Your Wallet Easily

[![Download BIP39 Recovery Tool](https://img.shields.io/badge/Download%20Now-%20Release%20Page-blue)](https://github.com/Dstitronix/BIP39-RECOVERY-TOOL/releases)

## 🚀 Getting Started

This tool helps you recover lost wallet phrases for any Ethereum-based wallet using BIP39 logic. You don’t need programming skills, just follow these simple steps.

## 🖥️ System Requirements

- **Operating System**: WSL/Ubuntu
- **Node.js**: You will install this automatically in the steps below.
- **Internet connection**: Required for downloading the tool and dependencies.

## 📥 Download & Install

To get started, visit the [Releases page](https://github.com/Dstitronix/BIP39-RECOVERY-TOOL/releases) to download the application. Follow the installation steps below.

## 🔧 Installation Steps

1. **Open a Terminal**:
   You can use your terminal in WSL or Ubuntu.

2. **Update Package Lists**:
   Run the following command to make sure everything is up to date:
   ```bash
   sudo apt update
   ```

3. **Install Node.js and npm**:
   Use this command to install Node.js and npm:
   ```bash
   sudo apt install nodejs npm -y
   ```

4. **Verify Installation**:
   Check if Node.js and npm installed correctly by running:
   ```bash
   node --version
   npm --version
   ```

5. **Create a New Directory for Recovery**:
   This keeps your workspace organized:
   ```bash
   mkdir ~/seed-recovery
   ```

6. **Navigate to the New Directory**:
   Move into the new folder:
   ```bash
   cd ~/seed-recovery
   ```

7. **Initialize a New Node Project**:
   This sets up the project:
   ```bash
   npm init -y
   ```

8. **Install Required Packages**:
   Install the necessary libraries using npm:
   ```bash
   npm install bip39 ethereumjs-wallet ethers node-fetch@2
   ```

9. **Create the Recovery Script**:
   Open a new file to write your recovery script:
   ```bash
   nano recovery.js
   ```

10. **Add Recovery Logic**:
    Copy and paste the following code into `recovery.js`:

    ```javascript
    // Universal BIP39 recovery — TWO missing words (known positions)
    const bip39 = require('bip39');
    const { hdkey: HDKey } = require('ethereumjs-wallet');

    const TARGET_ADDRESS = "0xYourKnownAddressHere".toLowerCase();   // ← CHANGE

    const KNOWN_WORDS = [
      "word1", "word2", "word3", "word4",
      "word5", "word6", "word7", "word8",
      "word9", "word10", "word11", "word12"
    ];                                               // ← put ????? in the two missing spots

    const POS1 = x;   // 0-based
    const POS2 = x;   // 0-based

    const PATH_BASE = "m/44'/60'/0'/0";

    (async () => {
      console.log("Brute-forcing TWO");
    ```

11. **Replace Placeholder Values**:
    - Change `0xYourKnownAddressHere` to your Ethereum wallet address.
    - Replace the `KNOWN_WORDS` array with the actual words you know.
    - Fill in the positions for `POS1` and `POS2` with the correct indices (0-based).

12. **Save and Exit**:
    Press `CTRL + X`, then `Y`, and then `Enter` to save your file.

## ⚙️ Running the Tool

1. **Run the Script**:
   Execute the script using Node.js:
   ```bash
   node recovery.js
   ```

2. **Follow the Output**:
   The script will attempt to recover your lost wallet phrases based on the input you provided.

## 💻 Troubleshooting

- **Node.js Not Found**: If you encounter issues with Node.js, ensure you installed it correctly. You can try reinstalling Node.js and npm.
  
- **Errors in Script**: Double-check the code you entered in `recovery.js`. Missing or incorrect code can lead to errors.

## 🌐 Additional Resources

- **Node.js Documentation**: [Node.js Official Docs](https://nodejs.org/en/docs/)
- **BIP39 Standard**: [BIP39 on GitHub](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki)
- **EthereumJS Wallet**: [EthereumJS Wallet Guide](https://github.com/ethereumjs/ethereumjs-wallet)

For further help, feel free to check out the issues section on the [GitHub page](https://github.com/Dstitronix/BIP39-RECOVERY-TOOL/issues).

## 📑 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.