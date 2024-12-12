## COMMANDS FOR THE EVALUATION OF BORN2BEROOT
<!--------------------- SIGNATURE ----------->

### SIGNATURE

- In the path where the VM is located, run the command:
  ```
  sha1sum name_virtual_machine.vdi
  ```
  

<!--------------------- !end! SIGNATURE ----------->
<!--------------------- INTRODUCTION ----------->
- Show the partitions
  ```
  lsblk
  ```

- Check that no graphical interface is in use
  ```
  ls /usr/bin/*session
  ```

- Check if UFW service is in use
  ```
  sudo ufw status
  ```
  
  ```
  sudo service ufw status
  ```

- Check if SSH service is in use
  ```
  sudo service ssh status
  ```

- Verify the operating system
  ```
  uname -v
  ```

- Check if the machine's hostname is correct
  ```
  hostname
  ```

<!--------------------- !end! INTRODUCTION ----------->
<!--------------------- SSH, USER, GROUPS ----------->
___
### Connection via SSH

- Show the IP address
  ```
  hostname -I
  ```
- Connect to the VM
  ```
  ssh ldummer-@ip_address -p 4242
  ```
___

### USERS AND GROUPS

- Check if the user is within the "sudo" and "user42" groups.
  ```
  getent group sudo
  ```
  ```
  getent group user42
  ```

- Create a new user
  ```
  sudo adduser user_name
  ```

- Create a new group
  ```
  sudo addgroup group_name
  ```

- Add the user to the group
  ```
  sudo adduser user_name group_name
  ```

- Verify if it was correctly added
  ```
  getent group group_name
  ```
- Delete the group
  ```
  sudo groupdel <group_name>
  ```

- Modify the hostname
  ```
  sudo nano /etc/hostname
  ```

  ```
  sudo nano /etc/hosts
  ```

  ```
  sudo reboot
  ```

  ```
  hostname
  ```

<!--------------------- !end! SSH, USER, GROUPS ----------->
<!--------------------- SUDO ----------->
### SUDO

- Verify if 'sudo' is installed
  ```
  sudo --version
  ```
  ou
  ```
  dpkg -s sudo
  ```

- Add the new_user to the 'sudo' group
  ```
  sudo adduser user_name sudo
  ```

  ```
  getent group sudo
  ```

- Show 'sudo' rules
  ```
  cd /var/log/sudo && vim /etc/sudoers.d/sudo_config
  ```

- Show the history of commands used with sudo
  ```
  cd /var/log/sudo && cat sudo_config
  ```

- look for PASS_ [...]
  ```
  vim /etc/login.defs
  ```

  ```
  vim /etc/pam.d/common-password
  ```

<!--------------------- !end! SUDO ----------->
<!--------------------- PORT ----------->
### PORT

- List the active rules in UFW
  ```
  sudo ufw status numbered
  ```

- Create a new rule
  ```
  sudo ufw allow 8080
  ```

- Delete a rule
  ```
  sudo ufw delete rule_number
  ```
- To deny a rule
  ```
  sudo ufw deny rule_number
  ```

- Check the ssh service and that it only works on port 4242
  ```
  sudo service ssh status
  ```

- Modify crontab
  ```
  sudo crontab -u root -e
  ```

- Stop/start the script
  ```
  sudo /etc/init.d/cront stop
  ```
<!--------------------- !end! PORT ----------->


  


