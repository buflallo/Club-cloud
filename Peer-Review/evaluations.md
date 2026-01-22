## Foreword: The Spirit of "Kata" in Peer-to-Peer Review :

<div align=center>
<img src=https://miro.medium.com/v2/resize:fit:720/format:webp/1*UT1XoD8kaEum8fiMHtveiA.jpeg width=500 />
</div>

In martial arts, "kata" refers to a predefined sequence of movements and techniques that practitioners use to hone their skills.  
Each kata is a reflection of discipline, practice, and the pursuit of perfection. Beyond the movements themselves,  
kata teaches us to approach challenges with humility, focus, and the understanding that mastery comes through collaboration and continuous improvement.  
Similarly, in the world of learning and problem-solving, peer-to-peer review embodies the spirit of kata.  
It is not about pointing out flaws but about guiding one another toward growth. Each correction is a movement in our shared kata—a step toward clarity.

### Please remain polite, courteous and respectful throughout the peer2peer review process.

Let’s begin!

---

# Check lists :

## 1. Questions

### Q: How does a virtual machine work?

---

### Q: Why Debian/CentOS?

---

### Q: What is the difference between Debian and CentOS?

| Feature         | Debian | CentOS |
|-----------------|--------|--------|
| Package Manager |        |        |
| Base System     |        |        |
| Release Model   |        |        |
| Default Desktop |        |        |

---

### Q: The purpose of a virtual machine?

---

### Q: What is the difference between `apt` and `aptitude`?

---

### Q: What is a firewall?

---

## 2. Intended Functionalities

### Operating System Configuration

1. **Install the Operating System**
   - Download the latest stable version of Debian or CentOS.
   - Set up the VM with the hostname `<YourName>-CC`.

---

2. **Configure SSH**
   - Install SSH:
     ```bash
     # Debian
     
     # CentOS
     ```
   - Edit the SSH configuration file (`/etc/ssh/sshd_config`):
     ```bash
     
     ```
   - Restart the SSH service:
     ```bash
     
     ```

---

3. **Set Up Firewall**
   - **Debian**:
     ```bash
     
     ```
   - **CentOS**:
     ```bash
     
     ```

---

4. **Set Up Users and Password Policy**
   - Create a user with the hostname:
     ```bash
     
     ```
   - Configure password policies:
     Edit `/etc/login.defs`:
     ```plaintext
     
     ```
     Edit `/etc/pam.d/common-password` (Debian) or `/etc/security/pwquality.conf` (CentOS):
     ```plaintext
     
     ```
   - Apply the rules to root and other users.

---

5. **Configure `sudo`**
   - Install `sudo`:
     ```bash
     
     ```
   - Add the user to the `sudo` group:
     ```bash
     
     ```
   - Limit authentication attempts:
     Edit `/etc/sudoers`:
     ```plaintext
     
     ```
   - Add a custom error message:
     ```plaintext
     
     ```
   - Enable logging:
     ```plaintext
     
     ```

---

## 3. Example of monitoring Script (`monitoring.sh`)

> the commands may differ, not everyone will use the same commands to solve the same problem.

Create the script:

```bash
#!/bin/bash

# Fill in the required system information commands here

wall << EOF
#Architecture: 
#CPU physical: 
#vCPU: 
#Memory Usage: 
#Disk Usage: 
#CPU load: 
#Last boot: 
#Connections TCP: 
#User log: 
#Network: IP () 
#Sudo:  cmd
EOF
