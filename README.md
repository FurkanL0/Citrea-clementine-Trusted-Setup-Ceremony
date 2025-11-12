# Citrea-clementine-Trusted-Setup-Ceremony;

| X        | Minimum              |
|------------------|----------------------------|
| **CPU**          | Belirsiz++ |
| **RAM**          | 32++ GB                    |
| **Disk**      | 50 GB+ NVME GB SDD                   |
| **Internet Hızı**      | 1 Gbps+  |
| **Ubuntu**      | Ubuntu 24.04++  |


| Server         | Link              | Features |
|------------------|----------------------------|----------------------------|
| **NetCup**          | [Link](https://www.netcup.com/en/?ref=261820) | Cheap / Paypal |


## Server Güncelleme : 

```bash
sudo apt update -y && sudo apt upgrade -y
```
## Paketleri İndirelim :

```bash
sudo apt install htop ca-certificates zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev tmux iptables curl nvme-cli git wget make jq libleveldb-dev build-essential pkg-config ncdu npm tar clang bsdmainutils lsb-release libssl-dev libreadline-dev libffi-dev jq gcc screen file unzip lz4 -y
```

## Rust ; 

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

<img width="547" height="342" alt="image" src="https://github.com/user-attachments/assets/acd40ba6-ff98-4d73-ba54-2f766776e385" />


- 1 sonrasında Enter.

```bash
. "$HOME/.cargo/env"
```


## Circom 

```bash
git clone https://github.com/iden3/circom.git
cd circom
cargo install --path circom          # installs the 'circom' binary into ~/.cargo/bin
circom --version                     # sanity check (expect v2.2.x)
cd ..
```

<img width="587" height="96" alt="image" src="https://github.com/user-attachments/assets/1e60ddab-7936-4ad5-a81c-ebb7fd406609" />


## Snark JS

```bash
npm install -g snarkjs
snarkjs --version
```

## BitVM Dosyaları Çekelim - Kurulum

```bash
screen -S ceremony
```

```bash
git clone https://github.com/chainwayxyz/risc0-to-bitvm2
cd risc0-to-bitvm2
```
