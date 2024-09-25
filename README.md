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

## 2. Adding SSH Key to DigitalOcean

After creating your SSH key pair, the next step is to add the public key to your DigitalOcean account. This allows you to securely connect to your droplets without needing to enter a password.

1. Copy your SSH public key: On your terminal, run the following command to direclty copy your public key:
```
cat ~/.ssh/do-key.pub
```
2. Once copied your key, log in to your DigitalOcean account (https://www.digitalocean.com/)

3. In the DigitalOcean dashboard, click on the "Settings" option in the left sidebar.

4. Then, select "Security" tab at the top.

5. Look for the "Add SSH Key" button and click it.

6. Paste your copied public key in the provied text box. In the "Name your key" field, give a recognizable name for your key. 

7. Click "Add SSH key" button to save your to save your public key to your DigitalOcean account.

8. After adding the key, you should see it listed in the SSH Keys section. This confirms that your SSH key is now associated with your account and ready to use for connecting to your droplets.

## 3. Creating a Project and Droplet in DigitalOcean

In this step, youwill create a project in DigitalOcean to organize your resources. You will also create a droplet, which is a virtual private server that will run your applications.

To create project: 
1. In the DigitalOcean dashboard, click on "+ New Project" located on the left side bar. 

2. In the "Project Name" field, enter a name for your project (e.g., "My Arch Linux Server").
- You can optionally add a description to help you remember what this project is about.

3. Click "Create Project" to save your new project.

4. You should see your created project appear on the left side bar under "Projects" .

To created a droplet:

1. From your project dashboard, click the "Create“ button and select "Droplets" in the dropdown menu. 

2. In the "Choose Region" section, choose San Francisco as this is the closest region to Vancouver.

3. Under the "Choose an image" section, select "Custom images", click "Add image" and upload the Arch Linux that you have previously downloaded. 

4. In the "Choose a plan" section, select the plan that suits your needs. 

5. Under the "Authentication" section, choose the "SSH Keys" option, and select the SSH key you added earlier from the list.

6. You can You can **optionally** choose additional settings like enabling backups, adding block based on your needs. 

7. Click the "Create Droplet" button at the bottom when you’re satisfied with your settings. 






















## Glossry
droplet  
ssh key

















