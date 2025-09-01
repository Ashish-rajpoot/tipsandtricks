Here's a complete **User Data script** you can use when launching an EC2 instance. This script will:

* Install **Nginx**
* Enable and start the Nginx service
* Add a simple HTML page that shows:

  * The **public IP address** of the instance
  * The **hostname (EC2 instance name)**

---

### ✅ **User Data Script (Bash)**

```bash
#!/bin/bash

# Update and install Nginx
yum update -y
yum install -y nginx

# Start and enable Nginx
systemctl start nginx
systemctl enable nginx

# Get instance metadata
IP_ADDRESS=$(curl -s http://169.254.169.254/latest/meta-data/public-ipv4)
HOSTNAME=$(curl -s http://169.254.169.254/latest/meta-data/hostname)

# Create custom HTML content
cat <<EOF > /usr/share/nginx/html/index.html
<!DOCTYPE html>
<html>
<head>
    <title>EC2 Instance Info</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding-top: 50px;
            background-color: #f2f2f2;
        }
        .box {
            background: white;
            display: inline-block;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
    </style>
</head>
<body>
    <div class="box">
        <h1>Welcome to your EC2 instance</h1>
        <p><strong>Public IP:</strong> $IP_ADDRESS</p>
        <p><strong>Hostname:</strong> $HOSTNAME</p>
    </div>
</body>
</html>
EOF
```

---

### 💡 Notes:

* This script is tailored for Amazon Linux 2 (uses `yum`). If you're using Ubuntu, change the package manager to `apt` and install Nginx accordingly.
* The metadata IP `169.254.169.254` is accessible from within EC2 and provides instance details.
* You can paste this directly into the **"User data"** field when launching an EC2 instance.

Would you like me to provide the Ubuntu version of this script as well?
