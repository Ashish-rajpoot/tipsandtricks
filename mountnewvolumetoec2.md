Step 7: Monitor Performance

Now that you have more disk space, you should monitor the performance of Jenkins and check the volume’s usage periodically:
```bash
df -h

# output
NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS loop0 7:0 0 27.6M 1 loop /snap/amazon-ssm-agent/11797 loop1 7:1 0 49.3M 1 loop /snap/snapd/24792 loop2 7:2 0 73.9M 1 loop /snap/core22/2045 nvme0n1 259:0 0 8G 0 disk ├─nvme0n1p1 259:1 0 7G 0 part / ├─nvme0n1p14 259:2 0 4M 0 part ├─nvme0n1p15 259:3 0 106M 0 part /boot/efi └─nvme0n1p16 259:4 0 913M 0 part /boot nvme1n1 259:5 0 15G 0 disk i added 15gm
```



You can also monitor the EC2 instance’s CPU and memory usage using top or htop.

Great! You've successfully attached a **15GB EBS volume** (`nvme1n1`) to your EC2 instance — it's visible in the `lsblk` output.

Now let's **format**, **mount**, and (optionally) **persist** it across reboots.

---

## ✅ Next Steps

### 🔹 **Step 1: Format the New Volume**

Format the new 15GB volume (`/dev/nvme1n1`) with the **ext4** file system:

```bash
sudo mkfs.ext4 /dev/nvme1n1
```

> ⚠️ This will erase anything on that disk — you're safe since it's a new volume.

---

### 🔹 **Step 2: Create a Mount Point**

Choose a directory to mount it, e.g., `/mnt/data`:

```bash
sudo mkdir -p /mnt/data
```

---

### 🔹 **Step 3: Mount the Volume**

Mount it:

```bash
sudo mount /dev/nvme1n1 /mnt/data
```

---

### 🔹 **Step 4: Verify Mount**

Run:

```bash
df -h
```

You should now see `/dev/nvme1n1` mounted on `/mnt/data`.

---

### 🔹 **Step 5: Make It Persistent (After Reboot)**

To make sure it stays mounted after reboot:

1. Get the UUID of the disk:

```bash
sudo blkid /dev/nvme1n1
```

You’ll get something like:

```
/dev/nvme1n1: UUID="abc123-xyz" TYPE="ext4"
```

2. Edit the `/etc/fstab` file:

```bash
sudo nano /etc/fstab
```

3. Add this line at the bottom (replace the UUID with the one from above):

```
UUID=abc123-xyz  /mnt/data  ext4  defaults,nofail  0  2
```

> `nofail` ensures the system boots even if the volume fails to mount.

4. Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).

5. Test the fstab entry without rebooting:

```bash
sudo mount -a
```

If there’s no error — you're good.

---

### ✅ Done!

You now have an extra **15GB** of space mounted at `/mnt/data`. You can use it to:

* Store Jenkins artifacts
* Cache Docker layers
* Offload logs or backups

Let me know if you want help **moving Jenkins data** or using the volume in a Docker container.
