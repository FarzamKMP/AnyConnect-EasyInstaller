# 🇮🇷 راهنمای سریع نصب AnyConnect VPN (ocserv)

این راهنما برای راه‌اندازی سریع VPN سازگار با **Cisco AnyConnect / OpenConnect** روی سرور لینوکس است.

---

## پیش‌نیازها

شما فقط نیاز دارید به:

* یک سرور Debian/Ubuntu
* یک دامنه فعال مثل:

  ```
  portal.yourdomain.com
  ```
* دسترسی root به سرور

---

## مراحل نصب (خیلی سریع)

### 1) اتصال دامنه به سرور

در DNS دامنه یک رکورد بسازید:

| Type | Name   | Value   |
| ---- | ------ | ------- |
| A    | portal | IP سرور |

تست کنید:

```bash
nslookup portal.yourdomain.com
```

---

### 2) دریافت SSL Certificate

```bash
apt update
apt install -y certbot
certbot certonly --standalone -d portal.yourdomain.com
```

---

### 3) اجرای اسکریپت نصب

```bash
git clone https://github.com/yourrepo/AnyConnect-EasyInstaller.git
cd AnyConnect-EasyInstaller
chmod +x anyconnect.sh
sudo ./anyconnect.sh --domain portal.yourdomain.com --user vpn --persist-iptables
```

---

## اطلاعات اتصال

بعد از نصب:

* **Server Address:**

  ```
  portal.yourdomain.com
  ```
* **Username:**

  ```
  vpn
  ```
* **Password:**
  پسوردی که هنگام اجرای اسکریپت وارد کردید

---

## اتصال از لینوکس

```bash
openconnect --protocol=anyconnect https://portal.yourdomain.com -u vpn
```

---

## دستورات مفید

وضعیت سرویس:

```bash
systemctl status ocserv
```

بررسی پورت:

```bash
ss -lntp | grep 443
```

لیست کاربران:

```bash
cat /usr/local/etc/ocserv/ocpasswd
```

ساخت کاربر جدید:

```bash
ocpasswd -c /usr/local/etc/ocserv/ocpasswd newuser
```

---

## نکته مهم

حتماً با **دامنه** وصل شوید، نه با IP:

```
https://portal.yourdomain.com
```

---

---

# 🇬🇧 Quick Setup Guide – AnyConnect VPN (ocserv)

This guide helps you deploy a fully working **AnyConnect-compatible VPN server** in minutes.

---

## Requirements

* Debian/Ubuntu server
* A valid domain name, e.g.:

```
portal.yourdomain.com
```

* Root access

---

## Installation Steps

### 1) Point Domain to Server

Create an A record in DNS:

| Type | Name   | Value          |
| ---- | ------ | -------------- |
| A    | portal | YOUR_SERVER_IP |

Verify:

```bash
nslookup portal.yourdomain.com
```

---

### 2) Obtain SSL Certificate

```bash
apt update
apt install -y certbot
certbot certonly --standalone -d portal.yourdomain.com
```

---

### 3) Run Installer Script

```bash
git clone https://github.com/yourrepo/AnyConnect-EasyInstaller.git
cd AnyConnect-EasyInstaller
chmod +x anyconnect.sh
sudo ./anyconnect.sh --domain portal.yourdomain.com --user vpn --persist-iptables
```

---

## Connection Details

* **Server Address:**

```
portal.yourdomain.com
```

* **Username:** `vpn`
* **Password:** the one you set during installation

---

## Connect from Linux

```bash
openconnect --protocol=anyconnect https://portal.yourdomain.com -u vpn
```

---

## Useful Commands

Check service:

```bash
systemctl status ocserv
```

Verify port:

```bash
ss -lntp | grep 443
```

Add new user:

```bash
ocpasswd -c /usr/local/etc/ocserv/ocpasswd newuser
```

---

## Important Notes

* Always connect using the **domain**, not IP
* Default setup uses **TCP 443 only** for maximum compatibility
* Make sure port 443 is free (no nginx/apache running)
