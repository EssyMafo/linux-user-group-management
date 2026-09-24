# User & Group Management

## Overview

This lab focuses on Linux user and group management, group-based permissions, shared directories, and the Set Group ID (setgid) permission.

## Learning Objectives

- Create and manage Linux users
- Create and manage Linux groups
- Add users to supplementary groups
- Configure a shared directory using group-based permissions
- Understand primary and supplementary groups
- Apply the setgid bit to a directory
- Verify group ownership inheritance
- Perform positive and negative access tests

- `devs`
- `interns`

Alice and Bob are members of `devs` and should have access to the shared directory.

Carol is a member of `interns` and should not have access.


##Environment
Operating System: CentOS Stream 9
Shell: Bash
Virtualization: VMware Workstation
Lab: User & Group Management

##Skills Demonstrated
Linux user administration
Linux group administration
Supplementary groups
File and directory ownership
Linux permissions
chmod
chown
Setgid
Access control
Permission verification
Linux command-line administration

Two groups were created:

## 1. Creating the Users

The users were created with:
sudo useradd alice
sudo useradd  bob
sudo useradd  carol

Passwords were configured separately:

sudo passwd alice
sudo passwd bob
sudo passwd carol

## 2.Creating the Groups
sudo groupadd devs
sudo groupadd interns

## 3. Adding Users to Groups

Alice was added to devs:

sudo usermod -aG devs alice

Bob was added to devs:

sudo usermod -aG devs bob

Carol was added to interns:

sudo usermod -aG interns carol

The -aG option adds a user to a supplementary group without removing existing supplementary-group memberships.

### Verification
id alice
id bob
id carol

### Expected arrangement:

alice → devs
bob   → devs
carol → interns

## 4. Creating /srv/teamshare

The shared directory was created with:

sudo mkdir -p /srv/teamshare

## 5. Setting Ownership

The directory was configured with root as the owner and devs as the group:

sudo chown root:devs /srv/teamshare

Verify with:

ls -ld /srv/teamshare

## 6. Setting Permissions

The required permissions were applied using:

sudo chmod 2770 /srv/teamshare
Meaning of 2770
2    7    7    0
│    │    │    │
│    │    │    └── Others: no permissions
│    │    └─────── Group: read, write, execute
│    └──────────── Owner: read, write, execute
└───────────────── Setgid

The resulting permission pattern should be:

drwxrws---

## 7. Testing Access as Alice

Alice is a member of devs.

sudo su - alice
cd /srv/teamshare
touch alice-file.txt
ls -l
exit

Bob should be able to access the directory and create a file.

The file should also have devs as its group.

-rw-r--r-- ... alice devs ... alice-file.txt

## 9. Testing Access as Carol

Carol belongs to interns, not devs.

Start a shell as Carol:

sudo su - carol

Attempt to enter the shared directory:

cd /srv/teamshare

Expected result:

-bash: cd: /srv/teamshare: Permission denied

This confirms that users outside the devs group cannot access the directory.


##What I Learned

This lab provided practical experience with Linux user and group administration.

I practiced creating users and groups, assigning supplementary group memberships, configuring directory ownership and permissions, and using groups to control access to shared resources.

I also learned how the setgid bit works on directories and how it ensures that newly created files inherit the directory's group ownership.
