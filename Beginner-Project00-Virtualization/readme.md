# Discover the world of virtualization 


### Preamble

> "Once upon a time, in a land filled with broken coffee machines and missing staplers, the brave sysadmins embarked on a journey of endless ticket queues. They dreamed of a world where servers rebooted on their own and updates didn’t require a prayer. Alas, this story has nothing to do with virtualization. So let’s focus on the real heroes—virtual machines. But first, let me tell you about the mythical unicorn of uptime..."

### Introduction

This project aims to introduce you to the wonderful world of virtualization.
You will create your first machine in VirtualBox under specific instructions. Then, at the end of this project, you will be able to set up your own operating system while implementing strict rules, just like you would for a small real-world server.

---

## What You Will Deliver

By the end of this project, you should have:

- A virtual machine running the latest stable Debian or CentOS.
- A hostname following the `<Firstname.Lastname>-CC` convention (e.g., `Issam.Doby-CC`).
- An SSH service running **only** on port `1111`, with root logins disabled.
- A firewall (UFW or firewalld) configured to allow only port `1111`.
- A strong password policy applied to users, including root.
- A strict sudo configuration with logging to `/var/log/sudo/`.
- A `monitoring.sh` Bash script that runs every 10 minutes (via cron) and broadcasts system information with `wall`.
- At least one deployed service (for example, a simple web server) running on your server.

---

## How You Will Work (strongly recommended)

To get closer to real-world workflows, it is **strongly recommended** that you:

- Use Git to track your scripts and configuration notes with several small, meaningful commits instead of one big "final" commit.
- Keep a simple setup log (for example, a Markdown file) where you write down key commands you used, configuration files you edited, and problems you encountered.
- Take a moment to read the `man` pages and `--help` output for commands you use, so you understand what each flag and file does instead of copy-pasting blindly.

<div align=center>
<img src=https://github.com/ablaamim/Born2BeRoot/blob/main/SRC/your-world%20(1).png width=500 />
</div>

---

## Learning Goals

- Understand the concept of virtualization.
- Learn how virtual machines operate.
- Understand the benefits and limitations of virtual machines.

---

## Concepts 

A **virtual machine**, as the name implies, is virtual; in other words, it's not a physical machine. Virtual machines are created using a process known as **virtualization**, which simply creates a virtual version of something, in this case, a full-fledged computer system. Virtual machines are utilized throughout the industry for all sorts of applications, and we will be making extensive use of them in this course.

Virtual machines have been around since the 1960s, thanks largely to *IBM's* development of logical partitions, which allowed a single computer mainframe to run multiple instances of operating systems simultaneously. 

That being said, it wasn't until the late 1990s that virtualization technology seriously took off, when hardware-assisted virtualization starting become ubiquitous. These advancements led to the widespread adoption of virtualization in the enterprise sector and the growth of the cloud computing industry.

In the modern day, virtualization is an essential component of computing, with all major computing platforms supporting it in some form.

A Virtual Machine is an abstraction so that you can have another guest operation system at top of your host operating system by the help of hypervisor. If you are confused what is host operating system? that is your own OS inside your machine. In another hand the guest operating system is an operating system that runs above your host operating system, let's see this picture if you are confused.

<div align=center>
<img src=https://miro.medium.com/v2/resize:fit:640/format:webp/1*i15PriPF4tex1KEcTTEl5w.png alt='Virtualization Diagram' width=500 />
</div>


## What are Virtual machines used for ?

The ability to isolate an entire system gives users a lot of advantages, including:

- **Sandboxing:** You can run anything you want inside a virtual machine with little risk since due to its contained nature. This allows for secure and reliable testing without impacting the rest of the system.
- **Replication:** Virtual machines can be backed up and restored with ease, allowing users to replicate systems very easily. If a system fails or becomes compromised, rolling back is an easy option.
- **Running incompatible software:** You can run other operating systems in virtual machines, allowing you to make use of software available only in a specific OS.
- **Compartmentalization**: Virtual machines allow you to split up a host system into multiple guest systems, allowing for clean separation between them.

That being said, it's not all rainbows and sunshine with virtual machines; they have some disadvantages as well:

- **Performance**: Virtual machines typically do not perform as well as their host computer due to the overhead that comes with virtualization. The difference depends on the virtualization technologies being used.
- **Complexity**: Setting up and managing virtual machines can be complex and time-consuming, and if you have to allocate resources ahead-of-time, adds a non-negligible amount of mental overhead
- **Storage**: While a smaller issue, virtual machines can end up consuming a significant amount of storage space depending on the environments installed on them.

This makes virtual machines particularly useful for **cloud computing**. Cloud vendors typically offer resources online on a pay-as-you-go model; virtual machines vastly facilitate this by allowing vendors to separate a machine based off individual client demands. This means vendors can quickly and easily scale their computing resources up or down as needed, without the need to purchase and maintain physical hardware.

<div align=center>
<img src=https://curriculum-content.s3.amazonaws.com/6685/devops-m0-virtual-machines/virtualization.png wisth=500 />
</div>

#### :ok_hand: Questions to look for 

> Q : How a virtual machine works ?

> Q : Why Debian/Centos?

> Q : What is the difference between Debian and CENTOS ?

> Q : The purpose of a virtual machine ?

> Q : What is the diffrence between apt and aptitude ?

> Q : What is a firewall ?

---

# Intended Functionalities :

---

<div align=center>
<img src=https://i.gifer.com/5Szk.gif width=500 />
</div>




> This project consists of having you set up your first server by following specific rules.

* You must choose as an operating system either the latest stable version of Debian (no testing/unstable), or the latest stable version of Centos. Debian is highly recommended if you are new to system administration.

* A SSH service will be running on port 1111 only. For security reasons, it must not be possible to connect using SSH as root.

* You have to configure your operating system with the UFW (or firewalld for centos) firewall and thus leave only port 1111 open.

* The hostname of your virtual machine must be project owner name ending with '-CC' (e.g., Issam.Doby-CC)

* You have to implement a strong password policy.

* You have to install and configure sudo following strict rules.

* In addition to the root user, a user with your hostname as Firstname.Lastname (e.g, Issam.Doby) has to be present.

> To set up a strong password policy, you have to comply with the following requirements:

• Your password has to expire every 30 days.

• The minimum number of days allowed before the modification of a password will be set to 2.

• The user has to receive a warning message 7 days before their password expires.

• Your password must be at least 10 characters long. It must contain an uppercase
letter, a lowercase letter, and a number. Also, it must not contain more than 3 consecutive identical characters.

• The password must not include the name of the user.

• The following rule does not apply to the root password: The password must have
at least 7 characters that are not part of the former password.

• Of course, your root password has to comply with this policy.

> To set up a strong configuration for your sudo group, you have to comply with the
following requirements:

• Authentication using sudo has to be limited to 3 attempts in the event of an incorrect password.

• A custom message of your choice has to be displayed if an error due to a wrong
password occurs when using sudo.

• Each action using sudo has to be archived, both inputs and outputs. The log file
has to be saved in the /var/log/sudo/ folder.

---

## Monitoring Script (monitoring.sh)

Finally, you have to create a simple script called `monitoring.sh`. It must be developed in Bash.

- The script must be executed automatically using **cron** so that it starts after boot and then runs **every 10 minutes**.
- On each run, it must display the required information (listed below) on all logged-in terminals using `wall`.
- The banner is optional.
- No error messages should be visible when the script runs.
- Hint: you can use a user crontab (for example, `crontab -e`) or system-wide cron configuration files, but always read the relevant `man` pages first so you understand what you are editing.

Your script must always be able to display at least the following information (as a snapshot at the time it runs):

• The architecture of your operating system and its kernel version.

• The number of physical processors.

• The number of virtual processors.

• The current available RAM on your server and its utilization rate as a percentage.

• The current available memory on your server and its utilization rate as a percentage.

• The current utilization rate of your processors as a percentage.

• The date and time of the last reboot.

• The number of active TCP connections.

• The number of users using the server.

• The IPv4 address of your server and its MAC (Media Access Control) address.

• The number of commands executed with the sudo program (for example, by counting entries in your sudo logs).

This is an example of how the script is expected to work:

```bash
Broadcast message from root@Issam.Doby-CC (tty1) 
#Architecture: Linux Issam.Doby-CC 4.18.0-553.6.1.el8.x86_64 #1 SMP Thu May 30 04:13:58 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
#CPU physical : 1
#vCPU : 1
#Memory Usage: 74/987MB (7.50%)
#Disk Usage: 1009/2Gb (49%)
#CPU load: 6.7%
#Last boot: 2024-10-09 7:45
#Connections TCP : 1 ESTABLISHED
#User log: 1
#Network: IP 10.0.2.15 (08:00:27:51:9b:a5)
#Sudo : 10 cmd
```

---

# Your Server your rules !

For the final task, you’ll deploy a service of your choice on your virtual server. You can start simple, setting up an easy-to-deploy web server like:

* Nginx: A lightweight option for hosting static websites or acting as a reverse proxy.

* Apache: A versatile, widely used HTTP server.

* Lighttpd: Known for minimal resource usage, ideal for smaller projects.

At a minimum, you should deploy **one simple service** (for example, a static website served by Nginx, Apache, or Lighttpd) that you can start, stop, and test.

> If you're ready to go further, try a more complex setup such as deploying a full website with an Nginx server, a back-end service, and a database. For the ambitious, consider a multi-component application, with separate front-end, back-end, and database layers.

When you document your final service, make sure you:

- Explain how to start, stop, and restart it.
- Show how to test it (for example, using `curl` and describing the expected output).
- Mention whether it is reachable only from inside the VM or also from the host.

---

## Deliverables :

Use this section as a checklist to verify that your server behaves like a small real-world machine.

### Operating System Configuration :

- [ ] A virtual machine running the latest stable version of Debian or CentOS.
- [ ] Hostname follows the `<Firstname.Lastname>-CC` convention.

### SSH & Firewall Configuration :

- [ ] SSH service is running only on port `1111`.
- [ ] It is not possible to log in as root over SSH (tested).
- [ ] UFW (Debian) or firewalld (CentOS) is configured to allow only port `1111`.

### User Management & Password Policy :

- [ ] A non-root user exists whose Firstname.Lastname matches your hostname without `-CC`.
- [ ] A strong password policy is implemented according to the listed rules and briefly documented.

### Sudo Configuration :

- [ ] Authentication using sudo is limited to 3 attempts.
- [ ] A custom error message appears on incorrect sudo passwords.
- [ ] All sudo actions (inputs and outputs) are logged under `/var/log/sudo/`.

### monitoring.sh Script :

- [ ] `monitoring.sh` is written in Bash and is readable.
- [ ] It is executed automatically by cron every 10 minutes after boot.
- [ ] It sends the required system information to all logged-in users using `wall`.
- [ ] No errors appear when the script runs. You can provide sample output (screenshot or copy-paste) as proof.

### Final Deployment Service :

- [ ] At least one service (for example, an Nginx/Apache/Lighttpd web server) is deployed and running.
- [ ] You can explain how to start, stop, and restart this service.
- [ ] You can show how to test it (for example, with `curl` and expected output).
- [ ] You wrote a short note describing any challenges you faced while setting it up and how you solved them.
