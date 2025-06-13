### **Project: MarketPeak E-commerce Deployment**  
#### **Complete Step-by-Step Guide for Beginners**  

---

### **Phase 1: Local Repository Setup**  
*(Where you organize project files on your computer)*  

#### **1. Create Project Directory**  
```bash
mkdir MarketPeak_Ecommerce  # Creates new folder
cd MarketPeak_Ecommerce     # Enters the folder
```  
**Why?**  
Folders keep your project files organized. This creates a dedicated home for your e-commerce site files.  
![Directory Creation](./img/1.%20Creating%20a%20local%20repo.jpg) 
![Directory Creation](./img/2.%20Change%20Directory%20to%20MarketPeak_Ecommerce.jpg)  

#### **2. Initialize Git Repository**  
```bash
git init          # Starts version control
git status        # Checks current status
```  
**Why?**  
Git tracks file changes. `git init` creates an invisible storage area (.git folder) to save file versions.  
![Git Initialization](./img/3.%20Confirming%20the%20git%20init.jpg)  

---

### **Phase 2: Template Setup**  
*(Adding website design files)*  

#### **3. Add E-commerce Template**  
Download and add:  
- HTML files (`index.html`, `about.html`)  
- CSS styles (`styles.css`)  
- JavaScript files (`script.js`)  
- Images (`/images` folder)  

**Why?**  
These files form your website's structure (HTML), design (CSS), and functionality (JS).  
![Template Structure](./img/4,%20Template%20Downloaded%20and%20edithed.jpg)  

---

### **Phase 3: Git Configuration & Commit**  
*(Saving your work)*  

#### **4. Set Git Identity**  
```bash
git config --global user.name "YourName"
git config --global user.email "you@example.com"
```  
**Why?**  
Git needs to know who made changes. This sets your author name/email for all future commits.  
![Directory Creation](./img/5.%20Staging%20and%20Setting%20Git%20Global%20config.jpg) 

#### **5. Save Changes (Commit)**  
```bash
git add .  # Stages all new/modified files
git commit -m "Initial website structure"  # Saves snapshot
```  
**Why?**  
`git add` selects files to save. `git commit` permanently stores this version with a description.  
![Initial Commit](./img/6.%20nitial%20commit%20with%20basic%20e-commerce%20site%20structure.jpg)  

---

### **Phase 4: GitHub Setup**  
*(Cloud backup and collaboration)*  

#### **6. Create GitHub Repository**  
1. Go to [github.com](./img/7.%20Created%20remote%20repo%20with%20name%20MarketPeak_Ecommerce%20without%20initializing.jpg)  
2. Click "+" → "New repository"  
3. Name: `MarketPeak_Ecommerce`  
4. **Uncheck** "Initialize with README" (avoids conflicts)  

**Why?**  
GitHub stores your code online. Not initializing with README lets you push existing code easily.  
![GitHub Repo Creation](./img/8.%20Remote%20repo%20created%20without%20init%20with%20README%20file.jpg)  

#### **7. Connect Local to GitHub**  
```bash
git remote add origin https://github.com/YourUsername/MarketPeak_Ecommerce.git
```  
**Why?**  
Links your local folder to the online GitHub repository. "origin" is a nickname for your GitHub URL.  
![Remote Setup](./img/9.%20Git%20remote%20add.jpg)  

---

### **Phase 5: AWS EC2 Setup**  
*(Creating a web server in the cloud)*  

#### **8. Create SSH Key Pair**  
1. In AWS EC2 console → "Key Pairs" → "Create key pair"  
2. Name: `alami-key`  
3. Type: RSA (works with most SSH clients)  
4. Format: .pem (private key file)  

**Why?**  
This key acts like a secure password to access your server. Never share the .pem file!  
![Key Pair Creation](./img/10.%20creating%20amazon%20linux%20AMI%20key.jpg)  

#### **9. Launch EC2 Instance**  
1. Choose "Amazon Linux 2023" AMI  
2. Select t2.micro (free tier eligible)  
3. Configure security group: Allow HTTP(80), HTTPS(443), SSH(22)  
4. Attach `alami-key`  

**Why?**  
EC2 is a virtual server. Security groups control what traffic can reach it (like a firewall).  
![Instance Creation](./img/11.%20Launch%20the%20Instance%20with%20Amazon%20Linux%20AMI.jpg)

![Success Creation](./img/12.%20Success%20Launch%20of%20the%20Instance%20with%20Amazon%20Linux%20AMI.jpg)

#### **10. Connect to Server**  
```bash
chmod 400 alami-key.pem  # Lock down key
ssh -i alami-key.pem ec2-user@YOUR_IP
```  
![SSH 2 Instance](./img/13.%20Coonect%20to%20the%20instance%20using%20SSH.jpg)

**Why?**  
`chmod 400` prevents others from reading your key. SSH lets you control the server remotely.  

![Generating SHH Keypair](./img/14.%20Generating%20SSH%20keypair%20using%20ssh-keygen%20-t%20ed25519%20-c%20myemail.jpg)

![AWS Public Key added to GH](./img/15.%20Public%20key%20added.jpg)

![Test Connection](./img/16.%20Test%20Connection%20sucessful.jpg)

*(Getting GitHub SSH url)*
![Repo URL](./img/17.%20Get%20Github%20ssh%20url.jpg)

*You must install Git on the Amazon Linux Server*
![Install Git](./img/18.%20Install%20Git%20on%20Amazon%20Linux.jpg)

![Clone the Repo](./img/19.%20Clone%20Repo%20from%20GitHub%20via%20SSH.jpg)

---

### **Phase 6: Deployment**  
*(Putting your site online)*  

#### **11. Install Web Server**  
```bash
sudo yum update -y        # Updates software
sudo yum install httpd -y # Installs Apache
sudo systemctl start httpd # Starts web server
```  
![Install Web Server](./img/20.%20Install%20httpd.jpg)
![Install Web Server](./img/21.%20Start%20and%20Enable%20the%20Apache%20Service.jpg)
**Why?**  
Apache (httpd) serves your website files to visitors. `systemctl start` activates it immediately.  
![Start and Enable Web Server](./img/21.%20Start%20and%20Enable%20the%20Apache%20Service.jpg)

*Confirm if Apache is running*
![Confirm if Apache is running](./img/22.%20Check%20if%20Apache%20service%20is%20running.jpg)

#### **12. Upload Website Files**  
```bash
sudo cp -r MarketPeak_Ecommerce/* /var/www/html/
```  
![Upload Files](./img/23.%20Configure%20Apache%20HTTP%20Server%20and%20prepare%20the%20web%20directory.jpg)

**Why?**  
`cp -r` copies your code from GitHub. `/var/www/html` is Apache's default folder for websites.  

#### **13. Fix Permissions**  
```bash
sudo chown -R apache:apache /var/www/html  # Sets owner
sudo chmod -R 755 /var/www/html           # Sets permissions
sudo systemctl restart httpd               # Reloads server
```  
**Why?**  
Apache needs ownership to serve files. `755` means:  
- Owner: read/write/execute  
- Others: read/execute  

---

### **Phase 7: Verify Your Live Site**  
Visit in your browser:  
```
http://YOUR_EC2_IP/index.html
```
![Verify Your Live Site](./img/25.%20Access%20MarketPeak%20from%20Browser.jpg)
---

### **Maintenance Guide**  
**Update Your Site:**  
1. Make changes locally  
2. Commit to GitHub:  
   ```bash
   git add .
   git commit -m "Update products"
   git push origin main
   ```  
   ![Make changes and stage](./img/27.%20Git%20Pushed%20to%20remote%20repo.jpg)
3. On server:  
   ```bash
   cd /var/www/html
   sudo git pull origin main
   ```   
---

#### **31. Create Pull Request**  
1. Go to GitHub → Pull Requests → "New Pull Request"  
2. Compare `development` → `main`  
3. Add description and merge  
![Pull Request](./img/28.%20Pull%20request%20initiated.jpg)  

---

### **Security Best Practices**  
#### **1. File Permissions**  
```bash
sudo chown -R apache:apache /var/www/html  # Correct owner
sudo find /var/www/html -type d -exec chmod 755 {} \;  # Directory permissions
sudo find /var/www/html -type f -exec chmod 644 {} \;  # File permissions
```  
**Why?**  
Prevents hackers from modifying files:  
- `755` = Owner: full, Others: read+execute  
- `644` = Owner: read+write, Others: read-only  

#### **2. Regular Updates**  
```bash
sudo yum update -y        # OS updates
sudo yum update httpd -y  # Apache updates
```  
Schedule weekly: `sudo crontab -e` → `0 3 * * 1 yum -y update`  

---

### **Troubleshooting Guide**  
| **Issue**               | **Solution**                          |
|-------------------------|---------------------------------------|
| **Site not updating**   | `sudo systemctl restart httpd`        |
| **Permission denied**   | `sudo chown -R apache:apache /var/www/html` |
| **Git pull fails**      | `sudo chmod -R 755 .git`              |
| **Port 80 blocked**     | Check AWS Security Group rules        |

---

### **Advanced: CI/CD Pipeline**  
Create `.github/workflows/deploy.yml`:  
```yaml
name: Deploy to EC2
on: [push]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: SSH & Deploy
        uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.EC2_IP }}
          key: ${{ secrets.SSH_KEY }}
          script: |
            cd /var/www/html
            sudo git pull
            sudo systemctl reload httpd
```  
**Benefits:**  
- Auto-deploys when you push to GitHub  
- No manual server commands needed  

---
**Key Principles:**  
1. **Version Control**: GitHub tracks all changes  
2. **Isolation**: Develop in branches, merge to main  
3. **Automation**: CI/CD eliminates manual deployment  
4. **Security**: Least privilege permissions  

### **Beginner FAQ**  
**Q: Why use SSH keys instead of passwords?**  
A: Keys are longer (2048+ bits vs. 8-12 characters) and immune to brute-force attacks.  

**Q: What if I lose my .pem file?**  
A: Create a new key pair in AWS, then replace it in the instance settings.  

**Q: How do I make my site use HTTPS?**  
A: Install Certbot:  
```bash
sudo yum install certbot python3-certbot-apache
sudo certbot --apache
```  

**Q: Why copy files instead of cloning directly to /var/www/html?**  
A: Apache requires specific ownership permissions that `git clone` doesn't set automatically.  

---

**Key Components:**  
1. **EC2**: Your cloud server  
2. **Apache**: Web server software  
3. **GitHub**: Code storage  
4. **SSH Keys**: Secure access method  


