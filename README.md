### 🧩Guide on how to recover lost wallet phrases of any wallet (evm based) - BIP39 logic
----
#### use wsl/ubuntu and paste the commands one by one :

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
  console.log("Brute-forcing TWO missing words →", TARGET_ADDRESS);
  console.log(`Positions: ${POS1+1} and ${POS2+1}\n`);

  const words = bip39.wordlists.english;
  let tried = 0;

  for (const w1 of words) {
    for (const w2 of words) {
      tried++;
      const phrase = [...KNOWN_WORDS];
      phrase[POS1] = w1;
      phrase[POS2] = w2;
      const mnemonic = phrase.join(" ");

      if (!bip39.validateMnemonic(mnemonic)) continue;

      const seed = await bip39.mnemonicToSeed(mnemonic);
      const root = HDKey.fromMasterSeed(seed);

      for (let i = 0; i < 30; i++) {
        const path = `${PATH_BASE}/${i}`;
        const addr = "0x" + root.derivePath(path).getWallet().getAddress().toString("hex").toLowerCase();

        if (addr === TARGET_ADDRESS) {
          console.log("\nFOUND BOTH MISSING WORDS!\n");
          console.log("Word 1 →", w1, `(position ${POS1+1})`);
          console.log("Word 2 →", w2, `(position ${POS2+1})`);
          console.log("Full phrase :", mnemonic);
          console.log("Address     :", addr);
          console.log("Path        :", path);
          process.exit(0);
        }
      }

      if (tried % 5000 === 0) {
        process.stdout.write(`\rTried ${tried.toLocaleString()} combos → ${w1} / ${w2}`);
      }
    }
  }
  console.log("\nNo match found (extremely rare).");
})();
```
#### change: const TARGET_ADDRESS to your address
#### put your rest phrase in : const KNOWN_WORDS and the word/s you lost put `?????` there upto 2 words can be recovered for now
#### put const POS1 = lost word1 position > note its 0 based, eg : if you lost 2nd word put 0, if your 3rd word put 2 and so on
#### put const POS2 = lost word2 position > same as above, if you are targetting only 1 word put any 1-9 number here then

#### ctrl + o > enter
#### ctrl + x

```
node recovery.js
```
