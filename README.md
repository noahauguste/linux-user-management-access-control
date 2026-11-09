# Linux User Management & Access Control Lab

**Project 01 | TechOps Home Lab**

## Overview

Built a Linux administration lab to practice secure remote access, user account management, group-based file permissions, and the principle of least privilege. Configured a standard user to read a protected file without being able to modify it, then verified both authorized and denied actions from that user's session.

## Lab environment

- **Host:** Apple Silicon Mac (8 GB RAM)
- **Virtualization:** UTM
- **Guest OS:** Ubuntu Server (ARM64)
- **Remote access:** SSH from macOS Terminal
- **Tools and commands:** `ssh`, `adduser`, `id`, `su`, `sudo`, `groupadd`, `usermod`, `chgrp`, `chmod`, `ls`, `cat`, `touch`

## Objectives

1. Create a standard Linux account without administrative privileges.
2. Configure a restricted directory and a read-only file.
3. Assign authorized access through a Linux group.
4. Verify that reading succeeds while unauthorized modification is denied.

## Configuration

1. Connected to the Ubuntu VM over SSH from macOS Terminal.
2. Created a standard user named `labtech` with `sudo adduser labtech`.
3. Verified that `labtech` did not have permission to run `sudo`.
4. Created the group `techops-readers` and added `labtech` using `sudo usermod -aG techops-readers labtech`.
5. Configured `/srv/techops-private` with owner `root`, group `techops-readers`, and permissions `750` (`rwxr-x---`).
6. Created `/srv/techops-private/notes.txt` with owner `root`, group `techops-readers`, and permissions `640` (`rw-r-----`).
7. Switched to the `labtech` account and tested access to the directory and file.

## Validation results

| Test | Expected result | Observed result |
| --- | --- | --- |
| User identity and membership (`id labtech`) | User belongs to `techops-readers` | Passed |
| Sudo privilege check (`sudo -l` as `labtech`) | Administrative access denied | Passed |
| Directory listing (`ls -la /srv/techops-private`) | Authorized group can list directory | Passed |
| File read (`cat /srv/techops-private/notes.txt`) | File contents displayed | Passed |
| File modification (`echo "Unauthorized edit" >> /srv/techops-private/notes.txt`) | Permission denied | Passed |

## Evidence

### 1. User and group membership
![User and group membership](screenshots/01_User_Group_Membership.png)

### 2. Directory and file permissions
![Directory and file permissions](screenshots/02_File_Permissions.png)

### 3. Read-only access verification
![Successful read and blocked file modification](screenshots/03_Access_Control_Test.png)

## Key takeaways

- Authentication establishes *who* a user is; authorization controls *what* they can access.
- Linux owner, group, and other permission bits can enforce read-only access without granting administrator privileges.
- An administrator's normal shell does not automatically run commands with root privileges; `sudo` elevates an individual command.
- Testing both allowed and denied actions is important for confirming that a security control works as intended.

> **Scope:** This is a personal training lab using sample data, not a production deployment. Group permissions demonstrate Linux discretionary access controls and least privilege; they do not by themselves constitute a full enterprise role-based access control system.
