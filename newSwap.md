Great — you want to increase your swap space to **4 GB**. Here's the exact sequence of commands to safely do that:

---

### ✅ **Step-by-Step: Set 4GB Swap**

> You’re replacing the old swap with a new 4 GB one.

#### 🔄 1. Turn off existing swap

```bash
sudo swapoff /swapfile
```

#### 🗑️ 2. Delete the old swap file (optional cleanup)

```bash
sudo rm /swapfile
```

#### 🆕 3. Create a new 4 GB swap file

```bash
sudo fallocate -l 4G /swapfile
```

*(If `fallocate` doesn’t work on your file system, use `dd` as a fallback:)*

```bash
sudo dd if=/dev/zero of=/swapfile bs=1M count=4096 status=progress
```

#### 🔒 4. Set correct permissions

```bash
sudo chmod 600 /swapfile
```

#### ⚙️ 5. Mark it as swap space

```bash
sudo mkswap /swapfile
```

#### 🚀 6. Enable swap

```bash
sudo swapon /swapfile
```

#### ✅ 7. Confirm it's active

```bash
swapon --show
free -h
```

You should see **4.0G** under the "Swap" section.

---

### 🔁 **Make It Permanent (if not already)**

Ensure this line is in your `/etc/fstab`:

```bash
/swapfile none swap sw 0 0
```

To check:

```bash
sudo nano /etc/fstab
```

If the line already exists, you're good. If not, add it manually at the bottom and save.

---

Let me know if you'd like to automate this, or also optimize Jenkins heap memory settings for better performance.
