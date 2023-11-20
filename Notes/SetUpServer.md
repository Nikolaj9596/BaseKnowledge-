# Base SetUp Prod server

## Configure Firewall:

```bash
sudo ufw enable
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
sudo ufw allow 2222/tcp
```
## Create new user:

```bash
adduser user
sudo passwd user
sudo usermod -aG sudo user
```
## Added new user to sudo file
```bash
sudo visudo
```
Add the "developer" user to the sudoers file: In the visudo editor, add a line to grant sudo privileges to the "developer" user. The line should look like this:
```bash
username ALL=(ALL:ALL) ALL
```
Replace "developer" with the actual username if it's different.

Save and exit the editor: In visudo, you can save your changes by pressing Ctrl + O, then press Enter. To exit, press Ctrl + X.

Test sudo access: After adding the user to the sudoers file, you can now test their sudo access by running a command with sudo. For example:

## Change ssh port

```bash

sudo nvim /etc/ssh/sshd_config
```
Change the Port: Change the port number to your desired port. For example, you can change it to 2222:
```bash
Port 2222
```

Restart the SSH service: After making the change, you need to restart the SSH service to apply the new configuration:
```bash
sudo systemctl restart sshd
```

