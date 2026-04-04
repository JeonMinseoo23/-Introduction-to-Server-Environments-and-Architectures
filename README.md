# BRG-27 — Infrastructure Systems Engineering Activity (ISEA)

**Student:** Teo Qing Ya Audrey  
**Kaplan ID:** CT0384570  
**Murdoch ID:** 36060198  
**Module:** BRG-27 Introduction to Server Environments and Architectures  
**Host OS:** Windows 11 Pro  
**Linux Environment:** Ubuntu 24.04.4 LTS via WSL 2

---

## About This Repository

This repository documents my hands-on lab work for the BRG-27 Infrastructure Systems Engineering Activity module at Murdoch University. Each folder corresponds to a lab session covering a core area of Linux administration, cloud infrastructure, and server management.

The labs progress from foundational Linux skills — setting up the environment, navigating the file system, managing services and permissions — through to real-world infrastructure topics such as cloud provisioning on AWS, DNS configuration, SSL certificates, and shell scripting automation.

All Linux lab work was performed on Ubuntu 24.04.4 LTS running natively on Windows 11 via WSL 2. Cloud labs were performed on Amazon Web Services (AWS) using the free tier. Each lab folder contains a written walkthrough of the steps taken, commands used, observations made, and reflections on what was learned. Screenshots are included as evidence of hands-on completion.

This repository also serves as preparation for the final video demonstration, where all lab work and key technical concepts will be presented and explained.

---

## Environment

| Component | Details |
|-----------|---------|
| Host OS | Windows 11 Pro |
| Linux Method | Windows Subsystem for Linux (WSL 2) |
| Distribution | Ubuntu 24.04.4 LTS (Noble Numbat) |
| Kernel | 6.6.87.2-microsoft-standard-WSL2 |
| Architecture | x86_64 |

---
# Lab 1b — Familiarity with Ubuntu Linux

**Module:** BRG-27 ISEA  
**Day:** 1b  
**Status:** Completed

---

## Objective

Get hands-on with core Linux administration — from basic command line navigation through to managing services, users, firewalls, SSH, and file compression. The loopback address `127.0.0.1` was used to simulate a partner machine, which is common industry practice for testing network configurations locally before going live.

---

## Environment

| Component | Details |
|-----------|---------|
| OS | Ubuntu 24.04.4 LTS via WSL 2 |
| Shell | bash |
| Primary User | XiaoXuee |
| Simulated Partner | audrey_test (via 127.0.0.1) |

---

## Learning Objectives

- Navigate the Linux file system using basic CLI commands
- Install and configure Apache, SSH server, and firewall (UFW)
- Test web service accessibility over LAN
- Use nmap to detect open services and ports
- Use SSH and SCP for remote access and file transfer
- Create and manage users and understand privilege separation
- Compress and decompress files using tar and bzip2

---

## Part 1 — Basic Command Line Navigation

Practised moving around the Linux file system using `pwd`, `ls`, and `cd`. `pwd` confirmed the home directory at `/home/user`. `ls /etc` revealed the full collection of system configuration files. `cd ~` returned to home from any location.

![Navigation commands](lab-1b/01-navigation.png)

---

## Part 2 — Creating Files and Directories

Used `mkdir` and `touch` to create a working directory and files inside it. `ls -l` confirmed both files were created with `-rw-r--r--` permissions.

![mkdir and touch](lab-1b/02-mkdir-touch.png)

---

## Part 3 — Linux Directory Structure

Explored the three key system directories:

| Directory | Purpose |
|-----------|---------|
| `/etc` | System-wide configuration files — network, accounts, service configs |
| `/var` | Variable runtime data — logs, mail, spool, crash reports |
| `/home` | User home directories — personal files and settings per user |

![Directory structure](lab-1b/03-directory-structure.png)

---

## Part 4 — Manual Pages

Used `man ls` to explore the built-in documentation for the `ls` command. The man page shows every available flag and option without needing internet access.

![man ls](lab-1b/04-man-ls.png)

---

## Part 5 — CLI File Operations & System Info

Practised creating, copying, and viewing files. Checked system information using `uname -a`, `hostnamectl`, and `ps -e`. The process list confirmed services like `sshd`, `apache2`, and `systemd` were running.

![File ops and system info](lab-1b/05-file-ops-sysinfo.png)

---

## Part 6 — Super User & Permissions

Demonstrated privilege escalation with `whoami` and `sudo whoami`. Attempted `adduser` without sudo — it failed. With sudo it succeeded.

![sudo whoami](lab-1b/06-sudo-whoami.png)

---

## Part 7 — Install Apache and Test Web Access

Installed Apache using `sudo apt install apache2` and visited `http://127.0.0.1` in the browser to confirm the default page was live. Used `ip a` to determine the machine's IP address.

![Install Apache](lab-1b/install%20apache2.png)

![Install Apache 2](lab-1b/install%20apache2%202.png)

![ip a](lab-1b/ip%20a.png)

---

## Part 8 — Edit index.html and Share with Partner

Edited `/var/www/html/index.html` using nano to replace the default Apache page with a custom page — "Peer Page, Modified by Audrey Teo". Verified the content with `cat` then visited `http://127.0.0.1` in the browser to confirm the change was live.

![Editing index.html in nano](lab-1b/09-edit-indexhtml.png)

![cat index.html output](lab-1b/10-cat-indexhtml.png)

![Peer page in browser](lab-1b/Peer%20page.png)

---

## Part 9 — Scan Ports with Nmap and Remove Apache

Ran `nmap 127.0.0.1` to scan open ports — both port 22 (SSH) and port 80 (HTTP) showed as open with Apache running. Removed Apache and reran Nmap — port 80 disappeared, confirming that removing a service directly closes its port.

![Nmap scan with Apache running](lab-1b/07-nmap.png)

![Remove Apache and rerun Nmap](lab-1b/11-remove-apache-nmap.png)

---

## Part 10 — Enable UFW and Observe Service Access

Enabled UFW and allowed port 80. Observed that blocking port 80 via UFW prevented web access even with Apache running — showing the firewall and the service are independent security layers.

```bash
sudo ufw enable
sudo ufw allow 80
sudo ufw status
```

![UFW status](lab-1b/08-ufw-status.png)

---

## Part 11 — Attempt SSH and Troubleshoot with UFW Rules

Attempted SSH into the partner machine using the loopback address. Troubleshot connectivity issues by checking UFW rules and ensuring OpenSSH was allowed through the firewall.

```bash
sudo ufw allow OpenSSH
ssh audrey_test@127.0.0.1
```

![SSH login](lab-1b/SH%20login.png)

---

## Part 12 — Create a New User and SSH

Created a new user `audrey_test` using `sudo adduser` and SSH'd into the machine using that account to simulate connecting to a partner machine.

![Install SSH](lab-1b/install%20SSH%201.png)

![Install SSH 2](lab-1b/install%20SSH%202.png)

---

## Part 13 — Download Books Using wget

Downloaded books from Project Gutenberg using `wget` to practise retrieving files from the internet via the command line.

```bash
wget https://www.gutenberg.org/cache/epub/11/pg11.txt
```

![wget download](lab-1b/13-wget.png)

---

## Part 14 — Create Directory, Move Files, Create tar Archive

Created a `books/` directory, moved downloaded files into it, and created a tar archive.

```bash
mkdir books
mv pg11.txt books/
tar -cvf books.tar books
```

![mkdir and tar](lab-1b/14-mkdir-tar.png)

---

## Part 15 — Compress, Decompress and Extract

Compressed the tar archive using `bzip2`, then decompressed and extracted it to verify the contents were intact.

```bash
bzip2 books.tar
ls -lh books.tar.bz2
bunzip2 books.tar.bz2
tar -xvf books.tar
```

![wget and bzip2 compression](lab-1b/wget%20bzip2.png)

---

## Challenge Activities

### Challenge 1 — Remote File Creation via SSH

SSH'd into `audrey_test` and created `remote_task.txt` remotely, confirming SSH gives full shell access to the remote machine.

![Remote file creation](lab-1b/touchremotetask.png)

### Challenge 2 — Remote GUI Apps via SSH

Attempted to launch `gedit` over SSH — it failed because `gedit` requires a display server. SSH provides terminal access only.

### Challenge 3 & 4 — SCP File Transfer

Used SCP to transfer a single file and recursively copy the entire `books/` directory to the partner machine.

```bash
scp hello.txt audrey_test@127.0.0.1:~/
scp -r books/ audrey_test@127.0.0.1:~/
```

![gedit fail and SCP](lab-1b/12-gedit-scp.png)

![SCP archive transfer](lab-1b/SCP%20transfer.png)

---

## Issues Encountered

| Issue | Resolution |
|-------|------------|
| `bzip2` not installed | Ran `sudo apt install bzip2 -y` |
| `gedit` failed over SSH | Expected — GUI apps require `ssh -X` for display forwarding |

---

## Outcome

- Navigated the Linux file system using `pwd`, `ls`, `cd`, `mkdir`, and `touch`
- Installed and tested Apache web server, edited `index.html` using nano
- Scanned open ports using Nmap before and after removing Apache — confirmed port 80 disappeared
- Configured UFW firewall rules and observed independent control over service accessibility
- Created a new user and SSH'd between accounts using the loopback address
- Downloaded files with `wget`, compressed with `tar` and `bzip2`, transferred with `scp`
- Demonstrated privilege escalation with `sudo` and discussed the principle of least privilege

---

## Reflection

Using `127.0.0.1` as a loopback to simulate a partner was a practical way to test SSH and SCP without needing a second machine. The Nmap exercise clearly showed that removing a service closes its port — a running service and a firewall rule are two independent controls. The gedit failure over SSH was a good reminder that servers are headless by default and all administration must be done through the terminal.

---

# Lab 2a — Total Cost of Ownership (TCO) Analysis

**Module:** BRG-27 ISEA  
**Day:** 2a  
**Status:** Completed

---

## Objective

Apply TCO methodology to a real-world procurement decision by comparing two printer models over a five-year period. The exercise required gathering manufacturer specifications, defining usage assumptions, calculating fixed and variable costs using a spreadsheet, and interpreting the results to make a justified recommendation.

---

## Environment

| Component | Details |
|-----------|---------|
| Tool | Microsoft Excel |
| Comparison Period | 5 Years |
| Printer A | Canon PIXMA G3020 (Ink Tank, Wireless, Print/Scan/Copy) |
| Printer B | HP LaserJet Pro M404n (Mono Laser, Network, Print Only) |
| Currency | SGD |

---

## Learning Objectives

- Define TCO and distinguish between fixed and variable costs
- Use spreadsheet formulas to calculate and compare total costs across printer types
- Define and document assumptions clearly and consistently
- Evaluate procurement decisions based on calculated TCO data
- Compare cost models for different usage scenarios

---

## Assumptions & Methodology

All costs were calculated using the assumptions below. Pricing was sourced from manufacturer spec sheets, Officeworks, and SP Group electricity rates.


![TCO Assumptions and Methodology](lab-2a/01-TOC1.png)

---

## TCO Spreadsheet — 5-Year Cost Comparison

The spreadsheet below documents every cost line item for both printers, using formulas to derive totals from the assumptions above.

![TCO Spreadsheet — Full Comparison](lab-2a/02-TOCO2.png)

---

## How Costs Were Calculated

The table below summarises the calculation method for each cost component side by side.

![Cost Calculation Breakdown](lab-2a/02-Cost3.png)

### Summary of Results

| Printer | 5-Year TCO |
|---------|-----------|
| Canon PIXMA G3020 | SGD $3,578.30 |
| HP LaserJet Pro M404n | SGD $9,217.00 |
| **Canon saves** | **SGD $5,638.70** |

The Canon PIXMA G3020 is significantly cheaper over five years. Its ink tank system costs a fraction of laser toner, and its 11W active power draw versus 380W for the HP means electricity costs are negligible by comparison. Even with contingency replacement units budgeted for both printers, Canon remains the more cost-effective choice. The HP M404n offers faster speeds (38 ppm vs 9 ipm) and suits mono-only, high-speed office environments where print speed and network reliability outweigh running costs.

---

## Reflection Questions

![Reflection Questions](lab-2a/02-Reflection%20Question4.png)

---

## Outcome

- Defined and applied TCO methodology to a real procurement scenario
- Documented all assumptions with sources and calculation basis
- Built a structured spreadsheet comparing fixed and variable costs across two printer models
- Calculated a 5-year TCO showing Canon at SGD $3,578.30 versus HP at SGD $9,217.00
- Identified break-even conditions and non-financial factors affecting the decision
- Produced a justified recommendation based on quantitative analysis

---

# Lab 2b — Cloud Computing & Bash Scripting

**Module:** BRG-27 ISEA  
**Day:** 2b  
**Status:** Completed

---

## Objective

Launch and configure a cloud virtual machine on Microsoft Azure, install and serve content using Apache2, and demonstrate file management, network access, and remote connectivity. Then extend the lab with Bash scripting — writing and executing shell scripts for system information, loops, conditionals, and resource monitoring.

---

## Environment

| Component | Details |
|-----------|---------|
| Cloud Platform | Microsoft Azure |
| VM Name | my-vm |
| OS | Ubuntu 24.04.4 LTS |
| VM Size | Standard B2ats v2 (2 vCPUs, 1 GiB RAM) |
| Region | Central India (Zone 1) |
| Public IP | 98.70.33.154 |
| Local OS | Windows 11 |
| SSH Client | Windows Command Prompt (native SSH) |
| Username | azureuser |

---

## Learning Objectives

- Launch and configure a cloud VM on Azure
- Configure Network Security Group rules to allow SSH and HTTP traffic
- Connect to a remote VM using SSH with a private key
- Install and verify Apache2 web server
- Edit live web content using nano
- Transfer files using `wget`, `sudo cp`, and `scp`
- Set file permissions using `chmod`
- Test network latency using `ping`
- Write and execute Bash scripts using `echo`, `for`, `if`, `read`, and system monitoring commands

---

## Part 1 — SSH into the Azure VM

Connected to the Azure VM from Windows Command Prompt using the downloaded private key. The key was reset via the Azure portal after the original key was not saved at creation time.

```bash
ssh -i C:\Users\user\Downloads\my-vm-key.pem azureuser@98.70.33.154
```

On first connection, confirmed the host fingerprint and accepted the connection. The terminal prompt changed to `azureuser@my-vm`, confirming a successful SSH session.

![SSH connected to Azure VM](lab-2a/01-ssh-connected.png)

---

## Part 2 — Update Package List

Updated the package index to ensure all installations use the latest available versions.

```bash
sudo apt update
```

![apt update output](lab-2a/02-apt-update.png)

---

## Part 3 — Install Apache2

Installed the Apache2 web server. The package manager resolved and installed all dependencies automatically.

```bash
sudo apt install apache2
```

![Apache2 installation](lab-2a/03-apache-install.png)

---

## Part 4 — Configure Network Security Group

By default, only port 22 (SSH) was open in the Azure Network Security Group. An inbound rule was added to allow HTTP traffic on port 80, making the web server publicly accessible.

| Rule | Port | Protocol | Action |
|------|------|----------|--------|
| SSH | 22 | TCP | Allow |
| HTTP | 80 | TCP | Allow |

![Networking rules — NSG inbound rules](lab-2a/04-networking-rules.png)

---

## Part 5 — Verify Apache in Browser

Accessed the VM's public IP address in a browser to confirm Apache2 was running and serving the default Ubuntu welcome page.

```
http://98.70.33.154
```

![Apache2 default page in browser](lab-2a/05-apache-browser.png)

---

## Part 6 — Edit index.html Using Nano

Opened the default Apache web page in the nano text editor to make a live modification.

```bash
sudo nano /var/www/html/index.html
```

![index.html open in nano](lab-2a/06-edit-index.png)

---

## Part 7 — Test Modified Page in Browser

After saving the changes in nano, the browser was refreshed to confirm the modification was live. The custom heading "Hello from my Audrey Azure WM" appeared on the page, confirming that edits to `/var/www/html/index.html` are served immediately without restarting Apache.

![Modified page in browser](lab-2a/07-modified-browser.png)

---

## Part 8 — Replace index.html with Custom Page and Add Hyperlinks

Replaced the entire default Apache page with a clean custom HTML page containing anchor tags linking to external resources. The file was written directly using `tee` to avoid nano paste limitations.

```bash
sudo tee /var/www/html/index.html << 'EOF'
<!DOCTYPE html>
<html>
<head>
  <title>My Azure Web Server</title>
</head>
<body>
  <h1>Hello from my Azure VM!</h1>
  <p>This page is hosted on an Apache2 web server running on Ubuntu 24.04.</p>
  <h2>Useful Links</h2>
  <ul>
    <li><a href="https://ubuntu.com">Ubuntu Official Site</a></li>
    <li><a href="https://httpd.apache.org">Apache HTTP Server</a></li>
    <li><a href="https://azure.microsoft.com">Microsoft Azure</a></li>
  </ul>
</body>
</html>
EOF
```

![Custom page with hyperlinks in browser](lab-2a/08-hyperlinks-browser.png)

---

## Part 9 — Download and Copy Files Using wget and sudo cp

Downloaded an image file from a remote URL using `wget`, then copied it into the Apache web directory using `sudo cp`, demonstrating remote file retrieval and web directory management.

```bash
wget https://www.w3schools.com/html/pic_trulli.jpg
sudo cp pic_trulli.jpg /var/www/html/
```

![wget download and sudo cp output](lab-2a/09-wget-copy.png)

---

## Part 10 — Test Access from Mobile Device

Accessed the web server from a mobile phone browser to confirm public accessibility across different devices and networks.

![Web server accessed from mobile browser](lab-2a/10-mobile-browser.png)

---

## Part 11 — Network Latency Testing with ping

Used `ping` to test network latency from the VM to servers in different regions. All responses returned under 4ms because Google distributes its servers globally — requests hit the nearest Google node regardless of the domain suffix.

```bash
ping -c 4 google.com
ping -c 4 google.co.jp
ping -c 4 google.co.za
```

| Target | Avg Latency |
|--------|-------------|
| google.com | 3.497 ms |
| google.co.jp | 3.072 ms |
| google.co.za | 3.170 ms |

![ping latency test results](lab-2a/11-ping-test.png)

---

## Part 12 — File Transfer Using SCP

Transferred a file from the local Windows machine to the Azure VM using `scp`, demonstrating secure remote file copying with a private key.

```bash
scp -i C:\Users\user\Downloads\my-vm-key.pem C:\Users\user\Downloads\my-vm-key.pem azureuser@98.70.33.154:/home/azureuser/
```

![SCP file transfer output](lab-2a/12-scp-transfer.png)

---

## Part 13 — Set File Permissions Using chmod

Set restrictive permissions on the private key file using `chmod 600`, ensuring only the owner can read or write the file. This is standard security practice for SSH keys.

```bash
chmod 600 /home/azureuser/my-vm-key.pem
ls -la /home/azureuser/my-vm-key.pem
```

The output `-rw-------` confirms the correct permission set was applied.

![chmod 600 and ls -la output](lab-2a/13-chmod.png)

---

## Part 14 — Bash Lab: Navigate File System and Manage Files

### Part 14a — Create and Navigate Directories

Created a working directory structure for the Bash scripting exercises.

```bash
mkdir mylab
cd mylab
mkdir scripts docs
ls
```

![mkdir and directory navigation](lab-2a/14-mkdir-navigate.png)

### Part 14b — File Operations: Create, Copy, Rename

Created a file using `touch`, copied it with `cp`, and renamed the copy using `mv` to demonstrate core file management commands.

```bash
touch docs/notes.txt
cp docs/notes.txt docs/notes_backup.txt
mv docs/notes_backup.txt docs/renamed.txt
ls docs/
```

![File operations — touch, cp, mv, ls](lab-2a/14b-file-commands.png)

---

## Part 15 — Bash Script: hello_world.sh

Created a basic Bash script demonstrating the shebang line, `echo` output, and command substitution with `$(hostname)` and `$(date)`.

```bash
nano scripts/hello_world.sh
```

```bash
#!/bin/bash
echo "Hello, World!"
echo "This script is running on: $(hostname)"
echo "Current date and time: $(date)"
```

Made the script executable using `chmod 777` and ran it.

```bash
chmod 777 scripts/hello_world.sh
./scripts/hello_world.sh
```

![hello_world.sh execution output](lab-2a/15-hello-world-run.png)

---

## Part 16 — Bash Script: system_info.sh

Created a script using a `for` loop for a countdown, `read` for interactive input, and `if/elif/else` to evaluate the user's response.

```bash
#!/bin/bash
echo "System Information Script"
echo "========================="

for i in 3 2 1; do
  echo "Starting in $i..."
done

echo "Enter your name:"
read name

if [ "$name" = "" ]; then
  echo "No name entered."
elif [ "$name" = "admin" ]; then
  echo "Welcome, Administrator!"
else
  echo "Hello, $name! Welcome to $(hostname)."
fi
```

When run with the input "Audrey", the script completed the countdown and returned a personalised greeting.

![system_info.sh execution output](lab-2a/16-system-info-run.png)

---

## Part 17 — Bash Script: resource_monitor.sh

Created a monitoring script that accepts user input to control iteration count, then uses `free -h`, `df -h`, and `top` to report memory, disk, and CPU usage at each interval with a `sleep` delay between checks.

```bash
#!/bin/bash
echo "How many times to monitor? (e.g. 3):"
read iterations
for i in $(seq 1 $iterations); do
  echo "=== Check $i ==="
  echo "-- Memory --"
  free -h
  echo "-- Disk --"
  df -h /
  echo "-- CPU Load --"
  top -bn1 | grep "Cpu(s)"
  sleep 2
done
echo "Monitoring complete."
```

Run with 2 iterations — the output showed memory at 419Mi used of 846Mi, disk at 8% used, and CPU idle above 88%.

![resource_monitor.sh execution output](lab-2a/17-resource-monitor-run.png)

---

## Reflection Questions

**What were the benefits of cloud deployment over local virtualisation?**

Cloud deployment removes the need for local hardware provisioning. The VM was available within minutes, accessible from any device, and could be stopped when not in use to avoid charges. Local virtualisation requires dedicated hardware and is not remotely accessible by default.

**How does Apache serve files, and how did you verify this?**

Apache listens on port 80 for HTTP requests and serves files from `/var/www/html/`. Verification was done by accessing the public IP in a browser and confirming the correct page loaded, including after edits to `index.html`.

**What did you learn about file ownership and permissions?**

The `chmod 600` command sets a file to owner-read and owner-write only (`-rw-------`). This is the required permission for SSH private keys — if the key is world-readable, SSH will refuse to use it as a security measure.

**What risks are associated with leaving instances running?**

A running VM continues to accumulate compute charges even when idle. It also remains exposed to the internet, increasing the attack surface. Azure's auto-shutdown feature was configured to shut down the VM at 7:00 PM UTC to mitigate both risks.

**How would you explain the difference between DNS and /etc/hosts to a client?**

DNS is a distributed, globally accessible system that resolves domain names to IP addresses. `/etc/hosts` is a local file on each machine that can override DNS for specific entries — useful for testing or internal environments. Changes to `/etc/hosts` only affect the local machine, while DNS changes propagate globally.

**What command did you use to create a new directory?**

`mkdir` — for example, `mkdir mylab` creates a directory named mylab in the current location.

**How can you view the contents of a file without opening it in a GUI?**

Using `cat filename` to print the full file, or `less filename` to scroll through it page by page.

**What is the purpose of `chmod 777`?**

It grants read, write, and execute permissions to the owner, group, and all other users. It was used to make the shell scripts executable. In production, this would be too permissive — `chmod 755` or `chmod +x` is preferred.

**What does `#!/bin/bash` do at the start of a script?**

It is the shebang line — it tells the operating system to use `/bin/bash` as the interpreter when the script is executed directly. Without it, the system may not know which shell to use.

**What happens when invalid input is entered into a script?**

The `else` branch handles unexpected input. In `system_info.sh`, any name that is not blank or "admin" triggers the default greeting, so the script handles all cases without crashing.

**What output does `free -h` show?**

It displays current memory usage in human-readable format — total, used, free, shared, buffer/cache, and available RAM. On this VM it showed 846Mi total with 419Mi used.

**How would you monitor network bandwidth in a Bash script?**

Using tools such as `ifstat`, `nload`, or reading from `/proc/net/dev` directly. For example: `cat /proc/net/dev` shows cumulative bytes transmitted and received per interface, which can be sampled at intervals to calculate bandwidth.

---

## Issues Encountered

| Issue | Resolution |
|-------|------------|
| SSH private key not saved at VM creation | Reset key via Azure portal → Reset password → Generate new key pair → Download `.pem` |
| PowerShell did not recognise `ssh` command | Used Windows Command Prompt instead, which has native SSH support |
| Paste disabled in CMD after SSH connection | Switched to Windows Terminal, which supports `Ctrl+Shift+V` paste consistently |
| Commands merged into one line when pasting | Ran commands one at a time; used `tee` with heredoc for multi-line file creation |

---

## Outcome

- Launched and configured an Ubuntu 24.04 VM on Microsoft Azure
- Reset and downloaded an SSH private key via the Azure portal
- Added an HTTP inbound rule to the Network Security Group to allow port 80
- Installed Apache2 and served a live custom HTML page with hyperlinks
- Transferred files using `wget`, `sudo cp`, and `scp`
- Set correct file permissions using `chmod` and verified with `ls -la`
- Tested network latency with `ping` across multiple regional targets
- Created and executed three Bash scripts covering `echo`, `for`, `if/elif/else`, `read`, `free -h`, `df -h`, and `top`
- Verified public server access from both desktop and mobile browsers

---


