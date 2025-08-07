
---

### 📄 `setup/dvwa-installation.md`

````markdown
# DVWA Installation Guide on Ubuntu Server

This guide covers the installation and basic configuration of **Damn Vulnerable Web Application (DVWA)** on an Ubuntu server (IP: `192.169.200.4`).

---

## 1. Update and Upgrade

```bash
sudo apt update && sudo apt upgrade -y
````

---

## 2. Install Apache, MySQL, PHP, and Required Modules

```bash
sudo apt install apache2 mysql-server php php-mysqli php-gd php-xml php-mbstring php-curl libapache2-mod-php -y
```

---

## 3. Secure MySQL Installation

```bash
sudo mysql_secure_installation
```

* Follow prompts to set root password and secure MySQL.
* Remember the root password or create a dedicated user for DVWA.

---

## 4. Create MySQL Database and User for DVWA

Login to MySQL:

```bash
sudo mysql -u root -p
```

Then run:

```sql
CREATE DATABASE dvwa;
CREATE USER 'dvwauser'@'localhost' IDENTIFIED BY 'dvwapassword';
GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwauser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

## 5. Download DVWA

```bash
cd /var/www/html
sudo git clone https://github.com/digininja/DVWA.git
sudo mv DVWA dvwa
```

---

## 6. Configure DVWA

Copy the sample config file and edit it:

```bash
cd /var/www/html/dvwa/config
sudo cp config.inc.php.dist config.inc.php
sudo nano config.inc.php
```

Edit the database settings:

```php
$_DVWA = array();
$_DVWA[ 'db_user' ] = 'dvwauser';
$_DVWA[ 'db_password' ] = 'dvwapassword';
$_DVWA[ 'db_database' ] = 'dvwa';
```

Save and exit.

---

## 7. Set Permissions

```bash
sudo chown -R www-data:www-data /var/www/html/dvwa/
sudo chmod -R 755 /var/www/html/dvwa/
```

---

## 8. Enable Apache Rewrite Module and Restart Apache

```bash
sudo a2enmod rewrite
sudo systemctl restart apache2
```

---

## 9. Initialize the DVWA Database

* Open a browser on your client or attacker machine.
* Navigate to:

  ```
  http://192.169.200.4/dvwa/setup.php
  ```
* Click the **Create / Reset Database** button.

---

## 10. Login to DVWA

* Default credentials:

  * Username: `admin`
  * Password: `password`

---

## 11. Change Security Level as Needed

* After logging in, go to **DVWA Security** page.
* Select desired security level: `low`, `medium`, `high`, or `impossible`.

---

## ✅ DVWA Installation Complete!

You can now start practicing web vulnerabilities, including session hijacking.

---

> ⚠️ Use only in a controlled lab environment for educational purposes.


