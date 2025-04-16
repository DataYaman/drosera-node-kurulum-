
# Drosera Operator Node Kurulum Rehberi

> Bu rehberde Drosera testnet’ine nasıl katılabileceğini, trap kurup operator node çalıştırmayı adım adım ve açıklamalı olarak gösteriyorum.

---

## 1. Sistem Gereksinimi

- Ubuntu 20.04/22.04
- 2 CPU, 4 GB RAM, 20 GB disk alanı

---

## 2. Güncelleme & Gerekli Araçlar

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl ufw git wget nano build-essential -y
```

---

## 3. Docker Kurulumu

```bash
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

---

## 4. CLI Araçları

```bash
# Drosera CLI
curl -L https://app.drosera.io/install | bash
source ~/.bashrc && droseraup

# Foundry CLI
curl -L https://foundry.paradigm.xyz | bash
source ~/.bashrc && foundryup

# Bun
curl -fsSL https://bun.sh/install | bash
```

---

## 5. Trap Oluşturma

```bash
mkdir my-trap && cd my-trap
git config --global user.email "mail"
git config --global user.name "kullanici"
forge init -t drosera-network/trap-foundry-template
bun install && forge build
DROSERA_PRIVATE_KEY=PRIVATEKEY drosera apply
```

---

## 6. Trap’e Operator Ekleme (Whitelist)

```bash
nano drosera.toml
```

En alta şunu ekle:

```toml
private_trap = true
whitelist = ["0xSENIN_OPERATOR_ADRESIN"]
```

Uygula:

```bash
DROSERA_PRIVATE_KEY=PRIVATEKEY drosera apply
```

---

## 7. Operator CLI & Kayıt

```bash
cd ~
curl -LO https://github.com/drosera-network/releases/releases/download/v1.16.2/drosera-operator-v1.16.2-x86_64-unknown-linux-gnu.tar.gz
tar -xvf drosera-operator*.tar.gz
sudo cp drosera-operator /usr/bin
drosera-operator register --eth-rpc-url https://ethereum-holesky-rpc.publicnode.com --eth-private-key PRIVATEKEY
```

---

## 8. Operator Node’u Çalıştır (Docker ile)

```bash
git clone https://github.com/0xmoei/Drosera-Network && cd Drosera-Network
cp .env.example .env && nano .env
```

`.env` dosyasına `PRIVATEKEY` ve `VPS_IP` gir.

```bash
docker compose up -d
docker compose logs -f
```

---

## 9. Dashboard'dan Trap'e Bağla

- [https://app.drosera.io](https://app.drosera.io) adresine git
- Cüzdanını bağla
- “Traps Owned” altında trap'ini bul
- “Opt-in” ile operator’ü trap’e bağla

---

## 10. Hepsi Bu Kadar

Node aktif çalışıyorsa, bloklar yeşil olarak görünür.  
Sorun yaşarsan `logs -f` ile kontrol et veya bana ulaşabilirsin.
