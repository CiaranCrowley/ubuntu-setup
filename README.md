# Ubuntu Setup

## XRDP
1. Access the command line and update the Ubuntu packages list
```
sudo apt update
```

2. Install **xrdp** using this command
```
sudo apt install xrdp -y
```

3. Check the status of the **xrdp** server
```
sudo systemctl status xrdp
```

4. Configure a port for **xrdp**
- Use `nano` to edit the xrdp config ini
```
sudo nano /etc/xrdp/xrdp.ini
```
5. Locate the **port** directive in the *[Globals]* section and set a numeric value
```
port=49952
```
- Exit and save using **CTRL+X** followed by **y**.
6. Restart **xrdp** to apply the changes
```
sudo systemctl restart xrdp
```

### Open a port for incoming traffic
1. Check the status of `ufw` firewall
```
sudo ufw status
```
- If the firewall is inactive, use the following command to turn on ufw
```
sudo ufw enable
```
2. Allow traffic on port 3389 or choose a different port for your RDP connection. The following command allows RDP connections on port 49952
```
sudo ufw allow 49952/tcp
```
3. Reload the ufw firewall tool to apply the changes
```
sudo ufw reload
```

4.  To connect on windows, open the **Remote Desktop Connection** app and enter the IP address of the machine you are trying to remote into
```
IP_ADDRESS:PORT
```

5.  You will be prompted to enter the login info of the machine you are trying to remote into.

## Mount a NAS

1. Install **cifs-utils**
```
sudo apt update
sudo apt install cifs-utils
```

2. Create a mount point
```
sudo mkdir -p /mnt/nas
```

3. Create a credentials file
```
sudo nano /etc/samba/nas-credentials
```

4. Add the following lines 
```
username=your_nas_username
password=your_nas_password
```

5. Secure it so that only the root user can read it
```
sudo chmod 600 /etc/samba/nas-credentials
```

6. Edit the **/etc/fstab** file
```
sudo nano /etc/fstab
```

7. Add the following line at the end of the file
```
//nas_ip_address_or_hostname/share_name /mnt/nas cifs credentials=/etc/samba/nas-credentials,uid=1000,gid=1000,file_mode=0755,dir_mode=0755 0 0
```
- Replace nas_ip_address_or_hostname with the IP address or hostname of your NAS.
- Replace share_name with the name of the shared folder on your NAS.
- Adjust uid=1000 and gid=1000 to match your user and group ID (usually 1000 for the first user on a system).

8. Mount the NAS
```
sudo mount -a
```

9. Verify the mount:
```
ls /mnt/nas
```

## Install Git (Optional)
```
sudo apt install git

git --version

git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"

git config --global --list

```