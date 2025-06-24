# Nexus-phase-3-node-setup-commands

## Web node 

```
#!/bin/bash

# === CONFIGURE YOUR PROXY AND TIMEZONE HERE ===
TIMEZONE="Asia/Kolkata"
PROXY_HOST="your.proxy.iproyal.com"
PROXY_PORT="1234"
PROXY_USER="yourproxyusername"
PROXY_PASS="yourproxypassword"
VNC_PASSWORD="changeme"  # set your VNC password

# === INSTALL DOCKER ===
sudo apt update && sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update && sudo apt install -y docker-ce docker-ce-cli containerd.io

# === SET TIMEZONE ===
sudo timedatectl set-timezone "$TIMEZONE"

# === PULL VNC + CHROMIUM IMAGE ===
docker pull consol/ubuntu-xfce-vnc

# === RUN DOCKER CONTAINER WITH PROXY AND VNC ===
docker run -d \
  --name chrome-vnc \
  -e HTTP_PROXY="http://$PROXY_USER:$PROXY_PASS@$PROXY_HOST:$PROXY_PORT" \
  -e HTTPS_PROXY="http://$PROXY_USER:$PROXY_PASS@$PROXY_HOST:$PROXY_PORT" \
  -e VNC_PASSWORD="$VNC_PASSWORD" \
  -p 5901:5901 \
  consol/ubuntu-xfce-vnc

echo "✅ Setup Complete"
echo "➡️  Connect to your VPS IP at port 5901 using RealVNC with password: $VNC_PASSWORD"
```

## CLI node 

```
# 1. Install dependencies
echo "📦 Installing system dependencies..."
sudo apt update && sudo apt install -y \
  curl build-essential pkg-config libssl-dev \
  protobuf-compiler screen git-all

# 2. Install Rust and add RISC-V target
echo "🛠️ Installing Rust toolchain..."
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
source "$HOME/.cargo/env"
rustup target add riscv32i-unknown-none-elf

# 3. Create and run in screen session
echo "📟 Launching Nexus Prover in screen session: $SCREEN_NAME..."
screen -dmS "$SCREEN_NAME" bash -c '
  echo "🚀 Installing Nexus CLI..."
  curl https://cli.nexus.xyz/ | sh

  echo "📌 IMPORTANT: After this finishes, choose option 2 to link your node with the Node ID from https://app.nexus.xyz/nodes"
  exec bash
'

echo -e "\n✅ Nexus CLI setup completed!"
echo "👉 To open your prover screen:"
echo "   screen -r $SCREEN_NAME"
echo "🔗 Inside, choose option 2 and paste your node ID from https://app.nexus.xyz/nodes"
echo "🧑‍💻 Detach anytime: Ctrl+A then D"
```

### 💾 Save and Run:

1. Copy this script to your VPS:
   
```
nano nexus-cli-setup.sh
```

3. Paste the full script above.


4. Make it executable and run:
   

```
chmod +x nexus-cli-setup.sh
./nexus-cli-setup.sh
```

🚀 After Setup:

To manage your prover:

```
screen -r nexus-prover        # Reattach
```
```
Ctrl+A then D                 # Detach
```
```
screen -XS nexus-prover quit  # Kill session
```
