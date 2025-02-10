# Assignment 1
# Account Setup
- In  the first step, I'd created Microsoft Azure account using email address that university provoded me.

- Then I got  to know about the cloud and its importance. 

- Additionally, I explored student benefits and open an student account and Azure give me *$100* worth of credits that I can use to purchase any package within the environment.


# Create Virtual Machine
- I created virtual machine where I have to work later on this course.

- Then I select Marketplace from *Ubuntu Server 24.04 LTS gen 2 Server* published by Canonical. 

- I named my machine *"rubi-robotics"*.

- I choose B series version 2 which is *Standard_B2ls_v2*.

- Later, I create a new resource group for the machine and subnet to place the machine in.

# Link my HAMK email to my GitHub account

- I create a new repository named *"linux"* for the assignment.

- Then I add my HAMK email as a secondary mail for version control.
![screenshot 1](image/Screenshot_22-1-2025_15321_github.com.jpeg)


# Assignment 3
This document details the process of creating users and managing permissions in a Linux environment. The tasks include creating different types of users, managing sudo privileges, and setting up a shared directory with specific permissions.

Task 1 & 2: Creating User Tupu and creating user Lupu
The adduser script was used to create the Tupu user. This script provides an interactive way to create a new user with all necessary directories and settings.
![Screenshot 2025-01-29 212757](https://github.com/user-attachments/assets/857340d1-313a-4d58-a7fa-85a98252bfc0)

The adduser script automatically creates the home directory
Sets up default shell
Prompts for password
Creates user-specific group
Sets up basic profile.
For task 2 initially, I tried using the following given command 

```sudo useradd -m -d /home/lupu -s /bin/bash -G lupu```
- However, this resulted is an error.

The issue was that we tried to add the user to a group that didn't exist yet. To fix this, we modified the command to include the -U flag, which automatically creates a user group with the same name as the user:

```sudo useradd -m -d /home/lupu -s /bin/bash -U lupu```
- Creating User Lupu

After creation, Set the password.

Task 3: Creating System User Hupu
Created a system user Hupu with restricted login capabilities:

```sudo useradd --system --shell /bin/false hupu```
- Creating System User Hupu

This creates a system account that:

Has no login shell (/bin/false)
Is typically used for running services
Has no home directory by default

![Screenshot 2025-01-29 212757](https://github.com/user-attachments/assets/1ee9dc4d-6211-45d8-9f1e-373a0e9d194c)

Task 4: Adding Sudo Privileges
Added sudo privileges for both Tupu and Lupu users using the usermod command:

```sudo visodu```

- Then adding:

```
tupu ALL=(ALL:ALL) ALL
   lupu ALL=(ALL:ALL) ALL
```

- Adding Sudo Privileges

![Screenshot 2025-01-29 212550](https://github.com/user-attachments/assets/b1dcc495-14b0-4227-be5e-9af66fa011ad)

![Screenshot 2025-01-29 215916](https://github.com/user-attachments/assets/3a9bbd7d-bba0-43a7-be08-f13d6246b73e)

Alternative method using:

Adding Sudo Privileges

![Screenshot 2025-01-29 213703](https://github.com/user-attachments/assets/ebbc6394-3c63-436c-9134-b030dc8a8477)


Task 5: Setting Up Shared Directory
This task required creating a shared directory with specific permissions for Tupu and Lupu. Here's the detailed solution:



![Screenshot 2025-01-29 215405](https://github.com/user-attachments/assets/4bfa99f5-99f6-420e-8c5b-8fc1e2b600d6)

The permission setup (2770) breaks down as:

- 2: SetGID bit (ensures new files inherit group ownership)
- 7: Owner has full permissions (read/write/execute)
- 7: Group has full permissions (read/write/execute)
- 0: Others have no permissions

  This configuration ensures :

- Only Tupu and Lupu can access the directory
- New files automatically inherit the projekti group
- Other users cannot access the directory
- Both users have full control over the directory and its contents


   
## Assingment 5 (Scripting)

In this assignment, we created a shell script called print.sh to add a line to the file diskspace.txt, reporting the home directory size and the current date and time. We then used crontab to schedule this script to run every 12 hours, ensuring it runs at least six times to populate diskspace.txt with multiple entries. Finally, we utilized an awk command to find the line with the maximum value in the first column and printed it in the format: Max=[maximum value], at [date and time]. This task helped us automate the process of monitoring disk space and identifying the largest recorded value efficiently.

### Step 1: Make a script and add it to cron

**Script: `print.sh`**

This script adds a line to the file `diskspace.txt`, reporting the home directory size and the current date.

``` bash
#!/bin/bash

output=$(ls -l / | grep home | awk '{print $5}')
current_datetime=$(date)
echo "$output $current_datetime" >> /home/azurerubi/discspace.txt
```

![Screenshot 2025-02-06 203348](https://github.com/user-attachments/assets/4631c04f-39a0-4f27-b91a-03a2c0abbdff)

**Add to Cron**

Use the following command to open your crontab file:

``` bash
crontab -e
```

Add the following line to run the print.sh script every 12 hours:

``` bash
0 */12 * * * /home/azurerubi/print.sh
```

![Screenshot 2025-02-06 204014](https://github.com/user-attachments/assets/60a5164e-7252-44ac-897a-9c031dc532d7)


### Step 2: Run the script a minimum of 6 times

After adding the script to cron, it will automatically run every 12 hours, adding a line to diskspace.txt each time. Ensure that it runs at least 6 times so that the file contains at least 6 lines.


![Screenshot 2025-02-06 211030](https://github.com/user-attachments/assets/2d89bca3-5cdd-4976-bd62-0f1a8b2ac134)

### Step 3: Find and print the line containing the maximum size

Use the following awk command to find the line with the maximum size and print it in the specified format:

``` bash
awk 'NR == 1 || $1 > max { max = $1; max_line = $0 } END { print "Max=" max ", at " substr(max_line, index(max_line, $2)) }' /home/azurerubi/discspace.txt
```

![Screenshot 2025-02-06 211216](https://github.com/user-attachments/assets/49c510cf-7c7c-4621-af0b-9ee716d490d8)


### Explanation

#### Script (print.sh):

- output=$(ls -l / | grep home | awk '{print $5}'): Get the size of the home directory.
- awk '{print $5}': Extract the size part.
- current_datetime=$(date): Get the current date and time and stored in the variable current_datetime.
- echo "$output $current_datetime" >> /home/azurerubi/discspace.txt: Append the output to diskspace.txt.

#### Cron:

- 0 */12 * * *: Run the script every 12 hours. 


#### awk command:

- NR == 1 || $1 > max { max = $1; max_line = $0 }: Find the maximum value in the first column.
- END { print "Max=" max ", at " substr(max_line, index(max_line, $2)) }: Print the maximum value and the corresponding row in the desired format.



# Assingment 6


# Part 1: Understanding APT & System Updates

### 1.APT Version Check

Command executed:
``` bash
apt --version
```

Output:

![Screenshot 2025-02-10 203326](https://github.com/user-attachments/assets/9e3a0cc7-dbb3-4fef-b4a9-81644f8d2da2)



### 2. Package List Update

Command executed:
``` bash
sudo apt update
```
Output:


![Screenshot 2025-02-10 203523](https://github.com/user-attachments/assets/46fc599d-0b5d-49c0-89f4-105766e5e1cf)


 **This step is important because:**

- Ensures the system has the latest information about available packages
- Helps identify which packages need updates
- It synchronizes the local package index with the remote repositories
- Required before performing any package installations or upgrades


### 3. Upgrade installed packages

Command executed:
``` bash
sudo apt upgrade -y
```
Output:


![Screenshot 2025-02-10 203900](https://github.com/user-attachments/assets/7563d8d5-69f8-4592-ad93-d872468200b5)

![Screenshot 2025-02-10 204232](https://github.com/user-attachments/assets/cd4963ae-78b8-430c-b237-1935c25095de)

![Screenshot 2025-02-10 204247](https://github.com/user-attachments/assets/7b1c4c4f-3cae-4a3f-b629-e6825374b177)


**Difference between update and upgrade:**

- apt update only refreshes the package index and metadata
- apt upgrade actually downloads and installs newer versions of installed packages

### 4. Pending Updates Check

Command executed:
``` bash
apt list --upgradable
```

Output:

![Screenshot 2025-02-10 204541](https://github.com/user-attachments/assets/da1cbeb0-eb87-4408-9f96-e887a8eeb87f)


# Part 2: Installing & Managing Packages

### 5. Search for a package using APT

Command executed:
``` bash
apt search image editor
```

*Selected package: koko ( image gallery for Plasma mobile )*

### 6. View Package Details

Command executed:
``` bash
apt show koko
```

Output:


![Screenshot 2025-02-10 205508](https://github.com/user-attachments/assets/f6f12d58-7f8d-4b5b-9bea-9f66100e4a6b)


**Dependencies:**

- koko-data (= 23.08.5+ds.1-1ubuntu2), libkf5purpose5, qml-module-org-kde-kquickcontrolsaddons, qml-module-org-kde-kquickimageeditor, qml-module-org-kde-qqc2breezestyle | qml-module-org-kde-qqc2desktopstyle, kio, libc6 (>= 2.34), libgcc-s1 (>= 3.3.1), libkf5configcore5 (>= 4.98.0), libkf5configgui5 (>= 5.74.0), libkf5coreaddons5 (>= 5.53.1), libkf5dbusaddons5 (>= 4.99.0), libkf5guiaddons-bin, libkf5guiaddons5 (>= 4.96.0), libkf5i18n5 (>= 5.17.0), libkf5kiocore5 (>= 5.92.0), libkf5kiowidgets5 (>= 5.59.0), libkf5notifications5 (>= 4.96.0), libkf5windowsystem5 (>= 5.91.0), libkokocommon0.0.1 (>= 23.08.3), libqt5core5t64 (>= 5.15.1), libqt5dbus5t64 (>= 5.0.2), libqt5gui5t64 (>= 5.14.1) | libqt5gui5-gles (>= 5.14.1), libqt5positioning5 (>= 5.6.0), libqt5qml5 (>= 5.14.1), libqt5quick5 (>= 5.4.0) | libqt5quick5-gles (>= 5.4.0), libqt5sql5t64 (>= 5.0.2), libqt5svg5 (>= 5.15.1), libqt5widgets5t64 (>= 5.0.2), libqt5x11extras5 (>= 5.6.0), libstdc++6 (>= 13.1), libxcb1

### 7. Package Installation

Command executed:
``` bash
sudo apt install koko -y
```

*Installation was successful, verified by launching the application.*

### 8. Version Check

Command executed:
``` bash
apt list --installed | grep koko
```

Output:

![Screenshot 2025-02-10 210313](https://github.com/user-attachments/assets/7b20f73b-e06c-4bd1-ba35-063f3c73b592)



# Part 3: Removing & Cleaning Packages

### 9. Uninstall the package

Command executed:
``` bash
sudo apt remove koko -y
```

*Note: This removes the package but keeps configuration files.*

### 10. Remove Configuration files

Command executed:
``` bash
sudo apt purge koko -y
```

**Difference between remove and purge:**

- remove only uninstalls the package binaries
- purge removes both the binaries and configuration files

### 11. Clear Unnecessary Dependencies

Command executed:
``` bash
sudo apt autoremove -y
```

**This step is important because:**

- Removes packages that were installed as dependencies but are no longer needed
- Keeps the system clean from unused software
- Free disk space


### 12. Clean Up Downloaded Package Files

Command executed:
``` bash
sudo apt clean
```

**This command:**

- Removes all downloaded package files (.deb) from the local cache
- Frees up disk space in /var/cache/apt/archives/
- Doesn't affect installed packages

# Part 4: Managing Repositories & Troubleshooting

### 13. Repository List

Command executed:
``` bash
cat /etc/apt/sources.list
```

Output:

![Screenshot 2025-02-10 210959](https://github.com/user-attachments/assets/8aeb5418-9895-4602-8f5d-edf56415ac02)


**Observations:**

- Contains main Ubuntu repositories
- Includes different components (main, restricted, universe, multiverse)
- Lists both source and binary package repositories
- Contains security updates repositories

### 14. To Add Universe Repository

Command executed:
``` bash
sudo add-apt-repository universe
sudo apt update
```

Output:

![Screenshot 2025-02-10 211330](https://github.com/user-attachments/assets/632a5d37-7c6d-404d-87d2-7ee8d882ddb2)


**Universe repository contains:**

- Community-maintained software
- Wide range of applications
- Less official support
  

### 15. Installation Failure Simulation

Command executed:
``` bash
sudo apt install fakepackage
```

Error message:

![Screenshot 2025-02-10 211520](https://github.com/user-attachments/assets/6cfa9dac-e415-4453-a8e0-f01ab32f909c)


**Troubleshooting steps:**

1. Check package name 
2. Check repositories
3. Update package lists
4. Search for package
5. Fix broken package

### _Bonus Challenge: Package Hold Management_

Commands executed:
``` bash
sudo apt-mark hold firefox
sudo apt-mark unhold firefox
```
![Screenshot 2025-02-10 211730](https://github.com/user-attachments/assets/25a3ceb3-4010-4246-8cd9-c01040abb691)


**Reasons to hold a package:**

- To prevent unwanted updates
- To maintain a stable version of a package that is known to work well with your system.
- To custom configurations
- To test new versions of other packages without affecting the held package.
