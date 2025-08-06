<h1 align="center"> Lumera




![image](https://github.com/user-attachments/assets/291dc80d-e97a-4941-b9be-ac0c39d2278c)

</h1>

 * [Topluluk kanalımız](https://t.me/corenodechat)<br>
 * [Topluluk Twitter](https://twitter.com/corenodeHQ)<br>


### Public RPC



### Explorer

### Faucet

https://faucet.testnet.lumera.io/

## 💻 Sistem Gereksinimleri
| Bileşenler | Minimum Gereksinimler | 
| ------------ | ------------ |
| CPU |	4|
| RAM	| 8+ GB |
| Storage	| 400 GB SSD |




### 🚧Gerekli kurulumlar
```
sudo apt update && sudo apt upgrade -y
sudo apt install curl git wget htop tmux build-essential jq make lz4 gcc unzip -y
```

### 🚧Go kurulumu
```
ver="1.22.5"
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
source ~/.bash_profile
go version
```
### 🚧Dosyaları çekelim
```
cd $HOME
wget https://github.com/LumeraProtocol/lumera/releases/download/v1.7.0/lumera_v1.7.0_linux_amd64.tar.gz
tar xzvf lumera_v1.7.0_linux_amd64.tar.gz
chmod +x lumerad
```
```
cd
mkdir -p $HOME/.lumera/cosmovisor/upgrades/v1.7.0/bin
sudo mv $HOME/lumerad $HOME/.lumera/cosmovisor/upgrades/v1.7.0/bin/lumerad
mv $HOME/libwasmvm.x86_64.so $HOME/.lumera/

```
```
echo 'export LD_LIBRARY_PATH=$HOME/.lumera:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```
```
sudo ln -sfn $HOME/.lumera/cosmovisor/upgrades/v1.7.0 $HOME/.lumera/cosmovisor/current
sudo ln -sfn $HOME/.lumera/cosmovisor/current/bin/lumerad /usr/local/bin/lumerad
```
### 🚧Cosmovisor kuralım
```
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.6.0
```
### 🚧Servis oluşturalım
```
sudo tee /etc/systemd/system/lumerad.service > /dev/null << EOF
[Unit]
Description=lumerad node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which cosmovisor) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.lumera"
Environment="DAEMON_NAME=lumerad"
Environment="UNSAFE_SKIP_BACKUP=true"
Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:$HOME/.lumera/cosmovisor/current/bin"
Environment="LD_LIBRARY_PATH=$HOME/.lumera"

[Install]
WantedBy=multi-user.target
EOF
```
### 🚧Etkinleştirelim
```
sudo systemctl daemon-reload
sudo systemctl enable lumerad
```
### 🚧İnit
```
lumerad init "MONIKER" --chain-id testnet-2
```
### 🚧Genesis ve addrbook
```
wget -O $HOME/.lumera/config/genesis.json https://raw.githubusercontent.com/Core-Node-Team/Lumera-Tesnet-2/refs/heads/main/genesis.json
wget -O $HOME/.lumera/config/addrbook.json https://raw.githubusercontent.com/Core-Node-Team/Lumera-Tesnet-2/refs/heads/main/addrbook.json
```
### 🚧Port
```
echo "export LUM_PORT="29"" >> $HOME/.bash_profile
source $HOME/.bash_profile
```
```
sed -i.bak -e "s%:1317%:${LUM_PORT}317%g;
s%:8080%:${LUM_PORT}080%g;
s%:9090%:${LUM_PORT}090%g;
s%:9091%:${LUM_PORT}091%g;
s%:8545%:${LUM_PORT}545%g;
s%:8546%:${LUM_PORT}546%g;
s%:6065%:${LUM_PORT}065%g" $HOME/.lumera/config/app.toml
```
### Port
```
sed -i.bak -e "s%:26658%:${LUM_PORT}658%g;
s%:26657%:${LUM_PORT}657%g;
s%:6060%:${LUM_PORT}060%g;
s%:26656%:${LUM_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${LUM_PORT}656\"%;
s%:26660%:${LUM_PORT}660%g" $HOME/.lumera/config/config.toml
```
### 🚧Seed ve Peer
```
SEEDS=""
PEERS="655e83ae60def1a4f435bc31e759562d419c18ac@195.26.251.0:26656,39e73a3566f9e9751aed23d56a40945b9daedb26@65.109.18.91:52656,345a910211c7eb5ae5d124683f24c25d54db767d@148.251.86.17:30756,a9f794c75040dc87973844c9f46d0e632a2b8539@38.242.201.82:13656,3b78b08bd9d9d0a2b17a944241a849ce04d8607e@44.204.100.172:26656,c2ae41bf57e1a5b56229846b1b113d46f3206bf3@44.193.206.52:26656,a416a81715da4ffde5abb492a7c4ac17c887ae37@167.235.178.134:30756,647d70baa4da4295cbd56aadd6e69be20aa1592e@213.199.63.152:16656,faff7c1350468c53121a669ac40e317a4a70c425@44.198.60.93:26656,4ce6b843ad54e727f72c5df85ee21b0611ee40e0@149.50.96.14:20656,5a269959ca845031781fa3f17b36ec0bc592e748@37.27.71.199:11656,9ffe7d48c77e175ecdde07e70f95c3056adf9d9d@65.21.220.178:26656,dbb496657b2cc3f90a282b7c7cecc1fd901f4f2b@84.247.191.181:13656,febc314fa60d99c62c735a32e40f701d41dcadab@209.126.12.85:16656,05df2ce27cd2f2a5c7ecc8ec4d78c9243e39444b@3.236.181.141:26656,317e36b7d37236c45572311b51d8209ca8e9f747@38.242.154.120:26656,a7502553e765123aede6cbb4f8040ccc1266a971@62.169.16.5:13656,2f68418ad7d6383a8caeb91cdf4b45c56d8ef55f@65.21.221.110:28656,228396bce456ee8dad81adf9db2fd1449d9e2a49@144.217.68.182:21956,ddd99cbdfc60db3cb12618f36364791b663c7904@157.180.8.121:26656,9ae8787e6519141369b8858f6c240336767c1f7c@95.217.76.186:26656,c87f0e41d1c5a64cc39e212972a864dd40a08da5@157.180.3.36:30656,511c3f3c82a2d04aa5bcdc91907b98eb3709f8d8@152.53.254.201:30656,71caa9ff25fc96988d24a1bdda8aee8391821ed0@95.214.55.5:40656,562a569597e5ac818df95257a7fe9b8e8dbbbc2d@138.201.17.210:26656,ab93e7e37ab9ca11930fda95ad9c5c514996ada9@62.169.16.14:13656,a0b137ce7a1c18dc8c840aa4067ad25c6f902482@89.250.74.164:26656,3852f4b2f4d774b4ae0fe8d90a5c863933e7e99d@149.102.142.113:26656,aeb7867b6ac0df3aa99f1a1265b50b0331d5415d@160.250.106.34:30756,046d22414c39347932c978a5c7f5c2ad02876ac0@195.26.253.188:10656,2c3fd4a39c4fedb1a4076a86c895a160f279e6ea@154.38.175.225:17656,2a45ee3dce194119acec8c11a5aaabf7640c42c7@31.220.80.77:13656,650486c12724eda778c8fa703de1de28b8947549@156.67.29.226:13656,21b3817a6b18b0ffc3773d39fe78f1f6fe1d2dd0@209.126.12.240:17656,b54251b6999fb9a7c19376c65643b3ecca3ce9da@209.126.11.54:13656,51bc2f17490409b3ca4b565912acd1c4d46598aa@89.117.72.209:13656,0d6891b0d8321a47d68ad459be5df93ec019e6da@62.169.16.79:18656,29ddbf16cfe7460bfa6a0c7e78537856363b6c32@65.108.129.163:29656,874e39efd7c31c6546350cd8c164c857ab965349@89.147.102.206:26656,d6238afe24aa289094556668914efb4fc3e74ba2@195.26.250.90:13656,7dab84e313053596727f5c5a83115ea29cde25b4@207.244.245.246:13656,dd7ee8d06bfcd4e7a89e8e6f7bdb46467c9a589b@209.126.12.242:13656,2e6980c58f576e6faa3a4fb3fbfdc6b9129234fa@149.86.227.209:28656,39b84ac1ec66983759dc39ab5824a162c9b1168e@188.40.66.173:30756,6d0380e87d7a4ed9d522009e906772c35bccc29a@144.91.69.120:28656,b5f306e5e0ba6b7b3c31459de32188e45deb4b4c@195.3.223.73:28656,07b7da71906e918552e3449408d36d702e941085@152.53.104.182:17656,08d1c0d9c7a184c642d3cbc09fea615906e5ac6e@135.181.181.59:28656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.lumera/config/config.toml
```

### 🚧Pruning
```
sed -i -e "s/^pruning *=.*/pruning = \"nothing\"/" $HOME/.lumera/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.lumera/config/app.toml
sed -i 's|^indexer *=.*|indexer = "null"|' $HOME/.lumera/config/config.toml
```
### 🚧Gas ve index ayarı
```
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.025ulume\"/" $HOME/.lumera/config/app.toml
```
### Snap 
YAPMA YAPMA YAPMAAAAA
```
lumerad tendermint unsafe-reset-all --home $HOME/.lumera
curl -L http://37.120.189.81/lumera_testnet/lumera_snap.tar.lz4 | tar -I lz4 -xf - -C $HOME/.lumera
```
### 🚧Başlatalım
NOT: buraya geldiysen dur önce gentx işlemlerini yap en alttan. başlatma da sonra sakın
```
sudo systemctl daemon-reload && sudo systemctl start lumerad && sudo journalctl -u lumerad -f --no-hostname -o cat
```
### Log
```
sudo journalctl -u lumerad -f --no-hostname -o cat
```
### Cüzdan olusturma
```
lumerad keys add cüzdan-adi-yaz
```
### Cüzdan import
```
lumerad keys add cüzdan-adi-yaz --recover
```
### Validator oluştur
```
cd $HOME
```
```
echo "{\"pubkey\":{\"@type\":\"/cosmos.crypto.ed25519.PubKey\",\"key\":\"$(lumerad comet show-validator | grep -Po '\"key\":\s*\"\K[^"]*')\"},
    \"amount\": \"1000000ulume\",
    \"moniker\": \"test\",
    \"identity\": \"AAA\",
    \"website\": \"AAA\",
    \"security\": \"AAA\",
    \"details\": \"AAA\",
    \"commission-rate\": \"0.1\",
    \"commission-max-rate\": \"0.2\",
    \"commission-max-change-rate\": \"0.01\",
    \"min-self-delegation\": \"1\"
}" > validatorlu.json
```

```
lumerad tx staking create-validator $HOME/.lumera/validatorlu.json \
--from wallet \
--chain-id testnet-2 \
--gas-prices=0.025ulume \
--gas-adjustment=1.5 \
--gas=auto 
```

### unjail
```
lumerad tx slashing unjail --from wallet --chain-id testnet-2 --gas-prices=0.025ulume --gas-adjustment=1.5 --gas=auto
```
### kendine stake
```
lumerad tx staking delegate $(lumerad keys show wallet --bech val -a) 1000000ulume --from wallet --chain-id testnet-2 --gas-prices=0.025ulume --gas-adjustment=1.5 --gas=auto
```

### Delete node
```
sudo systemctl stop lumerad
sudo systemctl disable lumerad
sudo rm /etc/systemd/system/lumerad.service
sudo systemctl daemon-reload
rm -f $(which lumerad)
rm -rf .lumera
```
