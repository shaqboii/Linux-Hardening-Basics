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

Our firewall clearly shows that we can receive SSH and HTTP traffic from anywhere successfully. Let's move on to the next step.

<h2>Step 3: Account Creation</h2>

Before we create a new account, let's observe the ```/etc/passwd``` file. This file displays vital account information.

<img src="https://i.imgur.com/rh9f3vQ.png" height="80%" width="80%" alt="Building Network Lab"/>

Now, this can look a little intimidating. But don't let it fool you, this is far simpler than you think. Let's break it down one colon at a time.

The first section contains the user or service's name. Pretty understandable.

Now, the ```x``` in the second field means that the password to this user can be found in another place. That is, the ```/etc/shadow``` file.

Next, we have the ```UID``` of the user or service. This is an effective way to identify what accounts are legitimate users and what are services. Simply put, accounts that have a UID of less than 1000 are services, and cannot be logged into. The only exception to this is the root user, with a UID of 0.

Then we have the comment field. This will normally have the full name of the user or service. 

Finally, there is the default shell. The ```/bin/bash``` shell is a login shell, which is the default user login. However, shells such as ```/usr/sbin/nologin``` and ```/bin/false``` typically mean that they cannot be logged into, most likely because they are service accounts.

The ```/etc/passwd``` file can be a good way to find all available accounts on a system to check and see if there are any that do not belong, or verify if any that you have created do indeed exist. Now that we have covered basic account info, let's create a new account.
