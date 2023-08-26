# File linking in Linux


![File Linking in Linux](https://github.com/Magee3/Creating-a-Backup-Directory-Using-Linux/assets/134301259/06d96312-3308-4165-b387-53245fa0fc90)

### Objective

We will establishing a backup directory for authentication logs, generating manual symbolic links, and subsequently, making an endeavor to eliminate a specific link. Creating a backup directory for log establishes a designated folder to store
copies of logs that track information, generating symbolic links specifies connections to improve organization, and we must also learn how to remove something as well as create it.

### Stage 1: Creating our Directories

First things first we will create our directory in our home file. We will create a "backup" directory and a "log" directory inside of the backup directory. Your path should look like this once you're inside the directory.

/backup/log

![Pt 1](https://github.com/Magee3/Creating-a-Backup-Directory-Using-Linux/assets/134301259/73acb0cc-df9d-4b19-86c4-646f59e45b0b)



After you move into your newly made "log" directory we will create a link. We will link /var/log/dnf.log with installation.log. This will create a hard link to the auth directory.
Run:
ln /var/log/dnf.log /backup/log/installation/log

![pt 2](https://github.com/Magee3/Creating-a-Backup-Directory-Using-Linux/assets/134301259/b901538c-bf00-4b89-992c-8fda0a5f5d85)

Now we will check the contents of both files installation.log and dnf.log. We are checking to make sure the files have the same content.
Run: 
tail /backup/log/installation.log
tail /var/log/dnf.log

![pt 3](https://github.com/Magee3/Creating-a-Backup-Directory-Using-Linux/assets/134301259/f029d998-a77d-4fa6-8f9c-8ac34a448ea0)

### Stage 2: See the changes get reflected


We will create some log entries.

Run:
dnf install -y vim

This should fail and generate a log entry. Search for the failed attempt in the logs by running

tail /var/log/dnf.log | grep -i failed
tail /backup/log/installation.log

![pt 4](https://github.com/Magee3/Creating-a-Backup-Directory-Using-Linux/assets/134301259/b7951fcc-4555-4f82-bd39-c33e1df002d2)

save the failed results into a file named "install-fail"

Run:
tail installation.log | grep -i failed >> install-fail

### Stage 3: Removing a File and Verify The Other is Still Intact


simply remove the installation.log file

Run:
rm /backup/log/installation.log

![pt 6](https://github.com/Magee3/Creating-a-Backup-Directory-Using-Linux/assets/134301259/43725a7b-9f38-4617-bbef-1655502163c8)

verify that it has been removed.
Read the failed installation once again to verify the file has not been tampered with.

Run:
tail /var/log/dnf.log | grep -i failed

![pt 4](https://github.com/Magee3/Creating-a-Backup-Directory-Using-Linux/assets/134301259/49f5a071-ffdc-4a10-8305-8d738338b051)


### Stage 3: Creating a symbolic link


We will change our directory to the root users directory.
Once in your root directory we will create a hard link from /proc/meminfo to memory_info.txt in your home directory.

Run: ln /proc/meminfo memory_info.txt

You will get an error because hard links accorss different file systems are not allowed. You must use a symbolic link.

Run: ln -s /proc/meminfo memory_info.txt

![pt 7](https://github.com/Magee3/Creating-a-Backup-Directory-Using-Linux/assets/134301259/07527007-f0c1-46f9-8ba1-dd33fd82df5c)

### Stage 4: Remove an original file through link


Create a file with something inside in the root directory. I used "linktest"

![pt 8](https://github.com/Magee3/Creating-a-Backup-Directory-Using-Linux/assets/134301259/59454bd2-dff2-4d9e-9f40-c54942bbb975)

Move into the "opt" directory
We will now link our "linktest" file with a newly made file we will call "linked"
Run: 
ln -s /linktest linked

Then Run: 
ls -l

![pt 9](https://github.com/Magee3/Creating-a-Backup-Directory-Using-Linux/assets/134301259/0b850510-0b49-4651-a00f-101231ddfeb2)

Now removing the original file will break the link but keep the file in the /opt directory. In order to delete both files you must delete both. Hardlinks and symbolic links are like shortcuts.
Hardlinks all point towards the data block on the file and a symbolic link points to a files path or location.



