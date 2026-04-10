# Create Solana Counter App

First, execute the following command to see available templates.
```
npx create-solana-dapp --list-templates
```

## Create Project with Template
```
npx create-solana-dapp -t web3js-next-tailwind-counter
```

## Build the Anchor Program
Move to the anchor directory and build.
```
cd solana-counter/anchor
anchor build
```

## Sync Program ID
The template has a hardcoded program ID from the original author's environment.
When you run `anchor build`, a new keypair is generated at `target/deploy/counter-keypair.json`, which has a different address.
You need to sync this to avoid a `DeclaredProgramIdMismatch` error.
```
anchor keys sync
anchor build
```

## Start the Local Validator
Open a **separate terminal** and run:
```
solana-test-validator --reset
```

## Airdrop SOL to Your Wallet
In another terminal, make sure you are connected to localhost:
```
solana config set --url localhost
```

Create a wallet keypair if you haven't already:
```
solana-keygen new -o ~/.config/solana/id.json
```

Airdrop test SOL:
```
solana airdrop 2
```

## Deploy the Program
```
cd solana-counter/anchor
anchor deploy
```

## Run Tests
Make sure the local validator is running in a separate terminal, then run:
```
cd solana-counter/anchor
anchor test --skip-local-validator
```

`--skip-local-validator` prevents Anchor from starting its own validator, since you already have one running.

If you are not running a validator separately, you can just use `anchor test` and it will start and stop one automatically.

Expected output:
```
 PASS  tests/counter.test.ts
  counter
    ✓ Initialize Counter
    ✓ Increment Counter
    ✓ Increment Counter Again
    ✓ Decrement Counter
    ✓ Set counter value
    ✓ Set close the counter account

Tests:       6 passed, 6 total
```

# Frontend
Start the frontend dev server:
```
cd solana-counter
npm run dev
```

Then open the app in your browser. You need a wallet such as [Phantom](https://phantom.app/).

## Configure Phantom for Localnet
In Phantom:

Settings → Developer Settings → Testnet Mode → Localnet

## Connect to the App
Back on the frontend screen:

1. Select network → **Local**
2. Click **Connect Wallet**

When connecting the wallet, Phantom will show a danger warning. It is safe to confirm this time because you are connecting to your own local validator. However, **never confirm such warnings on other sites**, as they usually indicate something suspicious.

# Execute Counter
Click "Counter Program" in the navigation bar, then click the "Create" button.
A card will appear showing "Counter: 0".

Click "Increment".
Approve the transaction in your wallet, and the counter on the card will increment.

## Verify the Counter on Chain via CLI
The counter value is not stored in the browser — it lives in an account on the Solana blockchain.
You can inspect it directly from the CLI.

Copy the counter account address shown on the card, then run:
```
solana account <COUNTER_ACCOUNT_ADDRESS> --url localhost
```

Example output:
```
Public Key: 7Bv4qvYHRprhpBTzPzxnUUCWR2rEv58YyK8M1ZFLkcap
Balance: 0.00095352 SOL
Owner: 3j8JPxxDhYpjnp82y3XRatmJMf8ZQYMk5zbpHWav4bnf
Executable: false
Length: 9 (0x9) bytes
0000:   ff b0 04 f5  bc fd 7c 19  02
```

How to read it:
- **Owner**: the counter program ID — confirms the account is owned by your program.
- **Balance**: rent deposit, which will be returned to the payer when the account is closed.
- **Data**: 9 bytes total.
  - First 8 bytes (`ff b0 04 f5 bc fd 7c 19`): Anchor account discriminator (identifies the account type).
  - Last 1 byte (`02`): the actual `count: u8` value — here it is `2`.

Try clicking Increment again and re-running the command — the last byte will change, proving that the state lives on chain.
