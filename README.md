
# Hosting a Website on Apache using EBS Volume (WSL2/Ubuntu)

This guide explains how to configure an Amazon EBS volume on your Linux (WSL2) system, mount it, and serve content from it using Apache HTTP Server.

---

## 🛠 Prerequisites

- Ubuntu running via WSL2
- Mounted and formatted EBS volume (e.g., `/dev/sdb`)
- Apache2 installed and running
- Root privileges

---

## 📁 Step-by-Step Instructions

### 1. Format and Mount the EBS Volume

```bash
sudo mkfs -t ext4 /dev/sdb               # Format the volume (if not already formatted)
sudo mkdir -p /mnt/myebs                 # Create mount point
sudo mount /dev/sdb /mnt/myebs           # Mount the EBS volume
```

### 2. Create a Directory for Your Website

```bash
sudo mkdir -p /mnt/myebs/website
sudo chown lisha:lisha /mnt/myebs/website
sudo chmod 755 /mnt/myebs/website
```

### 3. Test Write Access

```bash
echo "This is a test file." > /home/lisha/myfile.txt
mv /home/lisha/myfile.txt /mnt/myebs/website/
```

---

## 🌐 Apache Configuration

### 4. Create a Symbolic Link in Apache's Web Directory

```bash
sudo ln -s /mnt/myebs/website /var/www/html/website
```

### 5. Adjust Ownership and Permissions

```bash
sudo chown -R www-data:www-data /mnt/myebs/website
sudo chmod -R 755 /mnt/myebs/website
```

---

## 🔥 Enable and Restart Apache

```bash
sudo systemctl restart apache2
sudo systemctl status apache2
```

You should see Apache running successfully.

---

## 🔁 Redirect Handling

You might notice a 301 Redirect when you `curl` the `/website` path. This is standard for directories—Apache redirects to `/website/`.

### Check the Response:

```bash
curl -I http://localhost/website
```

You should receive a `301 Moved Permanently` header.

---

## 🔐 Allow Apache via Firewall (if UFW is used)

```bash
sudo ufw allow 'Apache Full'
```

---

## 🛜 Final Test

Open a browser and go to:

```
http://localhost/website/
```

You should see the contents served from your EBS-backed directory.

---

## 📌 Notes

- Make sure `/mnt/myebs` is mounted every time the system boots.
- Add an entry to `/etc/fstab` if you want to automate mounting.
- Ensure Apache has permission to read and serve the content.

---

## ✅ Done!

You have successfully hosted your website on Apache using an EBS volume in WSL2.

