<!----------------------- INTRODUCTION ----------------------->

## 📝 INTRODUCTION

+ **OBJECTIVE**

  Create and configure a Virtual Machine.

>A Virtual Machine is a software that emulate the hardware of a computer. It is very usefull when in a work you need to test something in a diferent system without having another machine ou changing your.
<br>
<br>

>[!WARNING]
>Before start the project, check the section [Bonus](#bonus) to decide if you will do it or not.<br>
>Then, check the bonus requirements listede there to garantee that you have enough space.

  <br>
<details>
<summary> 

  #### ⚠️ REQUIREMENTS
</summary>

  - Operating system: choose the latest stable version of Debian or Rocky.
    - SELinux must be running at startup. AppArmor for debian must be running at startup too.
  - Create af least 2 encrypted partitions using LVM (This differs in Bonuses).
  - A SHH service must be running on the mandatory por 4242 in the VM.
  - Configure the operating system with the UFW firewall and leave only port 4242 open in the VM.
  - The hostname bus be the login in 42 school ending with 42.
  - Implement a strong password policy.
  - Install and configure sudo.
  - Must have the root user and a user with your login as username. This user has to belong to the user42 and sudo groups.

**Other requirements will be described during the steps.**
    </details>  
    
<!----------------------- !end! INTRODUCTION -----------------------> 
<!----------------------- SUMMARY OF STEPS ----------------------->

<details open>
<summary>
  
  #### 📑 STEP BY STEP
</summary>

- [Downloading the Virtual Machine](#downloading-the-virtual-machine)<br>
- [Installing Debian](#installing-debian)<br>
- [Partition Disks](#partition-disks)<br>
- [Setting up the Virtual Machine](#setting-up-the-virtual-machine)<br>
- [Sudo, Users and Groups](#sudo-users-and-groups)<br>
- [Installing SSH and VIM](#installing-ssh-and-vim)<br>
- [Install and configure UFW Firewall](#install-and-configure-ufw-firewall)<br>
- [SUDO POLICY](#sudo-policy)<br>
- [Strong password policy](#strong-password-policy)<br>
- [Script](#script)<br>


- [Bonus](#bonus)<br>
- [Commands for the evaluation](/COMMANDS.md)<br>
- [References](#references)<br>

</details>

<!----------------------- !end! SUMMARY OF STEPS ----------------------->
<!----------------------- INSTALLING THE VIRTUAL MACHINE ----------------------->

## DOWNLOADING THE VIRTUAL MACHINE

  - Download the image(iso) for [Debian](https://cdimage.debian.org/debian-cd/current-live/amd64/iso-hybrid/).
   <br> In my case, I choose `debian-live-12.8.0-amd64-standard`.
  - Open `Virtual Box` and click `new`.
  - In 42 school you need to store de VM inside `/sgoinfre/` directory because of the limited space.
> [!WARNING]
>First check the space available in `sgoinfre`.
><br>For the bonuses you will need 30GB. If this is not available, you can store the VM inside `goinfre` (without S).
><br>Note that in that case you only can access the VM in the same computer, because `goinfre` is a local directory.<br>
> To make the bonuses, you will need 60gb so you can clone the project.
  - Create an `user` and define a `password` (save for later). I choose to define all my passwords equals.
  - The **hostname** must be like your `user42`.
  - Select the amount of `RAM` and `processors` (this can be changed later).
  - Create a Virtual Hard Disk with 30gb if you will make the bonuses.
      - The HD type **VDI** is already configurated for this case, as you can see if you choose `Expert Mode` in the first windown.
  - After finishing, go to `Settings` and then to `Storage`.
  - Click in `Empty`. Click in the options for `Optical Drive` and search for your Debian iso file. Now save.
   
<!------------------------ !end! INSTALLING THE VIRTUAL MACHINE ----------------------->
<!----------------------- INSTALLING DEBIAN ----------------------->

## INSTALLING DEBIAN

- Language `English`
- Selection your location. To choose Portugal your need to select `other` > `Europe` > `Portugal`
- Configure the keyboard to `United States en_US.UTF-8` > `American English`
- Hostname for the network: `youruser42`
- In `Domain name` you can just type `enter` to scape this option.
- Choose a `root password` and take note!
- Create an account with username and password.
- Configure the clock to Lisbon.

## PARTITION DISKS
  
  Choose Manual if you will do the bonuses.

<h4>Primary Partition</h4>

- Select the partition. Click `YES`.
- Select `FREE SPACE` > `Create New Partition`.
- This must have `500m` of size. Select `Primary` > `Beginning`.
- Change the mount point to `/boot`.
- `Done setting up the partition`.

<h4>Logical Partition</h4>

- Select `FREE SPACE` > `Create New Partition`.
- For the partition size type `max` > `Logical`.
- Mount point: `Do not mount it`. 

<h4>Configure Encrypted Volumes</h4>

- "Write the changes to disk and configure encrypted volumes?" `YES`
- `Create encrypted volumes`.
- Select the device `/dev/sda5` > `Done` > `Finish`> `YES` > `CANCEL`.
- Create a password to encrypt sda. 

<h4>Configure the Logical Volume Manager</h4>

- "Write the changes to disk and configure LVM?" `YES`.
- `Create volume group` and name it `LVMGroup`. Choose `/dev/mapper/sda5_crypt`.

<h4>Creating the logical partitions</h4>

- `Create logical volume` > `LVMGroup`   

| name | size | How to use | Mout Point |
|------:|---------------|-------|-------|
|  root  |  10g  | ext4 | /root |
|  swap  |  2.3g  | swap | swap |
|  home  |  5g  | ext4 | /home |
|  var  |  3g  | ext4 | /var |
|  srv  |  3g  | ext4 | /srv |
|  tmp  |  3g  | ext4 | /tmp |
|  var-log  |  4g  | ext4 | /var/tog |

- `Finish partitioning andwrite changes to disk`
- "Use a network mirror?" `YES`
- `Portugal` > `deb.debian.org` > Leave http empty
- Accept the installation of `GRUB boot loader`. Choose the device.
- Wait and then choose to reboot.

>[!TIP]
>You can check the partitions typing in the terminal:
>`lsblk`

<!----------------------- !end! INSTALLING DEBIAN ----------------------->
<!----------------------- SETTING UP THE VIRTUAL MACHINE ----------------------->

## SETTING UP THE VIRTUAL MACHINE

- Enter your encryption password.
- Enter the user and password.

### SUDO, USERS AND GROUPS

  `user@hostname`
  
- You must be at the `root` user, so type `su -` in the terminal and enter the password.
- Type `apt install sudo`
- Reboot the system with `sudo reboot`
- Check the installation `sudo -V | more`

> [!IMPORTANT]
>**Create a New USER:** `sudo adduser <login>`<br>
>**Created a New GROUP:** `sudo addgroup user42`<br>
>**Include User in a Group:** `sudo adduser <user> <group>`<br>
>To check users and groups in root:<br>
> `getent group`<br>
> `getent group <groupname>`

### INSTALLING SSH AND VIM

- First update the system
```
sudo apt update
```

- Instal OpenSSH
```
sudo apt instal openssh-server
```
- Check the SSH service
```
sudo service ssh status
```
- Install VIM instead of using nano
```
sudo apt install vim
```

<h4>Configure the SSH</h4>
- Change to root `su -`
- Open `sshd_config`
```
vim /etc/ssh/sshd/config
```
- Set `Port` to `Port4242`
- Change `#PermitRootLogin no` to `PermitRoologin no`
- Save and close.

- Open `ssh_config`
```
vim /etc/ssh/ssh_config
```
- Set `Port` to `Port 4242`
- Restart and update
```
sudo service ssh restart
```
- Check service's state
```
sudo service ssh status
```

### INSTALL AND CONFIGURE UFW FIREWALL

```
sudo apt install ufw
```
- Start UFW with the comand `sudo ufw enable`
- Configure the firewall to accept connections on 4242 port
```
sudo ufw allow 4242
```
- Check the status
```
sudo ufw status
```
In case you need to change some connections, you can check the status with the command bellow. Remember that you only can be connected to port 4242, and no other.
```
sudo ufw status numbered
```
If you have other connections, you can delet it with
```
sudo ufw delete [rule_number]
```

### CONNECTING TO SSH

- Get the VM's IP `hostname -I`
- Close the VM and go to `Settings`
- Click on `Network` > `Advanced.
- Modify `Attached to:` from `NAT` to `Bridged Adapter` > `OK`.
- Open the VM.
- Open a new terminal in your machine and type:
```
ssh <user>@localhost -p 4242
```
- It will ask for the user password.
- To close just type `exit`
- This allows you to work in your virtual machine from the terminal of your machine.

### CLOSE UNNECESSARY CONNECTIONS
- Check system sockets
```
ss -tunlp
```
**FLAGS**:
  - `t` : display TCP connections
  - `u` : display UDP connections
  - `n` : do not resolve IP addresses and service's ports
  - `l` : display listening sockets
  - `p` : shows the the process that owns the socket

- Get IP address `ip --color addr
- Get system's network interfaces
```
vim /etc/network/interfaces
```
- Edit to look like:
```
# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
aut enp0s3
iface enp0s3 inet static
    address <ip_address>
    netmask 255.255.0.0
    gateway <ip_address>
    dns-nameservers 10.51.1.253
```

<!-----------------------  !end! SETTING UP THE VIRTUAL MACHINE ----------------------->
<!-----------------------  SUDO POLICY ----------------------->

### SUDO POLICY

- Create a file to store the configuration
```
touch /etc/sudoers.d/sudo_config
```
- Now open it
```
vim /etc/sudoers.d/sudo_config
```
- Add these Defaults
```
Defaults  passwd_tries=3
Defaults  badpass_message="Wrong password"
Defaults  logfile="/var/log/sudo/sudo_config"
Defaults  log_input, log_output
Defaults  iolog_dir="/var/log/sudo"
Defaults  requiretty
Defaults  secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
```

`passwd_tries` - limit to 3 the attempts for entering the sudo password <br>
`badpass_message` - choose a message in case the passwords do not match <br>
`logfile` - set a log file to archive the sudo actions (described in the subject)<br>
`log_input, log_output` - what will be logged <br>
`iolog_dir` - path where input and ouput will be stored<br>
`requiretty` - enable TTY<br>
`secure_path` -  <br> secure path. Separates `root path` from `user path`

- Create the directory to store the system's sudo log
```
mkdir /var/log/sudo
```

>[!NOTE]
>How to change the **hostname** in defense<br>
>Check the current hostname: `hostnamectl`<br>
>Change hostname: `hostnamectl set-hostname <hostname>`<br>
>Edit **/etc/hosts**: `sudo vim/etc/hosts/`<br>
>Just change the older to a new name<br>
>Reboot and check: `sudo reboot hstnamectl`<br>

<!----------------------- !end! SUDO POLICY ----------------------->
<!-----------------------  STRONG PASSWORD POLICY ----------------------->

### STRONG PASSWORD POLICY

- Edit `vim /etc/login.defs`
- Set the params to:
  >PASS_MAX_DAYS 30<BR>
  >PASS_MIN_DAYS 2<br>
  >PASS_WARN_AGE 7<br>

- Install the package **libpam-pwquality** to provide common functions for password quality checking. Also provides a functin for generating random passwords with good pronounceability.
  ```
  sudo apt install libpam-pwquality
  ```
- Open to configure
```
sudo vim /etc/pam.d/common-password
```
- In the section `per-package`, after `retry=3`, add the following options:
```
minlen=10
ucredit=-1
dcredit=-1
lcredit=-1
maxrepeat=3
reject_username
difok=7
enforce_for_root
```

>`-` means lower bound | `+` means upper bound

<br>`minlen` - minimun of characters
<br>`ucredit` - must contain at least one `uppercase` char. 
<br>`dcredit` - must contain at least one `digit`
<br>`lcredit` - must contain at least one `lowercase` letter
<br>`maxrepeat` - only can repeate consecutively a character `x` number of times
<br>`reject_username` - password cannot contain the username
<br>`difok` - password must contain at least `x` different characters from the previously used password
<br>`enforce_for_root` - implement password policy to root

<!----------------------- !end! STRONG PASSWORD POLICY ----------------------->
<!----------------------- SCRIPT ----------------------->

### SCRIPT

You need to create a script called `monitoring.sh`. The script will display some information (described in the subject) on all terminals every 10 minutes (from the moment you connect to the account, not in real time from the computer).

Place the `monitoring.sh` at `/usr/local/bin`
Test the script:
```
sudo /usr/local/bin/monitoring.sh
```


- Indicates to Unix what program to use to run the script<br>
`#!/bin/bash`

- Get system Architecture info<br>
`arch=$(uname -a)`

- Get system's number of Physical Cores<br>
`cpuf=$(grep "physical id" /proc/cpuinfo | wc -l)`

- Get system's number of Virtual Cores<br>
`cpuv=$(grep "processor" /proc/cpuinfo | wc -l)`

- Get amount of used RAM<br>
`ram_total=$(free --mega | awk '$1 == "Mem:" {print $2}')`<br>

- Get total amount of memory in the system<br>
`ram_use=$(free --mega | awk '$1 == "Mem:" {print $3}')`<br>

- Get used memory percentage<br>
`ram_percent=$(free --mega | awk '$1 == "Mem:" {printf("%.2f"), $3/$2*100}')`<br>

- Get amount of used disk memory<br>
`disk_total=$(df -m | grep "/dev/" | grep -v "/boot" | awk '{disk_t += $2} END {printf ("%.1fGb\n"), disk_t/1024}')`<br>

- Get system's total disk space<br>
`disk_use=$(df -m | grep "/dev/" | grep -v "/boot" | awk '{disk_u += $3} END {print disk_u}')`<br>

- Get used memory percentage<br>
`disk_percent=$(df -m | grep "/dev/" | grep -v "/boot" | awk '{disk_u += $3} {disk_t+= $2} END {printf("%d"), disk_u/disk_t*100}')`<br>

- Get percentage of CPU usage<br>
`cpul=$(vmstat 1 2 | tail -1 | awk '{printf $15}')`<br>
`cpu_op=$(expr 100 - $cpul)`<br>
`cpu_fin=$(printf "%.1f" $cpu_op)`<br>

- Get the date and time of last reboot<br>
`lb=$(who -b | awk '$1 == "system" {print $3 " " $4}')`

- Check if LVM is active<br>
`lvmu=$(if [ $(lsblk | grep "lvm" | wc -l) -gt 0 ]; then echo yes; else echo no; fi)`
>$(...) is a command substitution; Executes the command inside the parentheses and replaces it with it's output;<br>
In bash the -gt flag is the Greater Than Comparison Operator used for arithmetic operations in bash scripting;

- Check for the number of established TCP connections<br>
`tcpc=$(ss -ta | grep ESTAB | wc -l)`

- Get number of users<br>
`ulog=$(users | wc -w)`

- Get the IP address<br>
`ip=$(hostname -I)`<br>

- Get the MAC address<br>
`mac=$(ip link | grep "link/ether" | awk '{print $2}')`

- Get number of commands invoked by `sudo`<br>
`cmnd=$(journalctl _COMM=sudo | grep COMMAND | wc -l)`


`wall "	Architecture: $arch`<br>
	`CPU physical: $cpuf`<br>
	`vCPU: $cpuv`<br>
	`Memory Usage: $ram_use/${ram_total}MB ($ram_percent%)`<br>
	`Disk Usage: $disk_use/${disk_total} ($disk_percent%)`<br>
	`CPU load: $cpu_fin%`<br>
	`Last boot: $lb`<br>
	`LVM use: $lvmu`<br>
	`Connections TCP: $tcpc ESTABLISHED`<br>
	`User log: $ulog`<br>
	`Network: IP $ip ($mac)`<br>
	`Sudo: $cmnd cmd"`<br>

>[!IMPORTANT]
>`awk` - filters **free**'s output, checking if first word ($1) is equal to `"Mem:"`, printing only the correspondents lines.<br>
>`df` - prints a summary about disk usage. `-m` flag is to show the result in MB.<br>
>`grep` - filter output to select only lines containing `/dev/`. `-v` exclude lines containing `/boot/`<br>
>`vmstat` - reports information with details about the processes, system status, etc. The numbers define an interval in seconds.<br>
>`tail` - when used with `-1` outputs only the last line received from the previous command.<br>


>[!TIP]
>Use `crontab -u root -e` to show the content

<!----------------------- !end! SCRIPT ----------------------->
<!----------------------- BONUS ----------------------->


## ✨ BONUS

Check this before doing the bonus!

- Check how much space do you have in the directory that you want to create the VM.
- You have at least 60gb (yes!!) in the directory (sgoinfre or goinfre).
  - Why? Maybe your signature will be altered after the first evaluation, so you need to clone the project or use save state.
- Choose do ou not the bonuses **before** start the project, cause you need to create the partitions since the beggining.
- For adding points you will have to do:
  - A functional `WordPress` website with `lighttpd`, `Mariadb` and `PHP` (and study what is these things)
  - Set up a service that you think is useful (NGINX / Apache2 excluded!) Then justify your choice.


<!----------------------- !end! BONUS ----------------------->
<!----------------------- REFERENCES ----------------------->

##  🔎 References

- [sudo install](https://milq.github.io/enable-sudo-user-account-debian/)<br>
- [UFW](https://tecadmin.net/ufw-common-firewall-rules-and-commands/)<br>
- [Mariadb/MySql](https://www.simplified.guide/mysql-mariadb/user-delete)<br>
- [What's MySql?](https://www.talend.com/resources/what-is-mysql/)<br>
- [Mariadb List Users](https://linuxhint.com/mariadb-list-users/)<br>
- [php](https://www.hostinger.com.br/tutoriais/arquivo-wp-config-php)<br>

**Githubs:** <br>
- [pasqualerossi](https://github.com/pasqualerossi/Born2BeRoot-Guide?tab=readme-ov-file)<br>
- [gemartin99](https://github.com/gemartin99/Born2beroot-Tutorial?tab=readme-ov-file#5--script-)<br>
- [passunca](https://github.com/PedroZappa/42_Born2beRoot/blob/main/GUIDE.md#installing-mariadb)
<!----------------------- !end! REFERENCES ----------------------->
