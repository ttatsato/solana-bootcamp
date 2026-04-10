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
