## COMMANDS FOR THE EVALUATION OF BORN2BEROOT

<!------------------- SUMMARY --------------->
### SUMMARY

- [Signature](#signature)<br>
- [Connect via SSH](#connection-via-ssh)<br>
- [Users and Groups](#users-and-groups)<br>
- [Sudo](#sudo)<br>
- [Port](#port)<br>
- [Script](#script)<br>
<!------------------- !end! SUMMARY --------------->
<!--------------------- SIGNATURE -----------><br>
___ 
### SIGNATURE

- In the PATH where the VM is located, run the command:
  ```
  sha1sum name_virtual_machine.vdi
  ```
>[!TIP]
>During the evaluation:
>- generate the signature code
>- paste in an empty .txt file
>- copy the content cloned from the file downloaded from intra
>- paste this content in the search (ctrl + f) to compare
>  

<br>
<!--------------------- !end! SIGNATURE ----------->
<!--------------------- INTRODUCTION ----------->

- LAST BOOT time
```
who -b
```

- Show the PARTITIONS
  ```
  lsblk
  ```

- Check that no graphical INTERFACE is in use
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

- Verify the OPERATION system
  ```
  uname -v
  ```

- Check if the machine's HOSTNAME is correct
  ```
  hostname
  ```

<!--------------------- !end! INTRODUCTION ----------->
<!--------------------- SSH, USER, GROUPS ----------->
___
### CONNECTION VIA SSH

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

- Check if the USER is within the "sudo" and "user42" GROUPS.
  ```
  getent group sudo user42
  ```

- Create a NEW USER
  ```
  sudo adduser user_name
  ```

- Create a NEW GROUP
  ```
  sudo addgroup group_name
  ```

- ADD the user to the group
  ```
  sudo usermod -aG groupname username
  ```

- VERIFY if it was correctly added
  ```
  getent group group_name
  ```
- DELETE the group
  ```
  sudo groupdel <group_name>
  ```

- MODIFY the hostname
  ```
  sudo vim /etc/hostname
  ```
  
<!--
  ```
  sudo nano /etc/hosts
  ```
  -->
- REBOOT the system
  ```
  sudo reboot
  ```

  ```
  hostname
  ```

<!--------------------- !end! SSH, USER, GROUPS ----------->
<!--------------------- SUDO ----------->
___
### SUDO

- Verify if 'sudo' is installed
  ```
  sudo --version
  ```
  ou
  ```
  dpkg -s sudo
  ```

- Show 'sudo' RULES
  ```
  cd /var/log/sudo && vim /etc/sudoers.d/sudo_config
  ```

- Show the LOG [history] of commands used with sudo
  ```
  cd /var/log/sudo && cat sudo_log
  ```

- Check password EXPIRE rules
```
sudo chage -l username
```
- Control the 'AGE' of the pass. Look for PASS_MAX [...]
  ```
  vim /etc/login.defs
  ```
  
- show the functions of the libpam-pwquality plugin
   'shortcuts' to implement the password policy
  ```
  vim /etc/pam.d/common-password
  ```

<!--------------------- !end! SUDO ----------->
<!--------------------- PORT ----------->
___
### PORT

- List the ACTIVE rules in UFW
  ```
  sudo ufw status numbered
  ```

- Create a NEW rule
  ```
  sudo ufw allow 8080
  ```

- DELETE a rule
  ```
  sudo ufw delete rule_number
  ```
- To DENY a rule
  ```
  sudo ufw deny rule_number
  ```

- Check the ssh service and that it only works on port 4242
  ```
  sudo service ssh status
  ```

- Modify CRONTAB
  ```
  sudo crontab -u root -e
  ```

- Stop/start the script
  ```
  sudo /etc/init.d/cront stop
  ```
<!--------------------- !end! PORT ----------->
<!---------------------- SCRIPTS ------------->
___
### SCRIPT

- PATH
  ```
  cd /usr/local/bin
  ```
- modify the crontab file
  ```
  sudo crontab -u root -e
  ```
- start/stop crontab
  ```
  sudo /etc/init.d/cront stop 
  ```
  
<!---------------------- !end!SCRIPTS ------------->

  


