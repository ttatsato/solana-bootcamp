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
