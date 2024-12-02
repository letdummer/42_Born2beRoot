<!-- INTRODUCTION-->

## INTRODUCTION

+ **OBJECTIVE**

  Create and configure a Virtual Machine.<br>
<details>
<summary> 

  #### REQUIREMENTS
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
<!-- !end! INTRODUCTION--> 

<!-- SUMMARY OF STEPS -->
<details open>
<summary>
  
  ###  STEP BY STEP
</summary>

- [Downloading the Virtual Machine](#downloading-the-virtual-machine)<br>
- [Installing Debian](#installing-debian)<br>
- [Partition Disks](#partition-disks)<br>
- [Setting up the Virtual Machine](#setting-up-the-virtual-machine)<br>
- [Sudo, Users and Groups](#sudo-users-and-groups)<br>
- [Installing SSH and VIM](#installing-ssh-and-vim)<br>


</details>


<!-- !end! SUMMARY OF STEPS -->

<!-- INSTALLING THE VIRTUAL MACHINE -->

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
   
<!-- !end! INSTALLING THE VIRTUAL MACHINE -->

<!-- INSTALLING DEBIAN -->

## INSTALLING DEBIAN

- Language `English`
- Selection your location. To choose Portugal your need to select `other` > `Europe` > `Portugal`
- Configure the keyboard to `United States en_US.UTF-8` > `American English`
- Hostname for the network: `youruser42`
- In `Domain name` you can just type `enter` to scape this option.
- Choose a `root password` and take note!
- Create an account with username and password.
- Configure the clock to Lisbon.

## Partition disks
  
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

<!-- !end! INSTALLING DEBIAN -->

<!-- SETTING UP THE VIRTUAL MACHINE -->

## Setting up the Virtual Machine

- Enter your encryption password.
- Enter the user and password.

### Sudo, Users and Groups

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

### Installing SSH and VIM

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
- Open sshd_config
```
vim /etc/ssh/sshd/config
```







<!--  !end! SETTING UP THE VIRTUAL MACHINE -->



<!-- REFERENCES -->
## References 

- [sudo install](https://milq.github.io/enable-sudo-user-account-debian/)<br>
- [Mariadb/MySql](https://www.simplified.guide/mysql-mariadb/user-delete)<br>
- [What's MySql?](https://www.talend.com/resources/what-is-mysql/)<br>
- [Mariadb List Users](https://linuxhint.com/mariadb-list-users/)<br>
- [php](https://www.hostinger.com.br/tutoriais/arquivo-wp-config-php)<br>

**Githubs:** <br>
- [pasqualerossi](https://github.com/pasqualerossi/Born2BeRoot-Guide?tab=readme-ov-file)<br>
- [gemartin99](https://github.com/gemartin99/Born2beroot-Tutorial?tab=readme-ov-file#5--script-)<br>
- [passunca](https://github.com/PedroZappa/42_Born2beRoot/blob/main/GUIDE.md#installing-mariadb)
<!-- !end! REFERENCES -->
