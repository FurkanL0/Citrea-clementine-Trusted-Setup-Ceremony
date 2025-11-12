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

<img width="939" height="230" alt="image" src="https://github.com/user-attachments/assets/68561c82-9b59-4611-beb1-1ddbf4177830" />

- Ara ara CPU FULL Çekiyor - validatör olan bir sunucuda kullanmanızı önermem.

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

- 14-15 GB İndiriyor.

<img width="1053" height="632" alt="image" src="https://github.com/user-attachments/assets/56219aa8-e1a7-4ecf-8246-f8aded5a3374" />


## Başlatalım ; 

```bash
export NODE_OPTIONS="--max-old-space-size=32768"
```

```bash
snarkjs zkey verify verify_for_guest.r1cs ppot_0080_23.ptau verify_for_guest_final.zkey | tee verify_log.txt
```

<img width="960" height="130" alt="image" src="https://github.com/user-attachments/assets/baef5c1a-7bb4-406c-b259-e0063ec6620e" />




- 30 Dakika İle 1 Saat Arası Sürer.

- Bitince Böyle Yazcak : snarkJS: ZKey Ok!

- Örnek Bir Bitiş ; 

- Kaynak : https://gist.github.com/ekrembal/527f44c91736a511f6bfd61e01bc4f22

```bash
snarkjs zkey verify risc0-to-bitvm2/verify_for_guest.r1cs ppot_0080_23.ptau verify_for_guest_final.zkey
[INFO]  snarkJS: Reading r1cs
[INFO]  snarkJS: Reading tauG1
[INFO]  snarkJS: Reading tauG2
[INFO]  snarkJS: Reading alphatauG1
[INFO]  snarkJS: Reading betatauG1
[INFO]  snarkJS: Circuit hash: 
		d6bdd0cc 0e74be24 352f9881 671924a0
		fc9fe97e 08354020 8e557302 3d9dec06
		53fbb619 de8d51f7 f457aaac 2da35ff0
		808415ac d17442c2 f81a68f1 c0e6212b
[INFO]  snarkJS: Circuit Hash: 
		d6bdd0cc 0e74be24 352f9881 671924a0
		fc9fe97e 08354020 8e557302 3d9dec06
		53fbb619 de8d51f7 f457aaac 2da35ff0
		808415ac d17442c2 f81a68f1 c0e6212b
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #64 ekrembal-29926881:
		841604dd 2c1349dd be9b720c 18ab55bd
		9479f6c4 199a571b a348423c b0d868bb
		fc19c897 f4735432 3b643b10 123e17d7
		cc081dd0 cc42f60d 0b0f2cfd 067b5861
[INFO]  snarkJS: Beacon generator: 9e32ff7fcb931c856923b50df8ce27da4a10d7150fee0717a1fd30db3cabb9c0
[INFO]  snarkJS: Beacon iterations Exp: 10
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #63 tugcesmith-153603286:
		9c6e6abf ccc424c7 8de715cb 45c5b906
		a1e54242 f529e4a2 f6490591 030a5aec
		6afde86b 925e9257 4305f750 49d906d2
		8efc92d9 b8819b2b f8be4d54 024a9673
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #62 asavagegalaxy-219733929:
		e4e63083 cb970436 1e93583e b5b9fa40
		287a153f 234cdf96 ecd552fb 1c039bde
		b006d0dd 09e290a2 3e852123 d72ea765
		1d762fd4 e04a0210 d1fc18c4 3960681d
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #61 ybyesilyurt-102187042:
		6fbe2e94 bde9cf8b 7b73e4a3 8930a764
		1a7766e2 a4972d82 28d1da2d 54db77fa
		3142a5b0 cfd942b7 a5526b4f df3837ee
		65c4544e 20af5bf3 cc424075 cdcc4006
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #60 b-j-roberts-54774639:
		c516e7e7 cd356130 8abacc1a fa7a1f5f
		19f2c155 4b99381e f8fe11af c24fad9e
		66c700ad e8520a68 7f387da7 c4e44ae3
		3af3ee3a ee5a5935 86217c36 6ce3dc50
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #59 m-kus-44951260:
		60eb97e3 149720df 68c52498 b610f027
		a553d5d5 ec3c2eed 878edc8c 8777241f
		87280845 914ceaf5 9bfe022b 02d9e199
		3cdb04f1 51efcbbb df222cfb 8ae597d7
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #58 VladdyC-56368755:
		4d58edd0 d3632a8d 4eeeb6d9 f0dababb
		6e9ac8df c209bcb3 21bf9155 a599779b
		ab7b7eaa 48eddc5e 16984cdc d151db5a
		cf8b832d 0fb467ee b352cdad a96db119
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #57 aphelionz-106148:
		4277d80d 93c17514 d1804a6c 9da33a9e
		841b65bd e5b87110 65ccd70f f474f441
		0fe9c185 cc43c363 83bf0315 5cfeee59
		2494256f ab011e3f 86972d4a b51499ca
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #56 0xanmol-53006995:
		d4549ede 6dd6185b 9d31a5af 19d7a5ff
		2073c9d6 bc8b5f6c 71ccae94 db881d9e
		aba42c99 1406372d a8a7e530 680a1a58
		9cf02c27 0d279838 e7db16c7 2f8f547d
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #55 bajpai244-41180869:
		f9453965 706bb57e cf16baf1 0c464e6f
		6a503807 07619a48 5cb6749d 523b18b3
		c3e778cb 1ef60382 58801357 6308a6f6
		e48cfced e01fd097 763c6afb c9087ee0
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #54 iiyyaammaami-238175802:
		acb7970f 8bfe5fb5 a0caffed 28bb3aab
		7435667c f3e22671 1156f612 8fe6b9ea
		c8cf251a 19cdd2bb dc789b5b 47d65537
		4f205b86 6b25b973 16f9318c 2e0f268d
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #53 timpel-fcs-185487257:
		91e0b6cc 249b3734 87a8e4a8 60b485e4
		e7a1bf98 75f6ce27 75200a51 4fad37b8
		5a1a5c92 6bcefdc7 da5eb9ac 24ff376a
		759bcc4c 0d969447 3ef6050f 3a194578
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #52 HashKeyCloud-76032176:
		b10ddd1c d2b3db0d 4d1cdcdd e03d1e9d
		57ffb4cb e166a0ba 7c2af616 e1a66094
		91456605 b4cf49fe 440ed4b8 cdf83bfb
		d949d5eb 751b40ec 68e31f7f ed10c747
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #51 nategraf-5684286:
		f6c3347f 8330ff44 46935650 37d5618c
		b84d6dc5 cd37388d a678751f 0817ff40
		d257d9c5 08830b7a e0dfae27 72ee7c25
		703ce3c7 f115e995 538ea87f 4bc14153
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #50 gehlotanish-35562972:
		a14f8e30 db2209b9 1c7dcd1b 8f13b64e
		e427b764 ac8161c3 147cd21a 987079a9
		ca9873c9 5d39cbf4 a5505a66 8345f535
		ab12febf 427df2c2 938b4b3f a86cba4e
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #49 raven-ruiwen-30341791:
		8fba3856 39c11e48 8ff69e85 1c430a84
		754e4b64 d5bc41ae abbef20d 242943af
		4a0dcae7 1ba57ff4 d82e2afc 63e704b7
		938c12ab 543e66a7 b3bcde59 7c520607
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #48 jcstein-46639943:
		7eaff9a3 aa58d49c 4017e853 110acb3f
		15759216 211c2878 5a6aca05 c0da1952
		6aeb2fb8 795b2c97 350c0f2a c01eea02
		92779f83 79f40282 547f1bf2 9e0b1a8b
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #47 pdg744-15272444:
		46ca93f4 aa37dc42 19c0df1a c4e89e7f
		b02b2a2c f308ccb6 22daf08d 25a1e1fc
		98f956ff f2a1a0bb 81a76b64 372f31f9
		88458a27 5b0f41b2 c546f727 ef2dc739
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #46 cetin-6861974:
		dca2d769 022c4887 52fc977d 739829cf
		5fcb6e85 ac5dd944 a370035f 03d07c77
		6c83b20a 428f0d45 d5445c62 92392e50
		c436a9e2 3118a202 7bcf767e 90b5f1bb
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #45 curlycrypto184-57609195:
		260ef8ab 5de96cc4 d41b6fbd 099ef32c
		c1a3e20b 98c3c4c2 94f9b700 505ef5a0
		5f9dd388 dd35964f 8503bd72 c64706f2
		73be465c 82859793 e9638c1d 75294f97
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #44 midlink-72997225:
		edc01b44 1ee1a69d a487f7fb 1ea59717
		74980d92 a02e7034 548420a4 2ff92c96
		c0ae1505 3df2c7b7 9622dca1 7e2a1896
		e445e854 9ea6e0e8 3c65cbab aa338cad
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #43 Chinacolt-8400905:
		95879300 d6b33f11 8519972e ec94e066
		2536dcc9 b317570e ae714bee d99e1506
		eb8b0aa5 15bd79ec bd18e493 c294e370
		1c37e87d edf09fa5 7c656ebe 29283124
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #42 liray-unendlich-15893314:
		05e4202c 7d0e3615 89eab254 8c2502eb
		08d6a6d1 16211864 a6359b03 2997ead9
		34b56ac6 6864cfe7 55e90d7d 83382899
		36a56694 cc761882 4b2143fa 28aafd95
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #41 0xinit-28729137:
		29d1b2cc 3bf48957 85735bbd fcf9c5c1
		56c6db73 76ff9259 cb790b2d 8b3ba845
		b9d6ca7f 52b1b1a9 7f2672cc f1cd9988
		5dd82bcb 26c57094 091fa24b 234c6b5d
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #40 JacobEverly-112036223:
		d545ed4c 162af4f2 818c5503 4024cd1d
		78791d5c aacb8b59 a9c3dd6b bd1e12ba
		eb04a211 3a283c1a aabe1a99 b33c732b
		4462ac79 cbe28486 3cf24acc 2ed8f552
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #39 hashbender-561911:
		df055246 621d86e7 edb0a9dd 682d018e
		97ff5ee9 9096193e f9814cd3 e43974e2
		0998cbf4 bac70f8a e701f09f 9e08ad62
		85bf7056 3f506cde 7bb4d55f d0ece990
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #38 tugbamaden-233120553:
		e160fdd2 36d19e8c 15a7a09d 3c4935fb
		da6e826d 5a450a30 9d219900 c0797798
		079ccc38 4557815b 745f9719 3f92c3a2
		1a78a47d b0d55611 582d8176 d614bda3
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #37 MrMikol-60373935:
		72053465 da6e4838 46d59df0 2a194c3c
		421ed8e6 03078c67 af317dab 0786f030
		7b285748 51efdb69 895a8d8d 53bcf2e7
		ea161ca6 698660ab 5b067401 3fdb0133
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #36 atacann-111396231:
		ccbfbe6a 80787018 4f327f83 8ba03239
		bfc86db5 a01c72a7 fbbaba9b 211e7503
		252233c6 ce2af240 c85e916b 34da755e
		fb77ad58 255c87fa a7c8701b dde6f09f
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #35 uok825-30076213:
		2e7a6aef 84be1070 fd080636 37abc101
		afd546d7 df541d57 12897180 7c58091f
		6a7360fa b5f78238 cf455979 747ffa0a
		0da66127 5ca16a75 27ee89a2 c3a9fa89
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #34 rchristensen-516785:
		78f8a36e a7f59ccd d3c1b11d 53451613
		d88aee47 8053a95d 11d2d650 7673e428
		acf186aa db87f522 aae04bc5 78f82083
		31af5a5c 1cceda21 f9dc4d42 b0a09775
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #33 eigmax-81497928:
		eca0ebe0 096894b9 7527a641 fe9a75b1
		5c6e7570 a73dc80c 79209b03 4755eb63
		e3fe4d84 7c831731 ddf0e008 9a6dc1a7
		d15f4783 585e653b 87aedbb7 844fba3d
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #32 rakanalh-195829:
		525bf4c8 beeb3125 a6c132c5 e76d84fc
		b3226344 f05a5494 8bad922b b3f9eac3
		3be5178a 17bcb127 9076432c 8503b11b
		670abb12 2d4982e1 922f3462 3f9b9c53
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #31 RedVelvetZip-48105289:
		0e59b470 c60e7268 bd8d737f 5d53e684
		7907909a 6d6aacd2 6a6d15a6 d0819694
		562355ad 8ccc59cd 49265c40 0556fdb0
		f99cade8 7b8bda37 561ef0f2 0704739e
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #30 just-erray-29013191:
		a0fe3316 3609d18f 865de7f4 ae1dc67c
		bbb35a0e ce2339e5 a062b8e4 51bbb914
		75464e31 8e459989 fc5f314e ba5894d5
		d0cf85ca aadf7c3f 80b5b0ae 2e0da488
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #29 stskeeps-1255200:
		7b88f346 6b35de77 b9c000d6 7da4a664
		091b0c28 13087851 f7f87654 dd2109b4
		11ed70b1 13ed47d3 844ffb7b 1cdc4102
		8e458002 80c584ec acd78445 1e0969fb
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #28 muhylmz-179225574:
		d260d01b c3456e1f 3f30e6b6 83605532
		d75405c4 40ed14b0 21d89bf5 55604a9f
		c6feb4bc 586ddfa4 c07b73e6 550844e7
		46f1cb0d a9699382 18fc1ea2 11d4b6f6
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #27 selinisik-56879777:
		1ceecb6e 09229b43 40c602a5 df694010
		78f27bf2 07ad0a80 994a7d6e fdf44dd8
		051180cf 819bed07 ee15917b e5a554e5
		0bb377d0 75372934 0fa6dc35 bbb836fd
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #26 okkothejawa-103260942:
		24d0b071 02e3c7bf 90b860cb e6de81fa
		d7659b3d a17aad96 aaca44b6 d0663e1e
		febc31db a308410f 5d2eaaca 79bbcc58
		aac76632 6eee869e d745df94 70d595a2
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #25 exeokan-35339130:
		6a581e49 c9338a70 fc04dc25 aeb480c1
		d05555e6 6a7a5416 1f36ac17 12c6947f
		ac4e2f43 24459161 5043730d f40f7c6f
		1eb1713f 55657f41 3d9e6c10 792fc4c1
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #24 bryanchainway-226732420:
		dc9e7983 56096fad 14f28bb9 1c2be575
		5109e1f3 e8f0b1b9 d02ae9ef 68f27c57
		b42c9232 6782fd6d 9d1b8f2a f1135b61
		b93025e0 eb33273e 60f82648 a511c220
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #23 omerfaunal-56688420:
		ada5eb1e 5b7a2716 fe0f146b 1d5e8a1e
		34d63ac3 7326a8ea 049c55a6 865a8adc
		1c4f0c03 169a23f7 54e42e2c 11c09f41
		52d13d0c b54c0f92 fe391a52 ee9c19d3
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #22 jason-chew-54255657:
		69396d58 c15a5e86 8f724152 f7656e73
		64d469a1 e1f44908 84abac87 e4598564
		a83463ea 8fcbdfd4 c2a72b6a 1d7fbb88
		18d5d1c5 7220480f 30f9aa1f f2709ec7
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #21 mmtftr-13402668:
		2cd40ab8 bdb608cb d2bae6b6 485adbfe
		8453aba1 1dd85036 578c189f adcd30a1
		c8196e4c acfc7e0e df4c5132 a6cfae6f
		294fd633 dcbba0c2 46e1c975 20341766
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #20 ozankaymak-92448699:
		d41c48c3 e39d3355 97e7d931 6fd0fae7
		52e8042d 4c26edfe deb6a8af bb8da0e0
		04403a92 678e4212 07ef4ca3 d3514743
		7f06ec10 a3831c59 751fd867 138b3f5b
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #19 ercecan-47954181:
		19c8d89d e2ae3b50 0a0f6c27 0512f9a6
		629bab8d cfc226c9 4e90b734 d5496d7f
		4ce524e2 e4cf8861 d63bdaee 52f0fc71
		310c5930 5710400e 8d5d839a 64e254bf
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #18 ec2-6364934:
		85feb15f 1c5428dd dbc3858b 7933fe8c
		cf84e6d1 21e25c82 2b11d817 db0de158
		3528f38c 1c1eebda ea34595a 99d3ebda
		19c3353c 1eb984d0 735ef7f7 42f5002e
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #17 muratkun-848258:
		a8c2ff13 1f58c940 866375b0 ff10c9d9
		5395871f 20497437 e729e763 20236869
		6fb83691 85eba4a9 5ca51060 ffb2a5e9
		80d6f881 76dac451 8858596c ae66a72f
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #16 nud3l-14966470:
		aea2a312 f6390bf8 ab1a3f1f 3d032c4b
		8a8d830a cfee332f 2ce57d22 5b8de8a1
		76e2a33d 5b30b686 ec69909f f570645b
		418af06d 781f3fdb 2c46bf0c 0aaa2a64
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #15 lukechilds-2123375:
		9ec2d2bd 4881ff17 d95a113b a26f17a3
		f8043a64 fed1a53d b4c50bdc fe022c55
		bc5da69e b3d6f697 94b3fd03 ba69d77e
		d012724f 1e3be2b8 729b1a35 3dc86aff
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #14 sevkett12-183107860:
		11900e80 68638dd7 77052abc a9ada7ff
		7ce1eb42 bb8ed40c 49fd38f6 81c97048
		20860fb0 b21279e7 39e9b291 54b96aac
		5d8a5584 238d6731 a0d84ff2 2f6e0bf6
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #13 willemolding-6880154:
		fc0ea637 a3064b9b 2e787836 d03a4d33
		441a1049 c0ee503d b2bad3d1 829347da
		b4f9833f 4237aca7 6c21c2a4 5c854caf
		dce8b9f5 50e6649c a1ce18ff 26408690
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #12 Caglankaan-33874042:
		552b3bbe d2cfac91 b6f565d8 9326fb28
		d2284a6f 9e10c4e2 5e7f531f 3ece628c
		b3437362 9e6d8682 c4ed8c9e 9682a8fe
		4e4a5ef3 6739ec02 94cfd243 a775febd
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #11 eyusufatik-19624533:
		5f061ff8 7fc7af96 c35e54b4 a83955ab
		d544d36c 2d3ab51e 63dbc211 83595b78
		97262569 efa45c87 8c713460 df8e03ef
		ef192dde ee70403e a0661329 3c7eca24
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #10 Hakkush-07-68596831:
		c2f08f31 3d0fdcd6 21ad9db8 fd3762e2
		c6848850 7f0a24d1 6d8fa37c 9dea681e
		0ac36ee8 b046192e 67c1e175 fea2c3c5
		bdf919bc 6f5b4b9d 6283484e c526bed9
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #9 weikengchen-14937807:
		539e4817 122bedc6 86e76090 533491a4
		3b02578d 80af1cbd 61ec88fc 95d308f3
		93a2af9c 1d9f1f53 eaf43fdb 6d48818a
		eb22b359 3827a5d1 4c75bfa3 702b08bc
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #8 orkunkilic-29154729:
		5545ebf3 fd8d30a0 12c2d921 7763b761
		b65d8444 def10b5e 18abd661 4d32f189
		3fbccc82 bedc27d0 04f50db0 575a49a9
		f04ebf7e d65df002 d7651ec6 1bf1a677
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #7 aoengin-85455415:
		0136b081 aeb9a6d9 3801a6e9 ef54309c
		e7c2caef e62489f4 1c67fe1a 0d9ffab3
		3879372d a8f11d3e 2ac9793e 3e860f6d
		d11ae812 b3ca89c1 604dfdd3 89ca8f93
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #6 erdkocak-93616117:
		40e15450 4854d0d9 09585052 c5ca921d
		a35cdabf 5f3d72d5 ec4fb300 20099765
		37b41969 5d8b0145 6c68436d 601ba5d9
		4b4127da a68734b5 3e9c73d4 3cdec188
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #5 ceyhunsen-47274673:
		87caff1c 1bfed23e b55375f2 2bc0a974
		edd5a2e1 755eabec a650b65e 7190092d
		12e75fd8 1bd061e6 ed185f9e 8f7654ba
		bcad4998 45785f3d 77467d4c f96d9e52
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #4 bmar-design-179503808:
		e3d6897c 7afa1374 bfd1d8bc abdf81e5
		266dff34 db96d271 2f80054b 5ddd61db
		a17c3704 2d988eeb a3cbfac2 b5b03796
		97674657 857a151e f701ad0d 97c3bab4
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #3 otaliptus-56600661:
		8c0406d4 697c6689 e04f080b 2b103bcd
		9c774d84 730bdf4d 2359c4da d362bffb
		597cb6ab 60e93fef e544eb7f 8e6f53a3
		c448793f a034dc6e 4510afab 75549922
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #2 bijanshahrokhi-26103938:
		6bb286a6 30beac16 776d49e3 a7d009e7
		ad6d5b0e 8435f6e0 8d323baa b19704f4
		9d7f632a faca8414 85587d9e 0d25be3d
		d8045ddd 3a3d440e c1314c33 1bffeb52
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: contribution #1 ekrembal-29926881:
		df1fc086 ed5a319b 26229134 179642af
		8e6ee7d9 2e73755b 03bb71ca 44a337b8
		442dcf19 6b8a84f4 852412ec da6176a3
		8aa56c5e b5e3d675 3e8ae8bc ba52ce17
[INFO]  snarkJS: -------------------------
[INFO]  snarkJS: ZKey Ok!
```
