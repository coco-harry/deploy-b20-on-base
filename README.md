# deploy-b20

A single-file, no-install tool to deploy a [B20 token](https://docs.base.org/get-started/launch-b20-token) on Base, signed entirely through your own wallet. No private key ever touches this code.

## Why this exists

The official Base docs show how to deploy a B20 token using Foundry (a terminal-based tool). This is an alternative for anyone who wants to do it in under a minute, straight from the browser, using a wallet they already have (MetaMask, Rabby, etc), with zero setup.

## How it works

- Connects to your wallet using the standard `window.ethereum` provider, the same mechanism every dApp uses.
- Builds the exact same calldata the official Foundry script produces (verified byte-for-byte against Base's `base-std` source).
- Calls the official `B20Factory` precompile at `0xB20f000000000000000000000000000000000000`, the same address published in Base's docs.
- Signs entirely inside your wallet's own popup. This page never sees, asks for, or stores your private key or seed phrase, at any point.
- After a successful deploy, automatically reads the new token's address from the `B20Created` event and prompts a second wallet signature to mint your initial supply.

## Usage

1. Download `deploy-b20-on-base.html`.
2. Serve it locally (recommended, some wallet extensions restrict `file://` pages):
   ```bash
   python3 -m http.server 8000
   ```
3. Open `http://localhost:8000/deploy-b20-on-base.html` in your browser.
4. Connect your wallet (make sure it's set to Base Mainnet).
5. Fill in a token name, symbol, decimals, initial mint amount, and a unique salt.
6. Click Deploy, confirm the two wallet popups (deploy, then mint).

Total cost is typically under a cent in gas.

## Security notes

- This is not an official Base product. It's a community-built tool that calls Base's official, documented precompile.
- Review the code yourself before using it, it's a single readable HTML file, nothing minified or obfuscated.
- Your private key never leaves your wallet extension. This page only ever sees your public address and builds unsigned transaction data for your wallet to sign.
- Every token you deploy is publicly visible onchain and linked to your wallet address.

## License

MIT
