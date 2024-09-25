# 2420-Assignment-1 
---
Name: Qiyu (Ivy) Chen  

StudentID: A01338328

---

# Setting up a  Remote Sever with DigitalOcean  

## Learning Objecitves:

In this tutorial, you will be guided through the process of creating and managing a remote server using DigitalOcean. You will learn how to install and configure doctl on an existing droplet, generate SSH keys, and use them to securely connect to a remote server. Additionally, you will create a Droplet running Arch Linux, configure it with doctl and cloud-init, and apply best practices for server management and security.<mark>(to be changed)

 By compeleting this tutorial, you will learn: 

- Creating and managing a remote server with DigitalOcean

- Using the web console or the command line (`doctl`) to create and manage a remote server.

- Understand and use cloud-init to configure DigitalOcean Droplets.

- Using SSH to connect to a remote server and manage it securely.

- Generating and managing SSH keys for authentication.

- Applying best practices for security and access control in cloud environments.

## Prerequisites

- You have a DigitalOcean account.

- You have a Basic Knowledge of Linux.

- <mark>add more</mark>

## Table of Contents

1. Creating an SSH keys on your Local Machine

2. Adding SSH Key to DigitalOcean

3. Creating a Project and Droplet in DigitalOcean

4. Installing `doctl` Command-line Tool

5. Creating a Droplet Using `doctl`

6. Using cloud-init to Configure Your Droplet

7. <mark>add more

8. Glossary

9. References


## 1. Creating an SSH key pair on your Local Machine

### What is SSH?

- The Secure Shell (SSH) protocol is a method for securely sending commands to a computer over an unsecured network. SSH uses cryptography to authenticate and encrypt connections between devices.

- SSH also allows for tunneling, or port forwarding, which is when data packets are able to cross networks that they would not otherwise be able to cross. SSH is often used for controlling servers remotely, for managing infrastructure, and for transferring files.

### Why use SSH?

- SSH offers enhanced security through public key cryptography, which protects against unauthorized access and is more resistant to brute force attacks. It eliminates the risk of password interception during login, as SSH keys are not transmitted over the network. 
 
- SSH allows for convenient logins without needing to enter a password each time and provides better access control, enabling easy management and revocation of user access. 

To create a SSH key pair on your local machine:
1. Open your Terminal.

2. Run the following command to create your SSH key pair:
```
ssh-keygen -t ed25519 -f ~/.ssh/do-key -C "your_email@example.com"
```

t ed25519: Specifies the type of key to create (ed25519 is recommended for security).

f ~/.ssh/do-key: Specifies the file in which to save the private key. The public key will be saved with the same name but with a .pub extension.

C "your_email@example.com": Adds a label to your key, typically your email address. 


3. Follow the prompts to generate your key pair. You can optionally set a passphrase. If you choose to set a passphrase, please do remember it.

4. Verify your SSH key pair:After creating the SSH key, you can check if your key was created successfully by listing contents of the .ssh directory:
```
ls ~/.ssh
```
You should see `do-key` (the private key) and `do-key.pub` (the public key) listed.








