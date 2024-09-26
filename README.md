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

- Yoy have download the appropriate [Arch Linux](https://gitlab.archlinux.org/archlinux/arch-boxes/-/packages/)  image.  You want the most recent "cloudimg". The ".qcow2" like in the image below, the date may be different. Download the most recent image.

![Arch Image](2420-Assignment-1/assets/arch-linux.png)

## Table of Contents

1. Creating an SSH keys on your Local Machine

2. Adding SSH Key to DigitalOcean

3. Creating a Project and Droplet in DigitalOcean

4. Installing `doctl` Command-line Tool

5. Creating cloud-init file

6. Creating a Droplet Using doctl (with the cloud-init file)




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
2. Once copied your key, log in to your [DigitalOcean](https://www.digitalocean.com/) account. 

3. In the DigitalOcean dashboard, click on the "Settings" option in the left sidebar.

4. Then, select "Security" tab at the top.

5. Look for the "Add SSH Key" button and click it.

6. Paste your copied public key in the provied text box. In the "Name your key" field, give a recognizable name for your key. 

7. Click "Add SSH key" button to save your to save your public key to your DigitalOcean account.

8. After adding the key, you should see it listed in the SSH Keys section. This confirms that your SSH key is now associated with your account and ready to use for connecting to your droplets.

## 3. Creating a Project and Droplet in DigitalOcean

In this step, you will create a project in DigitalOcean to organize your resources. You will also create a droplet, which is a virtual private server that will run your applications.

To create project: 
1. In the DigitalOcean dashboard, click on "+ New Project" located on the left side bar. 

2. In the "Project Name" field, enter a name for your project.
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

8. You can view your droplet under "Resources" tab in your project homepage. 

## 4. Installing `doctl` Command-line Tool

### What is `doctl`

- `doctl` is the official DigitalOcean command line interface (CLI). It allows you to interact with the DigitalOcean API via the command line. 

### Why Install `doctl`
- It supports most functionality found in the control panel. You can create, configure, and destroy DigitalOcean resources like Droplets, Kubernetes clusters, firewalls, load balancers, database clusters, domains, and more.
https://docs.digitalocean.com/reference/doctl/

1. To install the latest version of doctl using Homebrew on macOS, run:
```
brew install doctl
```
2. Create an API token
- Create a [DigitalOcean API token](https://docs.digitalocean.com/reference/api/create-personal-access-token/) for your account with read and write access from the Applications & API page in the control panel. The token string is only displayed once, so save it in a safe place

3. Use the API token to grant account access to doctl

- Use the API token to grant doctl access to your DigitalOcean account. Pass in the token string when prompted by doctl auth init, and give this authentication context a name.
```
doctl auth init --context <NAME>
```
- Authentication contexts let you switch between multiple authenticated accounts. You can repeat steps 2 and 3 to add other DigitalOcean accounts, then list and switch between authentication contexts
```
doctl auth list
doctl auth switch --context <NAME>
```
4. Validate that doctl is working

Now that doctl is authorized to use your account, try some test commands.

To confirm that you have successfully authorized doctl, review your account details by running:
```
doctl account get
```
If successful, the output looks like:
```
Email                      Droplet Limit    Email Verified    UUID                                        Status
sammy@example.org          10               true              3a56c5e109736b50e823eaebca85708ca0e5087c    active
```

## 5. Creating cloud-init file

### What is cloud-init?
- Cloud-init is a widely used tool for initializing cloud instances across various platforms. It automatically configures systems during the first boot by detecting the cloud environment and setting up essential aspects such as networking, storage, SSH keys, and packages.

### Why use Cloud-init?
- Cloud-init allows us to set up a server with initial configurations quickly. For instance, after creating and connecting to servers, we often need to run a few commands to update and install packages. While this will not take long time for one server, configuring 10 or 100 servers can be time-consuming.
- The easiest way to apply server configurations with cloud-init is through a config file, which is simply a YAML file. YAML is a human-friendly data serialization language for all programming languages.This allows for automated and consistent setup across multiple servers, saving time and reducing manual effort.

1. To Create a Cloud-Init FIle:
While logged into your droplet, use a text editor to create a new file named cloud-init.yaml. You can use nano, vim, or any other text editor you prefer. For example:
```
nano cloud-init.yaml
```
2. In the cloud-init.yaml file, you will define the settings you want for your droplet. Simply copy and paste the following example into your cloud-init configuration file and make changes as needed. 
```
#cloud-config
users:
  - name: user-name     #change to your desired name
    primary_group: group-name    #The primary group for the user
    groups: wheel     #Additional groups, granting sudo privileges
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']     #Default shell for the user
    ssh-authorized-keys:
      - ssh-ed25519 ...    #Replace with your actual SSH public key

packages:
  - ripgrep
  - rsync
  - neovim
  - fd
  - less
  - man-db
  - bash-completion
  - tmux

disable_root: true     #Disable root login via SSH for enhanced security
```
3. Save and Exit the File:

If you're using Vim:

Press Esc to enter command mode.
Type :wq (stands for write and quit) and then press Enter. This will save the file and exit the editor.

If you're using Nano:

Press Ctrl + O to save the file. You’ll be prompted to confirm the filename (just press Enter).
Then press Ctrl + X to exit the editor.

Now that your cloud-init.yaml file is saved and properly formatted, you can proceed with creating a Droplet using this configuration!

## 6. Creating a Droplet Using doctl with the cloud-init File

With your cloud-init configuration file ready, you can use it to create a new DigitalOcean Droplet. This will automate the initial setup, such as creating a user, installing packages, and configuring security settings.

Reminder: Before creating a droplet, make sure you have the follong set up: 
- You have doctl installed and configured on your local machine (covered in Step 4).
- You have a cloud-init YAML file (cloud-init.yaml) ready with your desired configuration (covered in Step 5).
- You have a DigitalOcean API token added and authenticated with doctl.

1. To creat a droplet,run the following command:
```
doctl compute droplet create "my-arch-droplet" \
--size s-1vcpu-1gb \
--region sfo3 \
--image <YOUR-CUSTOM-IMAGE-ID> \
--ssh-keys <YOUR-SSH-KEY-ID> \
--user-data-file cloud-init.yaml \
--wait
```
- "my-arch-droplet": Replace this with your desired droplet name.
- "nyc3": Replace this with your desired region. For example, use "sfo3" for San Francisco or "nyc3" for New York.
- "s-1vcpu-1gb": Replace this with the droplet size you need. For a basic plan, this example uses a droplet with 1 vCPU and 1 GB of RAM.
- "arch-linux-iso": Replace this with the image slug or ID for your custom Arch Linux image.  

    - If you don't remember the exact image ID (or slug) for your custom Arch Linux image, you can list all available images using the following command:
    ```
    doctl compute image list-user
    ```
    This will show a table of all custom images associated with your account. Look for the column labeled ID or Slug and find the corresponding value for your Arch Linux image. Use this value in the --image option of the droplet creation command.

- "<your-ssh-key-fingerprint>": Replace this with your SSH key fingerprint. You can find this value using the command doctl compute ssh-key list.
- --user-data-file cloud-init.yaml: This option specifies the path to your cloud-init configuration file.
- --wait: This option waits for the droplet to be fully created before returning control to your terminal.

Example here

2. Verify the Droplet Creation

After executing the command, you will see the progress and the details of your new droplet, including its public IP address.

You can also use this command to list all your droplets and verify that the new one has been created successfully:
```
doctl compute droplet list
```
3. Connecting to Your Droplet

Once the droplet is created and active, you can connect to it using SSH with the following command:
```
ssh user-name@<droplet-ip>
```
- Replace user-name with the username you specified in the cloud-init.yaml file.
- Replace <droplet-ip> with the public IP address of your droplet, which you will find in the output of the droplet creation command or the doctl compute droplet list command.  


Congratulations! You have successfully created a new DigitalOcean droplet using the doctl command-line tool and your cloud-init configuration. 



## Glossry
droplet  
ssh key
API token," "YAML," "sudo

















