# 🐧 Run Full Debian Linux on Android (No Root!) — Beginner Guide

> Powered by Termux + QEMU. No VNC. No Docker. No stress. Just paste and run.




---

# ✅ What You'll Get

A full Debian 11 (x86_64) system running inside Android

More storage (+10GB disk expansion)

SSH access (optional)


Works 100% on all Android devices — no root needed.


---

# ⚙️ Step 1: Install Required Tools in Termux

Open Termux and paste:
```
pkg update && pkg upgrade -y
```
```
pkg install wget curl proot unzip tar -y
```
```
pkg install x11-repo -y
```
```
pkg install qemu-utils qemu-system-x86_64 -y

```
---

# 📁 Step 2: Download Debian System Image
```
mkdir -p ~/qemu-debian
cd ~/qemu-debian

wget https://cloud.debian.org/images/cloud/bullseye/latest/debian-11-nocloud-amd64.qcow2
mv debian-11-nocloud-amd64.qcow2 debian-11.qcow2
```

---

# 💾 Step 3: Expand Storage (+10 GB) can always add more to it
```
qemu-img resize debian-11.qcow2 +10G

```
---

# 🚀 Step 4: Boot into Debian

Paste below into Termux:
```
qemu-system-x86_64 \
  -m 2048 \
  -smp 2 \
  -drive file=debian-11.qcow2,format=qcow2 \
  -net nic -net user,hostfwd=tcp::2222-:22 \
  -nographic
```
👤 Login: root (no password)


---

# 🛠️ Step 5: First-Time Setup (Inside Debian)

Once inside Debian, paste:
```
apt update && apt upgrade -y
apt install curl wget build-essential -y

```
---

# 🧠 Step 6: Expand Disk From Inside Debian

Paste the commands below (this will auto-expand your disk):
```
cat <<EOF > ~/resize-disk.sh
#!/bin/bash
set -e

echo "Updating package list and installing required tools..."
apt update
apt install -y parted e2fsprogs

echo "Fixing GPT to use full disk space..."
parted /dev/sda --script fix

echo "Resizing partition 1 to full disk..."
parted /dev/sda --script resizepart 1 100%

echo "Resizing filesystem on /dev/sda1..."
resize2fs /dev/sda1

echo "Done! Disk expanded successfully."
df -h /
EOF

chmod +x ~/resize-disk.sh
./resize-disk.sh

```
---

# 🧞 Step 7: Create a Start Script (One-liner Boot)

Back in Termux, paste:
```
cat <<EOF > ~/start-debian.sh
#!/data/data/com.termux/files/usr/bin/bash
cd ~/qemu-debian
qemu-system-x86_64 \
  -m 2048 \
  -smp 2 \
  -drive file=debian-11.qcow2,format=qcow2 \
  -net nic -net user,hostfwd=tcp::2222-:22 \
  -nographic
EOF

chmod +x ~/start-debian.sh
```
# 🔁 Now you can start Debian anytime with:
```
~/start-debian.sh
```

---

# 🌍 Optional: Fix Slow or Broken Debian Repos

Inside the Debian VM:
```
cat <<EOF > /etc/apt/sources.list
deb http://deb.debian.org/debian bookworm main contrib non-free
deb-src http://deb.debian.org/debian bookworm main contrib non-free

deb http://security.debian.org/debian-security bookworm-security main contrib non-free
deb-src http://security.debian.org/debian-security bookworm-security main contrib non-free

deb http://deb.debian.org/debian bookworm-updates main contrib non-free
deb-src http://deb.debian.org/debian bookworm-updates main contrib non-free
EOF

apt update
apt full-upgrade -y

```
---

# 🔐 Optional: Enable SSH Login (Port 2222)
```
apt install openssh-server -y
service ssh start
```
# From Termux or another device:
```
ssh root@127.0.0.1 -p 2222
```

---

🎉 You Did It!

✅ Debian 11 (x86_64) fully running

✅ +10GB expanded disk

✅ SSH support (optional)

✅ Auto start script


Built with ❤️ by @admirkhen

Drop a star ⭐ if this helped!

