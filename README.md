# Ansible_Practice
Practice project for learning Ansible, the main objective is to control other machine (Probably VMWare Worstation VMs) to boot/maintain services

# Environnement

~~[OLD] Installed Ansible on WSL Ubuntu, VMWare Workstation installed on Windows host~~

> [!NOTE]
> Since WSL is in NAT network, it has it's own network range, and thus cant communicate with VMWare VM

Two Ubuntu VM on VMWare Workstation connected via bridge network

## Adding certification to linux

> [!NOTE]
> This part isn't part of the actual installation, just some extra steps I need to take since I've got some restriction on my hardware, but in theory on your own setup you can just skip this part

- Get the Run dialogue box by using `Windows + R`
- Type `certmgr.msc` and press Enter
- Look for the certificate that you need (in my case it was the company root certificate) and export it
- You should have a file `mycert.cer`, import it to the machine where you want the cert to be installed
- Move your certificate to this folder : `/usr/local/share/ca-certificates/`
- Give your file the appropriate permission `sudo chmod 644 /usr/local/share/ca-certificates/mycert.crt`
- Use the command `sudo update-ca-certificates` to update your CA certificate list

> [!TIP]
> In WMware, you should be able to copy/paste it or drag and drop it, but for some reason I couldn't do it so I used an USB drive to transfer the file

> [!IMPORTANT]
> We need to convert the certificate to PEM format (most Linux system expect .crt format for certificate)\
> If your file looks like this :
> ```
> -----BEGIN CERTIFICATE-----
> ....
> -----END CERTIFICATE-----
> ```
> Then your file is already in PEM format, just rename it `mv mycert.cer mycert.crt`\
> \
> If the file looks like unreadable binary, it’s likely in DER format.\
> Convert it using openssl `openssl x509 -inform DER -in certificate.cer -out certificate.crt`

## Installing Ansible

The actual installation is very well documented, you can find it [here](https://docs.ansible.com/projects/ansible/latest/installation_guide/intro_installation.html)

Simple installation `pipx install --include-deps ansible`
> [!NOTE]
> Make sure you are root when using Ansible

## Installing OpenSSH

Ansible uses an agentless ssh connection when executing a task. For this to work we need to install an SSH server to our node, in my case I chose OpenSSH for it's ease to install and configure.

To install OpenSSh simply use the command  `sudo apt install openssh-server` ans open port 22 with `sudo ufw allow 22`

We then need to create our ssh key with `ssh-keygen` and bla bla [TODO] Finish writing after making sure to know how this works

## Running Ansible Playbook
Inventory can be both yml or ini file ([doc](https://docs.ansible.com/projects/ansible/latest/inventory_guide/intro_inventory.html))

To build my inventory, I used an ini file, in order to avoid having raw ip addresses in my inventory file, I added the ip address of my node in my /etc/hosts file. 

To run playbook with inventory of local host, use `localhost Ansible_connection=local` in the inventory file

Run Playbook : `ansible-playbook playbook.yml -i inventory.ini`
