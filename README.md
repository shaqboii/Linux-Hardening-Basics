# Linux-Hardening-Basics

This project demonstrates basic system hardening, setting up a basic firewall, account creation and controlling their privileges. I am using Ubuntu to complete this lab.

<h2>Step 1: System Update</h2>
We can update the update and upgrade the system and it's dependencies by running this command in the terminal:

```bash
sudo apt update && sudo apt upgrade -y
```
<img src="https://i.imgur.com/5uNTlTg.png" height="80%" width="80%" alt="Linux Hardening Lab"/>

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

<img src="https://i.imgur.com/tP9YCnT.png" height="80%" width="80%" alt="Linux Hardening Lab"/>

Our firewall clearly shows that we can receive SSH and HTTP traffic from anywhere successfully. Let's move on to the next step.

<h2>Step 3: Account Creation</h2>

Before we create a new account, let's observe the ```/etc/passwd``` file. This file displays vital account information.

<img src="https://i.imgur.com/rh9f3vQ.png" height="80%" width="80%" alt="Linux Hardening Lab"/>

Now, this can look a little intimidating. But don't let it fool you, this is far simpler than you think. Let's break it down one colon at a time.

The first section contains the user or service's name. Pretty understandable.

The ```x``` in the second field means that the password to this user can be found in another place. That is, the ```/etc/shadow``` file.

Next, we have the ```UID```, or User Identifier of the user or service. This is an effective way to identify what accounts are legitimate users and what are services. Simply put, accounts that have a UID of less than 1000 are services, and cannot be logged into. The only exception to this is the root user, with a UID of 0.

Following the UID is the ```GID```, or the Group Identifier. Unless otherwise configured, the GID will typically have the same number as its UID. If you are working in a large organization, you'll see many GIDs applied to different users based on level of privilege and responsibility.

Then we have the comment field. This will normally have the full name of the user or service. 

Finally, there is the default shell. The ```/bin/bash``` shell is a login shell, which is the default user login. However, shells such as ```/usr/sbin/nologin``` and ```/bin/false``` typically mean that they cannot be logged into, most likely because they are service accounts.

The ```/etc/passwd``` file can be a good way to find all available accounts on a system to check and see if there are any that do not belong, or verify if any that you have created do indeed exist. Now that we have covered basic account info, let's create a new account.

To do this, I will run the following command:

```bash
sudo adduser testuser
```

<img src="https://i.imgur.com/3uqUrj9.png" height="80%" width="80%" alt="Linux Hardening Lab"/>

I gave the account a strong password, but I want to know if it will be secure. Remember how I mentioned how ```/etc/shadow``` contains account passwords? Well, we can check the level of encryption on our password by printing the contents of the file. Let's cat the file by running ```sudo cat /etc/shadow```.

<img src="https://i.imgur.com/kWkW2dx.png" height="80%" width="80%" alt="Linux Hardening Lab"/>

As we can see, my main account, shaqboi, has a ```$6$``` symbol in front of the encrypted password. This means that the password for my account is encrypted via ```SHA-512```, which is good. As for my testuser, it has a ```$y$``` in front of the encrypted password. This is ```Yescrypt```, which is a very good encryption algoritm and is very strong against local offline attacks. With account creation and information covered, let's move on to the next step.

<h2>Step 4: Privilege Escalation</h2>

Let's assume that this testuser was a part of our organization and had admin privileges. To give him admin privileges, from my main account, I run ```sudo usermod -aG sudo testuser```. 

When I log of Ubuntu, there is a new account that I can log into. Log into the testuser account.

Now, let's assume that that this user was feeling a little malicious and wanted to mess with us. What if they decided to mess with our firewall?

In this scenario, testuser will deny SSH, or port 22 traffic, on the firewall. To do this, testuser will escalate themselves to a sudo user, and then run the following:

```bash
sudo ufw deny 22/tcp
```
<img src="https://i.imgur.com/CAsZ7y3.png" height="80%" width="80%" alt="Linux Hardening Lab"/>

Now I, as the network admin, would typically be notified of this. I will need to verify that our firewall rule has been changed. To do this, I will run:

```bash
sudo ufw status
```
<img src="https://i.imgur.com/gjYRzTh.png" height="80%" width="80%" alt="Linux Hardening Lab"/>

As we can see, SSH traffic is denied. Now that I have verified that our firewall has been changed, I'll lock testuser's account to prevent any further damage. I will run:

```bash
sudo passwd -l testuser
```

This will lock the account. I can verify that the account is locked by checking the ```/etc/shadow``` file.

<img src="https://i.imgur.com/32uQBQS.png" height="80%" width="80%" alt="Linux Hardening Lab"/>

Before showing the encryption flag, there is an exclamation point. This means that the account is locked, and cannot be logged into. Now that we have secured our environment from testuser, let's make amends to our firewall.

As we have done before, we can re-enable SSH traffic on our firewall by running ```sudo ufw allow 22/tcp```. After doing so, let's verify and check that it's up by running ```sudo ufw status```.

<img src="https://i.imgur.com/IJhdoL5.png" height="80%" width="80%" alt="Linux Hardening Lab"/>

As we can see, the SSH traffic is enabled and up as intended. 

After giving testuser a little "disciplinary action", they have apologized for their actions. To show our mercy, let's unlock their account.

```bash
sudo passwd -u testuser
```
<img src="https://i.imgur.com/45OoGGC.png" height="80%" width="80%" alt="Linux Hardening Lab"/>

That concludes this lab.

<h2>Conclusion</h2>

This lab, while being fairly simple, taught me a lot of things. Let's list the things we covered:

- System updating and upgrading.
- Basic firewall setup.
- Account creation, information, and security.
- Adding a user to the sudo users.
- Changing firewall rules
- Controlling user accounts.


