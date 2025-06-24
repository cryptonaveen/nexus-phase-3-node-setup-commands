# Nexus-phase-3-node-setup-commands

## CLI node
```
sudo apt update & sudo apt upgrade -y
sudo apt install screen curl build-essential pkg-config libssl-dev git-all -y
sudo apt install protobuf-compiler -y
sudo apt update

curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

source $HOME/.cargo/env

rustup target add riscv32i-unknown-none-elf

screen -S nexus

curl https://cli.nexus.xyz/ | sh

source ~/.bashrc

nexus-network start --node-id your-node-id #Replace your-node-id

source ~/.bashrc

nexus-network register-user --wallet-address your-wallet-address #replace your wallet addres

nexus-network start --node-id your-node-id #Replace your node id
```
