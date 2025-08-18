# Linux-Hardening-Basics

This project demonstrates basic system hardening, setting up a basic firewall, account creation and controlling their privileges. I am using Ubuntu to complete this lab.

<h2>Step 1: System Update</h2>
We can update the update and upgrade the system and it's dependencies by running this command in the terminal:

```bash
sudo apt update && sudo apt upgrade -y
```
<img src="https://i.imgur.com/5uNTlTg.png" height="80%" width="80%" alt="Building Network Lab"/>

After doing so, our system will be updated. A restart may be required.

<h2>Step 2: Firewall Configuration</h2>

Next, let's configure a simple firewall. For the sake of simplicity, I am going to allow only SSH and HTTP traffic onto my machine. This means allowing ports 22 and 80.

To do this, I will run the following commands:

```bash
sudo ufw enable
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw status
```
The first command enables the firewall. We then allow the firewall to receive SSH and HTTP traffic via ports 22 and 80. Finally, we print the status of the firewall.

<img src="https://i.imgur.com/tP9YCnT.png" height="80%" width="80%" alt="Building Network Lab"/>
