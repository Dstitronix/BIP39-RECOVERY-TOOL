## Guide on how to recover lost wallet phrases of any wallet (evm based) - BIP39 logic

```
sudo apt update
```
```
sudo apt install nodejs npm -y
```
```
node --version
npm --version
```
```
mkdir ~/seed-recovery
```
```
cd ~/seed-recovery
```
```
npm init -y
```
```
npm install bip39 ethereumjs-wallet ethers node-fetch@2
```
```
nano recovery.js
```
```
// UNIVERSAL RECOVERY — works for 1 OR 2 missing words (just change MODE)
const bip39 = require('bip39');
const { hdkey: HDKey } = require('ethereumjs-wallet');

const TARGET_ADDRESS = "0xcf755ebb6e021930e3fed909df24bcf77f14bde6".toLowerCase();  // ← change

// ────────────────────── CHOOSE MODE HERE ──────────────────────
const MODE = 1;   // ← 1 = one missing word   |   2 = two missing words

const PHRASE = [
  "bomb", "trick", "marriage", "torch",
  "?????", "any", "gather", "blade", "cause", "slush", "mention", "shift"
];   // put ????? where the word(s) are missing (1 or 2 ?????)

const POS1 = 4;   // position of first ?????  (0-based → 5th word = 4)
const POS2 = 8;   // position of second ????? (only used in MODE 2)
// ─────────────────────────────────────────────────────────────

const PATH_BASE = "m/44'/60'/0'/0";

(async () => {
  console.log(`Running in MODE ${MODE} → ${MODE === 1 ? "ONE" : "TWO"} missing word(s)\n`);
  console.log("Target address :", TARGET_ADDRESS, "\n");

  const words = bip39.wordlists.english;
  let tried = 0;

  if (MODE === 1) {
    // ────── ONE missing word (fast) ──────
    for (const w of words) {
      tried++;
      const phrase = [...PHRASE];
      phrase[POS1] = w;
      const mnemonic = phrase.join(" ");

      if (!bip39.validateMnemonic(mnemonic)) continue;

      const seed = await bip39.mnemonicToSeed(mnemonic);
      const root = HDKey.fromMasterSeed(seed);

      for (let i = 0; i < 30; i++) {
        const addr = "0x" + root.derivePath(`${PATH_BASE}/${i}`).getWallet().getAddress().toString("hex").toLowerCase();
        if (addr === TARGET_ADDRESS) {
          console.log("\nFOUND THE MISSING WORD!\n");
          console.log("Word     :", w);
          console.log("Position :", POS1 + 1);
          console.log("Full seed:", mnemonic);
          console.log("Address  :", addr);
          process.exit(0);
        }
      }
      process.stdout.write(`\r1-word mode → trying: ${w.padEnd(12)}`);
    }
  }

  else if (MODE === 2) {
    // ────── TWO missing words (slower) ──────
    for (const w1 of words) {
      for (const w2 of words) {
        tried++;
        const phrase = [...PHRASE];
        phrase[POS1] = w1;
        phrase[POS2] = w2;
        const mnemonic = phrase.join(" ");

        if (!bip39.validateMnemonic(mnemonic)) continue;

        const seed = await bip39.mnemonicToSeed(mnemonic);
        const root = HDKey.fromMasterSeed(seed);

        for (let i = 0; i < 30; i++) {
          const addr = "0x" + root.derivePath(`${PATH_BASE}/${i}`).getWallet().getAddress().toString("hex").toLowerCase();
          if (addr === TARGET_ADDRESS) {
            console.log("\nFOUND BOTH MISSING WORDS!\n");
            console.log(`Position ${POS1 + 1} → ${w1}`);
            console.log(`Position ${POS2 + 1} → ${w2}`);
            console.log("Full seed:", mnemonic);
            console.log("Address  :", addr);
            process.exit(0);
          }
        }
        if (tried % 5000 === 0) process.stdout.write(`\r2-word mode → ${tried.toLocaleString()} combos`);
      }
    }
  }

  console.log("\nNo match found.");
})();
```
```
node recovery.js
```
