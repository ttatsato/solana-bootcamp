# Install Solana development tools
exeucte the following command in the terminal.

```
curl --proto '=https' --tlsv1.2 -sSfL https://solana-install.solana.workers.dev | bash
```

If the output loogs like below, the installation was successful.

```
└  Done!  Review skills before use; they run with full agent permissions.

[INFO] Solana dev skill installation complete.


Installed Versions:
Rust: rustc 1.94.1 (e408947bf 2026-03-25) (Homebrew)
Solana CLI: solana-cli 3.1.12 (src:6c1ba346; feat:4140108451, client:Agave)
Anchor CLI: anchor-cli 0.32.1
Surfpool CLI: surfpool 1.1.2
Node.js: v25.8.0
Yarn: 1.22.22

Installation complete. Please restart your terminal to apply all changes.
```

## reload shell
```
source ~/.zshrc
```


## check tools and versions
```
rustc --version && solana --version && anchor --version && surfpool --version && node --version && yarn --version
rustc 1.94.1 (e408947bf 2026-03-25) (Homebrew)
solana-cli 3.1.12 (src:6c1ba346; feat:4140108451, client:Agave)
anchor-cli 0.32.1
surfpool 1.1.2
v25.8.0
1.22.22
```

### explain tools 
rustc: Rust compiler
Solana cli: Create wallet or deploy programs on test enviroment.
Anchor cli: Framework for solana. 
Surfpool cli: it can run a validator node locally.
