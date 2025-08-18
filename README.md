# Linux-Hardening-Basics

This project demonstrates basic system hardening, setting up a basic firewall, account creation and controlling their privileges. I am using Ubuntu to complete this lab.

<h2>Step 1: System Update</h2>
We can update the update the system and it's dependencies by running this command in the terminal:

```bash
sudo apt update && sudo apt upgrade -y
```
<img src="https://i.imgur.com/5uNTlTg.png" height="80%" width="80%" alt="Building Network Lab"/>

After doing so, our system will update. A restart may be required.

Next, let's configure a simple firewall. To do this, I will 
