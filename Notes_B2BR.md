# IMPORTANT

### What is a Virtual Machine??

- Software-based emulation of a computer running on another computer in an isolated environment. Performs exactly the same as a normal operating system, but on a software environment.

### How does virtual machine works?

- Runs on a hypervisor =⇒ software that creates and manages Virtual Machines and it sits between the hardware and the VMs. Many different virtual machines can be created on top of it.
    - Hypervisor also helps those virtual machines to allocate enough resources by acting as a bridge between the device hardware and the virtual machine.

### Why Virtual Machines?

- Can run multiple different operating systems on one single machine
- Able to isolate the environment so that any permanent changes that were made, “rm -rf /root” something like that will not harm the rest of the operating systems in the machine. This includes virus attacks as well.
- Provide an environment where you can test programs and stuff on different operating systems under a same physical computer.

## Why Debian?

- Easier to configure installed
- Tried Rocky, got bug on partitioning with encrypted hard drives, cannot pass installation.
- Less documented for tutorials, therefore hard.

### Difference of Debian and Rocky

- Rocky is an open-source enterprise linux that was based off the RedHat CentOS. It is cross compatible with other Red Hat Enterprise Linx.
    - Enterprise —> Designed for business / academic use, and contains much advanced features.

### Differences of apitude and apt

- Aptitude is more advanced that apt,
- Aptitude is more high level package manager and apt is more low level
- Aptitude and solve package conflicts easily by analyzing the dependencies between the packages and used its dependency resolver to choose the most suitable package to upgrade.
- Both can provide the necessary means to do package management, but aptitude is more feature rich.

### What is AppArmor

- MAC (mandatory access control) security tool inside the linux kernel that can protect applications from being exploited.
- MAC —> access control policy —> grant or deny access to resource objects on a file system.
- can be said like a virtual machine environment but like for apps, so any exploits on a particular apps will be protected by the AppArmor isolated environment.

### What is that script that was running?

- That script displays the information of the server every 10 minutes later ill go through more in depth as there is a section for it later

### Check for graphics

- there is no GUI, but to further check it:
- ls /usr/bin/*session (wayland and x11 are some type of graphics environment | display server protocols)

### Check UFW and ssh

- sudo ufw status
- sudo ssh status

### Check operating system

- uname -r
- prints the operating system and the hardware platform of the linux system

### Check current user

- sudo getent group <group>
- getent means get entries

### Create user

- Sudo adduser<user>
- Password restrictions are imposed on the /etc/pam.d/common-password
- Another one is how long a user can modify its passwords:
    - /etc/login.defs

## inside /etc/pam.d/common-password

- Used to set rules for the password
- minlen —> Means the minimum length
- Below here at least is “-” and at max is “+”
- maxrepeat —> max number of characters that can be repeated
- ucredit —> Means number of uppercase letters at least/at max
- dcredit —> Means number of digits at least/at max
- lcredit —> Means number of lowercase letters at least/at max
- reject_username —> Reject inclusions of the username in the password.
- difok —> Contains at least a number of  characters that cannot be the same as the old password.
- enforce_for_root —> Implement this password policy for root.

### Create user groups

sudo adduser <user> <group>

**Then proceed to check if the user is in the group with sudo getent <group>**

### Advantages and Disadvantages Of This password policy

**ADVANTAGES**

- Very hard for user to crack or guess it
- If the user change the password every time, they cannot set the same password
- Constantly remind the users 7 days before the password expires.

**DISADVANTAGES**

- The rules are strict, which means that the password the user needs to come up with is hard and might need several retries
- User are forced to change their password multiple times.
- Plus, they only can have 3 tries, so they have to restart the reset password configuration a lot of times.

### Why is the partition size different from the example?

This is a bug in debian, where if i type lsblk, it shows the partion based on GiB, MiB, instead of GB and MB. You can use an online converter to see the difference.

The setup for debian only allows for you to create it on gigabytes and megabytes.

Another reason is that there is a bit of overhead when partitions are allocated for the partition table.

### 

### Explain the disk partioning system.

- Two types of partitions
    - Primary
        - Can only store up to 4.
        - primary partition is bootable, therefore it must be stored there.  Although linux can installed on other partitions, its only logical for
    - Extended
        - Goes beyond the four by creating one partition to store multiple different
    - Logical
        - In Linus, there is a limit of 15 logical partitions to be created. Partition that was created in extended partition.

### What is a LVM?

- Logical Volume Manager
- Method for allocating space on devices
- More flexible than conventional partitioning system
- **HOW DOES IT WORK**
    - **Takes different Physical Volumes and combines it into one volume groups**
    - Physical Group: Physical Drives
    - Then divide the partitions through different Logical Volumes.
- Since it combines different physical volumes, you can expand the partitions by just adding a new physical disks, therefore making it more flexible than other options or non-Linux.

### What is sudo?

- Sudo allows users that are in the sudo group able to run root funcionalities.
- It creates extra rules such as each sudo command being protected by a password, to provide additional layer of confirmation to the users.
- Better than using root, as you might accidentally “rm -rf /root”  by accident without any restrictions when you are root.
    - Rules are stored in /etc/sudoers.d/sudo_config
    - passwd_tries —> How long can a user try the password before termination of the command.
    - badpass_message —> shown if the user typed the wrong password.
    - logfile —> Logs down the sudo command when it was being used, as well as the timestamp and the user information.
    - log input output —> Logs down the sudo command input and output of it into the log directory file.
    - iolog_dir —> Stores the sudo logs of the input and output.
    - requiretty —> Ensures that the sudo is only ran on a live active terminal and not on a script, etc.
    - secure_path —> Make sures that sudo only ran the commands that were being listed on the path below, and not otherwise.
- **WHY THESE RULES?**
    - So that it sets an additional barrier.
    - Logs were taken so that the user can keep track on what went wrong with the sudo command or if any users were manipulating the sudo command.
    
    ### What is UFW?
    
    - Uncomplicated firewall. Basically to configure firewall by just blocking and allowing network traffic from different ports.
    - Firewall in general is to just provide an additional layer that protects and handles the network traffic.
    
    **WHAT IS A PORT?**
    
    - Port is being assigned to uniquely identify a connection endpoint and a direct data to a specific service.
    - 2 types —> TCP and UDP
        - TCP —> The protocol will keep track of segments when transmitting but will take very slow and takes more bandwidth.
        - UDP —> Faster than TCP, provides a simple request response communication, but much more unreliable than other solutions.
    
    **WHY USE UFW?**
    
    - It is because it is easy to configure by allowing and disallowing ports and also easy syntax to understand than other firewall protocols, like iptables and shorewall.

**LIST DOWN RULES**

- sudo ufw status
- sudo allow port 8080 / sudo delete allow port 8080

### What is SSH?

- cryptographic network protocol —> secure way to transfer data over through an unsecure network.
- Use SSH for —> Remote Shell login, remote pc access, file transfer and so on.

**THERE ARE TWO KEYS FOR SSH:**

- Private Key —> Kept on your device
- Public Key —> Can be shared.

- To establish an SSH connection,
    - Server first checks if the connection’s public key is in their database
    - Then, encrpts the message based on the user’s public key for the private key to decrypt it and sends it back to the server.
    - Therefore, connection is established.

### WHY USE SSH?

- Provides security and authentication.

**HOW TO CHECK SSH PORTS**

- at /etc/ssh/ssh_config

## Script description

### What is cron?

- Cron is a time-based job scheduler tool. Basically it allows the user or the root to set a command or scripts to run on a set time, for example, 10 minutes, 5 hours, or even 3 days for example.
- There are two crontab files, one in root one in user
    - Root is systemwide —> no matter which user you are logged in, the cron will do the same
        - sudo -u root crontab -e
    - User scripts will only run when you are that particular user
        - logged in to user —> crontab -e

# Bonus

### Why Chose OpenLiteSpeed?

- It is because it’s the fourth most popular web server to setup, and does not require much configurations like lighttpd to setup the file.
    - Can contain dynamic webpages, which lighttpd only have less support for static webpages, as the static webpages.
    - Have simillar access to Nginx and Apache. Lighttpd has less features
    - Lighttpd is single threaded; openlitespeed is multithreaded.