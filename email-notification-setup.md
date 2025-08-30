
## ✅ Solution: Fix Jenkins SMTP settings for Gmail

Go to **Jenkins > Manage Jenkins > Configure System** → under **E-mail Notification**, adjust these:

| Setting                     | Value                                  |
| --------------------------- | -------------------------------------- |
| **SMTP server**             | `smtp.gmail.com`                       |
| **Use SMTP Authentication** | ✅ Checked                              |
| **User Name**               | `your_email@gmail.com`                 |
| **Password**                | App password (NOT your Gmail password) |
| **Use SSL**                 | ❌ Unchecked                            |
| **Use TLS**                 | ✅ Checked                              |
| **SMTP Port**               | `587`                                  |
| **Reply-To Address**        | Optional                               |
| **Charset**                 | `UTF-8`                                |

> 💡 **Note**: Only **check "Use TLS"**, not SSL. Gmail's port 587 is **TLS-based**, not SSL.

---

### 🔐 App Password Reminder (If using Gmail):

If you have 2-step verification enabled on your Gmail, you'll **need to generate an app password**:

1. Go to: [https://myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords)
2. Choose app = "Mail", device = "Other (custom name)" → name it `Jenkins`
3. Copy the 16-character app password
4. Use **that** in the Jenkins "Password" field

---

### 🧪 After updating:

* Save the configuration.
* Try the **"Test configuration"** again.
* You should see: `Email was successfully sent.`

---
