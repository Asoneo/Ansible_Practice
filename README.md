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
