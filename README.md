
# 🌟 How to Use AI Town on Windows 🌟

AI Town is a virtual town where AI characters live, chat, and socialize. This guide will help you set up AI Town on a Windows machine using the Windows Subsystem for Linux (WSL). You will connect to Ollama running on your Windows machine and use Convex for the backend.

## Prerequisites

1. **Windows 10/11 with WSL2 installed**
2. **Internet connection**

## Steps

### 1. Install WSL2

First, you need to install WSL2. Follow [this guide](https://docs.microsoft.com/en-us/windows/wsl/install) to set up WSL2 on your Windows machine. We recommend using Ubuntu as your Linux distribution.

### 2. Update Packages

Open your WSL terminal (Ubuntu) and update your packages:

```bash
sudo apt update


### 3. Install NVM and Node.js

NVM (Node Version Manager) helps manage multiple versions of Node.js. Install NVM and Node.js 18 (the stable version):

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.2/install.sh | bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
source ~/.bashrc
nvm install 18
nvm use 18


### 4. Install Python and Pip

Python is required for some dependencies. Install Python and Pip:

```bash
sudo apt-get install python3 python3-pip
sudo ln -s /usr/bin/python3 /usr/bin/python


### 5. Install Additional Tools

Install \`unzip\` and \`socat\`:

```bash
sudo apt install unzip socat


### 6. Install Rust and Cargo

Cargo is the Rust package manager. Install Rust and Cargo:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env


### 7. Install \`just\` with Cargo

\`just\` is used to run commands. Install it with Cargo:

```bash
cargo install just
export PATH="$HOME/.cargo/bin:$PATH"
just --version


### 8. Configure \`socat\` to Bridge Ports for Ollama

Run the following command to bridge ports, allowing communication between Convex and Ollama:

```bash
socat TCP-LISTEN:11434,fork TCP:$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):11434 &


Test if it's working:

```bash
curl http://127.0.0.1:11434


If it responds OK, the Ollama API is accessible.

### 9. Clone the AI Town Repository

Clone the AI Town repository from GitHub:

```bash
git clone https://github.com/a16z-infra/ai-town.git
cd ai-town


### 10. Install NPM Packages

Install the necessary npm packages:

```bash
npm install


### 11. Install Precompiled Convex

Download and install the precompiled version of Convex:

```bash
curl -L -O https://github.com/get-convex/convex-backend/releases/download/precompiled-2024-06-28-91981ab/convex-local-backend-x86_64-unknown-linux-gnu.zip
unzip convex-local-backend-x86_64-unknown-linux-gnu.zip
rm convex-local-backend-x86_64-unknown-linux-gnu.zip
chmod +x convex-local-backend


### 12. Launch Convex

In a separate terminal, launch Convex:

```bash
./convex-local-backend


### 13. Configure Convex to Use Ollama

Set the Ollama host in Convex:

```bash
just convex env set OLLAMA_HOST http://localhost:11434


### 14. Launch AI Town

Finally, launch AI Town:

```bash
npm run dev


Visit \`http://localhost:5173\` in your browser to see AI Town in action.

### Relaunching AI Town

If you need to restart the services:

1. Ensure \`socat\` is running:
   ```bash
   socat TCP-LISTEN:11434,fork TCP:$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):11434 &
   

2. Launch Convex:
   ```bash
   ./convex-local-backend
   

3. Launch AI Town:
   ```bash
   npm run dev
   

Enjoy your AI Town experience! If you encounter any issues, feel free to reach out for support. 🌟
