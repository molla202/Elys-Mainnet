<h1 align="center"> 0G

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/9d9b2b64-736b-4921-aa50-ae87f6d8d34b)


</h1>


 * [Topluluk kanalımız](https://t.me/corenodechat)<br>
 * [Topluluk Twitter](https://twitter.com/corenodeHQ)<br>
 * [Discord](https://discord.com/invite/0glabs)<br>



## 💻 Sistem Gereksinimleri
| Bileşenler | Minimum Gereksinimler | 
| ------------ | ------------ |
| CPU |	16|
| RAM	| 32+ GB |
| Storage	| 400 GB SSD |

### Public RPC

SOON....

### 🚧Gerekli kurulumlar
```
sudo apt update && sudo apt upgrade -y
sudo apt install curl git wget htop tmux build-essential jq make lz4 gcc unzip gcc clang cmake build-essential -y
```

### 🚧 Go kurulumu
```
cd $HOME
VER="1.22.3"
wget "https://golang.org/dl/go$VER.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
rm "go$VER.linux-amd64.tar.gz"
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
```

### 🚧Dosyaları çekelim
```
git clone https://github.com/elys-network/elys.git
cd elys
git checkout v1.0.0
```
```
mkdir -p $HOME/.elys/cosmovisor/genesis/bin
mv /root/elys/build/elysd $HOME/.elys/cosmovisor/genesis/bin/elysd
```
### 🚧System link
```
sudo ln -s $HOME/.elys/cosmovisor/genesis $HOME/.elys/cosmovisor/current -f
sudo ln -s $HOME/.elys/cosmovisor/current/bin/elysd /usr/local/bin/elysd -f
```
### 🚧Cosmovisor indirelim
```
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.5.0
```
### 🚧Servis oluşturalım
```
sudo tee /etc/systemd/system/elysd.service > /dev/null << EOF
[Unit]
Description=elys node service
After=network-online.target

[Service]
User=root
ExecStart=/root/go/bin/cosmovisor run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=/root/.elys"
Environment="DAEMON_NAME=elysd"
Environment="UNSAFE_SKIP_BACKUP=true"
Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/root/.elys/cosmovisor/current/bin"

[Install]
WantedBy=multi-user.target

EOF
```
```
sudo systemctl daemon-reload
sudo systemctl enable elysd.service
```
### 🚧İnit
NOT: node adınızı yazınız.
```
elysd init NODE-ADI-YAZ --chain-id elys-1
```
### 🚧Genesis addrbook
```
curl -o $HOME/.elys/config/genesis.json https://raw.githubusercontent.com/elys-network/networks/refs/heads/main/mainnet/genesis.json
```

### 🚧Gas pruning ayarı
```
...
```
### indexer null
```
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.elys/config/config.toml
```
### 🚧Port Ayarları
```
echo "export G_PORT="15"" >> $HOME/.bash_profile
source $HOME/.bash_profile
```
```
sed -i.bak -e "s%:1317%:${G_PORT}317%g;
s%:8080%:${G_PORT}080%g;
s%:9090%:${G_PORT}090%g;
s%:9091%:${G_PORT}091%g;
s%:8545%:${G_PORT}545%g;
s%:8546%:${G_PORT}546%g;
s%:6065%:${G_PORT}065%g" $HOME/.elys/config/app.toml
```
```
sed -i.bak -e "s%:26658%:${G_PORT}658%g;
s%:26657%:${G_PORT}657%g;
s%:6060%:${G_PORT}060%g;
s%:26656%:${G_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${G_PORT}656\"%;
s%:26660%:${G_PORT}660%g" $HOME/.elys/config/config.toml
```
### 🚧Seed
```
PEERS="637077d431f618181597706810a65c826524fd74@148.251.68.103:22056,b279780f951eaf05d285a19e080f72fbb85eec11@88.218.224.57:26656"
SEEDS="" && \
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.elys/config/config.toml
```
### 🚧Snap
```
SOON
```
### 🚧Başlatalım   
```
sudo systemctl daemon-reload
sudo systemctl restart 0gchaind
```
### 🚧Log
```
sudo journalctl -u elysd.service -f --no-hostname -o cat
```
### 🚧Cüzdan oluşturma
NOT: cüzdan adınızı yazınız
```
elysd keys add cuzdan-adini-yaz
```
- Eski cüzdan import ederkene bele
```
elysd keys add wallet --recover
```

### 🚧Validator oluşturma






