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
