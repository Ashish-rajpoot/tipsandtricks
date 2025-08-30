No, you're not supposed to perform the swap file creation directly inside the `/etc/fstab` file. Instead, the steps to create the swap file and make it permanent are as follows:

1. **Creating the swap file** (These steps are done **outside** `/etc/fstab`):

   * You **create** and **configure** the swap file on your system (typically in the root directory or another location on your disk).

   ```bash
   sudo fallocate -l 1G /swapfile       # Create a 1GB swap file
   sudo chmod 600 /swapfile             # Set proper permissions for security
   sudo mkswap /swapfile                # Mark the file as swap space
   sudo swapon /swapfile                # Enable the swap file
   ```

2. **Making Swap Permanent (Edit `/etc/fstab`)**:

   * After creating and enabling the swap file, you then **edit `/etc/fstab`** to make the swap file persist across system reboots.

   * **How to do this:**

     1. Open the `/etc/fstab` file for editing with a text editor:

        ```bash
        sudo nano /etc/fstab
        ```

     2. Add the following line at the end of the file to tell your system to automatically mount the swap file at boot:

        ```
        /swapfile none swap sw 0 0
        ```

     3. Save the file and exit the editor (for `nano`, it's `Ctrl + X`, then press `Y` to confirm, and `Enter` to save).

3. **Check Swap Status**:

   * Verify if the swap file is enabled:

     ```bash
     sudo swapon --show
     ```

   * You should see `/swapfile` listed as swap space.

4. **Reboot to Confirm**:

   * Reboot your system to ensure the swap file is activated automatically after a reboot:

     ```bash
     sudo reboot
     ```

   * After rebooting, run the `swapon --show` command again to verify that the swap file is still active.

### Key Points:

* **Step 1**: You create the swap file (outside `/etc/fstab`), which is a temporary step.
* **Step 2**: You make it **permanent** by adding an entry in `/etc/fstab`, which will ensure the swap file is mounted automatically after each reboot.

You **don’t** create the swap file inside the `/etc/fstab` file; the `/etc/fstab` file is just used to tell the system where to find the swap space during boot.
