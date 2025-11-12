# Citrea-clementine-Trusted-Setup-Ceremony;

![1500x500](https://github.com/user-attachments/assets/b4b945ad-6ffc-4889-abd7-63ff7404483a)


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

<img width="575" height="84" alt="image" src="https://github.com/user-attachments/assets/3ef8abed-a131-41bf-b831-051da655cf2e" />


## BitVM Dosyaları Çekelim - Kurulum

```bash
screen -S ceremony
```

```bash
git clone https://github.com/chainwayxyz/risc0-to-bitvm2
cd risc0-to-bitvm2
```

<img width="692" height="177" alt="image" src="https://github.com/user-attachments/assets/25fe72c1-f92a-4acd-b4ba-a14d60266cbb" />

## Git-LFS

```bash
sudo apt-get install -y git-lfs
git lfs pull
```

<img width="881" height="651" alt="image" src="https://github.com/user-attachments/assets/efe6e627-543c-43d4-96bd-3677029b8e53" />


## R1CS Dosyasını Üretelim ; 
```bash
# circomlib (used by the circuit)
git clone https://github.com/iden3/circomlib.git

# compile the circuit -> verify_for_guest.r1cs
sed -i '$d' ./groth16_proof/circuits/stark_verify.circom
circom groth16_proof/circuits/verify_for_guest.circom --r1cs --O2

# (optional) shasum check of the r1cs you built
sha256sum verify_for_guest.r1cs
# c90be1fb4b83f8f10f4be51e47ebdde2fd97dacd60d658d63d6828f3f642a116
```

<img width="750" height="441" alt="image" src="https://github.com/user-attachments/assets/e95d8f7f-83da-4412-9953-5b282f77720b" />

- Biraz uzun süredü ( 10-15 dk )

## Powers of Tau Ve Finaly Zkey İndirelim ; 

```bash
# download the exact artifacts used for the ceremony (10+ GB download)
wget https://pse-trusted-setup-ppot.s3.eu-central-1.amazonaws.com/pot28_0080/ppot_0080_23.ptau
wget https://static.mainnet.citrea.xyz/verify_for_guest_final.zkey

# (optional) check them, too
sha256sum ppot_0080_23.ptau verify_for_guest_final.zkey
# 05085994c8aa34e4925585202e178e09fb8acc04e9565311751e8a04dec81cb1  ppot_0080_23.ptau
# 86fc7478fce8b91b502538cc64e8295a104f68ecf025042a21b10b54d7d504c0  verify_for_guest_final.zkey
```

## Başlatalım ; 

```bash
export NODE_OPTIONS="--max-old-space-size=32768"
```

```bash
snarkjs zkey verify verify_for_guest.r1cs ppot_0080_23.ptau verify_for_guest_final.zkey
```

- 30 Dakika İle 1 Saat Arası Sürer.
