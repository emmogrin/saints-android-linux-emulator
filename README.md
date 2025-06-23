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

# 🧞 Step 4: Create a Start Script (One-liner Boot)

Back in Termux, paste:
```
cat <<EOF > ~/start-debian.sh
#!/data/data/com.termux/files/usr/bin/bash
cd ~/qemu-debian
qemu-system-x86_64 \
-m 2048 \
-smp 2 \
-drive file=debian-11.qcow2,format=qcow2 \
-net nic \
-net user,hostfwd=tcp::2222-:22,hostfwd=tcp::3010-:3010,hostfwd=tcp::3011-:3011 \
-nographic
EOF


```

# 🔁 Now you can start Debian anytime (like anyday anytime with):
also make sure you are in the qemu-debian directory, if not enter (cd qemu-debian ). remove brackets.

```
chmod +x ~/start-debian.sh
~/start-debian.sh
```
OR use this

Paste below into Termux:
```
cd qemu-debian
```
```
qemu-system-x86_64 \
-m 2048 \
-smp 2 \
-drive file=debian-11.qcow2,format=qcow2 \
-net nic \
-net user,hostfwd=tcp::2222-:22,hostfwd=tcp::3010-:3010,hostfwd=tcp::3011-:3011 \
-nographic
```
👤 Login: root (no password) you must be asked for it. dont paste until when asked😩

```
root
```

---

# 🛠️ Step 5: First-Time Setup (Inside Debian)

Once inside Debian, paste:
```
apt update && apt upgrade -y
```
```
apt install curl wget build-essential -y

```
---

# 🧠 Step 6: Expand Disk From Inside Debian

Paste the commands below (this will auto-expand your disk):

```
apt install -y parted e2fsprogs
```
```
parted /dev/sda
```
```
print
```
```
resizepart 1
```
Warning: Partition /dev/sda1 is being used. Are you sure you want to continue?
Yes/No? enter Yes.
```
Yes
```
then you get End? [3220MB]? enter 100%
```
100%
```

```
quit
```
```
resize2fs /dev/sda1
```
```
df -h 
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

```
```
apt update
```
```
apt full-upgrade -y
```
---



---

🎉 You Did It!

✅ Debian 11 (x86_64) fully running

✅ +10GB expanded disk

✅ SSH support (optional)

✅ Auto start script


Built with ❤️ by @admirkhen

Drop a star ⭐ if this helped!

