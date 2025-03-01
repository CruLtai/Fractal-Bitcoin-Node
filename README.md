# Fractal-Bitcoin-Node
 ## Fractal Bitcoin Node Kurulum Rehberi

Binance tarafından desteklenen ve Eylül ayında tokeni çıkacak olan [Fractal Bitcoin](https://www.fractalbitcoin.io/) projesinde bir node kuracağız. 

Node kurulum süreci ve ayrıntılı rehberi aşağıda bulabilirsiniz.

![image](https://github.com/user-attachments/assets/7e9c98e5-d1e9-4a4f-a59f-4997a0e86d8e)

## Fractal Bitcoin Nedir?

Fractal Bitcoin, sonsuz katmanları yinelemeli olarak ölçeklendirmek için Bitcoin Core kodunu kullanan tek Bitcoin ölçeklendirme çözümüdür. Bu, Bitcoin'e uygulanan dünyadaki ilk sanallaştırma yöntemidir. Fractal, Bitcoin blok zincirini, Bitcoin ana zincirindeki tutarlılığı bozmadan ölçeklenebilir bir bilgi işlem sistemine kademeli olarak genişletir. Güçlü araçlar ve destek ile Fractal üzerine inşa etmek oldukça basittir.

### Gereksinimler

| Gereksinim                   | Açıklama                            |
|------------------------------|-------------------------------------|
| İşletim Sistemi              | Ubuntu 22.04 veya daha yeni bir sürüm |
| Bellek (RAM)                 | En az 4 GB                          |
| Disk Alanı                   | En az 100 GB boş disk alanı         |

## Kurulum

1. **Paketlerin Kurulumu:**

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl build-essential pkg-config libssl-dev git wget jq make gcc chrony -y
```

2. **Fractal Reposunu Çekme:**

```bash
git clone https://github.com/fractal-bitcoin/fractald-release.git
```

3. **Dizine Gitme:**

```bash
cd /root/fractald-release/fractald-x86_64-linux-gnu
```

4. **Data Klasörünü Oluşturma:**

```bash
mkdir data
```

5. **Konfigürasyon Dosyasını Kopyalama:**

```bash
cp ./bitcoin.conf ./data
```

6. **Servis Oluşturma:**

```bash
tee /etc/systemd/system/fractald.service > /dev/null <<EOF
[Unit]
Description=Fractal Node
After=network.target
[Service]
User=root
ExecStart=/root/fractald-release/fractald-x86_64-linux-gnu/bin/bitcoind -datadir=/root/fractald-release/fractald-x86_64-linux-gnu/data/ -maxtipage=504576000
Restart=always
RestartSec=3
LimitNOFILE=infinity
[Install]
WantedBy=multi-user.target
EOF
```

```bash
sudo systemctl daemon-reload && \
sudo systemctl enable fractald && \
sudo systemctl start fractald
```
Blokları https://mempool.fractalbitcoin.io/ burdan kontrol edebilirsiniz .

**Log Kontrol**
```
sudo journalctl -u fractald -fo cat
```

### 7. Cüzdan Oluşturma

#### 7.1: Cüzdan Adını Belirleyin

Öncelikle, oluşturmak istediğiniz cüzdan için bir ad belirleyin ve bu adı bir değişkene atayın:

```shell
CUZDAN="cuzdan_adiniz"
```
> Not: `cuzdan_adiniz` kısmını, oluşturmak istediğiniz cüzdanın adıyla değiştirin.

#### 7.2: Cüzdan Oluşturma Komutlarını Çalıştırın

Terminalde aşağıdaki komutları sırasıyla çalıştırarak cüzdan oluşturun:

```shell
cd /root/fractald-release/fractald-x86_64-linux-gnu/bin
./bitcoin-wallet -wallet="$CUZDAN" -legacy create
```
Bu adımlar sonucunda, belirlediğiniz isimde yeni bir cüzdan oluşturmuş olacaksınız.

![Ekran Resmi 2024-07-15 22 51 53](https://github.com/user-attachments/assets/e3abaf80-6ee2-4ae5-8fc4-dd6debe75819)

8. **Cüzdan Private Key Alma:**
> Aşağıdaki komutla private keyinizi öğrenebilirsiniz. Komutta $CUZDAN yazan yerlere bi önceki komutta oluşturduğunuz cüzdan ismini yazarak değiştirin.
Tek komut olarak yapıştıralım.

```shell
cd /root/fractald-release/fractald-x86_64-linux-gnu/bin
./bitcoin-wallet -wallet=/root/.bitcoin/wallets/$CUZDAN/wallet.dat -dumpfile=/root/.bitcoin/wallets/$CUZDAN/MyPK.dat dump
cd
cat .bitcoin/wallets/$CUZDAN/MyPK.dat
```

BU KOMUTTAN SONRA loglar akacak ve durduğunda checksum,********  böyle yazacak , den sonrakiler sizin private keyiniz hayırlı olsun UNISAT a import etmeyi bilmeyen yoktur diye anlatmıyorum...


* Fractal Explorer: [https://explorer.fractalbitcoin.io/](https://explorer.fractalbitcoin.io/)

**[Beni X'te takip etmeyi unutmayın](https://x.com/neona_te)**
