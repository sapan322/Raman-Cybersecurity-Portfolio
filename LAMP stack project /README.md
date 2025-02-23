# Description  
A LAMP stack is a group of open-source software installed together to enable a server to host dynamic websites and web apps.  

**LAMP**: Linux operating system, Apache web server, MariaDB database, PHP for dynamic content.  

*(MariaDB is a drop-in replacement for MySQL in Debian.)*  

---

## Steps:  

### **L** - Linux  

1. Create a **Proxmox VM**.  
2. Install **Debian** on the VM ([Installation Guide](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Installation%20Configuration%20%20Guides/Debian%20(Proxmox%20VM)/Installation.md)).  
3. Configure **Debian** ([Configuration Guide](https://github.com/sapan322/Raman-Cybersecurity-Portfolio/blob/main/Installation%20Configuration%20%20Guides/Debian%20(Proxmox%20VM)/Configuration.md)).  

---

### **A** - Apache  

1. **Update apt packages:**  
   - `sudo apt update`  
2. **Install Apache:**  
   - `sudo apt install apache2` (Confirm by pressing `Y`)  
3. **Verify Apache installation:**  
   - Visit the **Debian VM's IP address** in a web browser to confirm that the Apache HTTP server is running.  

   ![Apache Test](https://github.com/user-attachments/assets/ebc98958-045d-432b-931e-9ab8fd99600c)  

---

### **M** - MariaDB  

1. **Install MariaDB:**  
   - `sudo apt install mariadb-server` (Confirm by pressing `Y`)  
2. **Secure MariaDB:**  
   - Run the security script that comes pre-installed with MariaDB:  
     - `sudo mysql_secure_installation`  
3. **Check MariaDB status:**  
   - `sudo systemctl status mariadb`  

---

### **P** - PHP  

1. **Install PHP and required packages:**  
   - `sudo apt install php libapache2-mod-php php-mysql` (Confirm by pressing `Y`.)  
2. **Restart Apache server:**  
   - `sudo systemctl restart apache2`  
3. **Create an `index.php` file to test PHP:**  
   - `echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/index.php`  

---

## Testing  

### **PHP Test**  
- Open `http://<Linux_IP_address>/index.php` in a web browser.  

   ![PHP Info Page](https://github.com/user-attachments/assets/58fc2c04-e6b3-4da8-b84f-ca2293bde652)  

### **Database Test**  
- Open Terminal and run:  
  - `sudo mariadb`  
  - `CREATE DATABASE example_database;`  
  - `SHOW DATABASES;`  

---

## Lessons Learned  

- How to set up a **LAMP stack** in a **Debian-based** environment.  
- How to install and configure **Apache, MariaDB, and PHP**.  
- How to test **web server functionality** and **PHP processing**.  
- How to secure **MariaDB** using built-in security scripts.  
- How to restart services and troubleshoot installation issues.  
- How to verify and debug **Apache and PHP configurations**.  

---
