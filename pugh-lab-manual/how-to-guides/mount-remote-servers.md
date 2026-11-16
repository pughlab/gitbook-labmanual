---
description: Steps on how to mount a remote server
---

# Mount remote servers

## Mounting remote servers to your local environment

To enable direct access files on a remote server from your local computer we use macfusion. The following steps can be used to mount the mordor filesystem through samwise which can be applied to the other servers you may have access to (ex. H4H, OICR, SickKids):

### SSHFS

Download and install FUSE AND SSHFS from [https://osxfuse.github.io/](https://osxfuse.github.io/)

To mount any directory on mordor:

```
General:
sshfs -o defer_permissions username@ip.OR.hostname:/remote/dir/to/mount/ /local/dir/to/mount/to

Example:
sshfs -o defer_permissions jsmith@205.210.128.25:/mnt/work1/users/pughlab/ ~/Desktop/Samwise/
```

### Additional notes on mounting various servers

* For remote access, use [GlobalProtect](https://www.uhnresearch.ca/news/working-remotely-vpn)

To ensure seamless flow-through from your local device to various servers, you should set up rsa key pairs for every pass through. Below are the steps to do so:

1\) Create a RSA key on your local computer

```
ssh-keygen -t rsa
```

```
Enter file in which to save the key (/Users/jsmith/.ssh/id_rsa): Press “Enter”
Enter passphrase (empty for no passphrase): Press “Enter”
Enter same passphrase again: Press “Enter”
Your identification has been saved in /Users/jsmith/.ssh/id_rsa.
Your public key has been saved in /Users/jsmith/.ssh/id_rsa.pub.
The key fingerprint is:
XXXXXXXX jsmith@tpugh30.local
The key's randomart image is:
+--[ RSA xxx]--+
XXXXXXXXXXX
XXXXXXXXXXX
XXXXXXXXXXX
XXXXXXXXXXX
XXXXXXXXXXX
```

2\) Copy the RSA key into H4H data transfer node (h4huhndata1)

```
ssh-copy-id -i ~/.ssh/id_rsa jsmith@172.27.23.173
jsmith@172.27.23.173's password: <your password>
```

3\) Login to H4H to confirm RSA key has been properly set up

```
ssh jsmith@172.27.23.173
```

\
**To use the same key to copy data between H4H and Valinor:**\
\
4\) Copy the RSA key into Valinor

```
ssh-copy-id -i ~/.ssh/id_rsa jsmith@valinordata1
jsmith@valinordata1's password: <your password>
```

5\) Log into Valinor to make sure that only the key(s) you wanted were added

```
ssh jsmith@valinor.uhnresearch.ca
```

6\) Exit H4H and login to Valinor, and continue connecting if warning

```
ssh jsmith@valinor.uhnresearch.ca
Are you sure you want to continue connecting (yes/no)? yes
exit
```

**Setting up ProxyCommands if required:**

* For various network/VPN setups proxies may be necessary. ProxyCommand can be used to send commands through one host to another. These can be hard coded in your config file (\~/.ssh/config) and strung together to achieve the desired results. Below is a sample config file with ProxyCommands to aid with access to various servers:

```
Host h4huhndata1
   HostName 172.27.23.173    
   User jsmith

Host valinordata1
   User jsmith
   ProxyCommand ssh -W %h:%p jsmith@valinor.uhnresearch.ca

```

### **Setting up directories to mirror a remote structure**

{% hint style="info" %}
It can be useful to have the remote directory structures mirrored on your local computer for ease of viewing/transferring/accessing files once these servers are mounted.
{% endhint %}

{% hint style="warning" %}
**NOTE: Practice extreme caution whenever you use “sudo”. This gives you permission to do almost anything on your local computer, including overwriting or deleting important files. If in doubt about the following steps, please reach out to Jeff, Jeffrey.Bruce@uhn.ca.**
{% endhint %}

1\) Create a mirror for the H4H directory structure on your local

```
sudo mkdir -p /cluster/projects/
Password: <your password>
```

```
sudo chown jsmith /cluster/projects/
```

\* jsmith represents your local username.

```
/cluster/projects/ will be your ‘Path’ and ‘Mount Point’ on macfusion for mounting H4H
```

2\) Create a mirror for the Valinor directory structure on your local

```
sudo mkdir -p /mnt/work1/users/pughlab/
Password: <your password>
```

```
sudo chown jsmith /mnt/work1/users/pughlab/
```

```
/mnt/work1/users/pughlab/ will be your ‘Path’ and ‘Mount Point’ on macfusion for mounting mordor
```

**Starting with Mac OS 10.15 (Catalina)**\* you can no longer create folders in the root directory (even with sudo) so you will need to create a folder in a writable location on your computer and then a symlink using the instructions found [HERE](https://derflounder.wordpress.com/2020/01/18/creating-root-level-directories-and-symbolic-links-on-macos-catalina/)<br>

**Example /etc/synthetic.conf**

```
# create a symbolic link named "cluster" at / which points to ~/Desktop/cluster
cluster	/Users/jbruce/Desktop/cluster
# create a symbolic link named "mnt" at / which points to ~/Desktop/mnt
mnt	/Users/jbruce/Desktop/mnt 
```

**Some reasons your mount may fail**

* Folder you're mounting over does not exist
* Permissions of the folder you're mounting over (your user must have read & write permissions)
* Old version of OSXFUSE (download and install the latest version)
* Sym links have not been created from synthetic.conf yet (need to restart your mac for changes to take affect)

**Some useful IP addresses:**

* H4H login node IP: 172.27.23.163
* H4H data transfer node (h4huhndata1) ip: 172.27.23.173 ← use this for mounting the H4H filesystem
* mordor head node IP: 192.168.198.99
* mordor Slurm login node IP: -p 10022 username@192.168.198.99
* samwise ip: 205.210.128.25
* hamfast ip: 192.168.2.37

## Macfusion (DEPRECATED)

1. Download and install OSXFUSE from [https://osxfuse.github.io/](https://osxfuse.github.io/)
2. Download and install MacFusion from [http://macfusionapp.org/](http://macfusionapp.org/)
3. For MacOS 10.12 (Sierra) to High Sierra: [Version 2.1-dev](https://github.com/macfusion-ng/macfusion2/releases/tag/2.1-dev)
4. For MacOS 10.14.6 and above: [https://github.com/macfusion-ng/macfusion2/releases/tag/2.1.1-dev.3](https://github.com/macfusion-ng/macfusion2/releases/tag/2.1.1-dev.3)
5. Open a terminal and run the “id” command. Make note of your “uid” and “gid” for use in step 4.
6. In MacFusion, add a new SSHFS server and configure as follows:

```
   SSH tab:
        Host: Samwise or 205.210.128.25
        User Name: <your username>
        Password: <your password>
        Path: /mnt/work1/users/pughlab
   SSH Advanced tab:
        Port: 22
        [Uncheck] Enable Compression
        [Check] Follow Symbolic Links
        Extra Options: -o idmap=user -o uid=<uid> -o gid=<gid>
        ** Example: -o idmap=user -o uid=502 -o gid=20
   Macfusion tab:
        Mount Point: /Users/username/Desktop/mount_directory.
        ** See below for a note on mirroring a remote directory on your local computer.
```

Note: If configuration above does not work, try changing gid in Extra options to: ’gid=20(staff)’.



## Troubleshooting MacFUSE after MacOS or MacFUSE update&#x20;

* After MacOS update, your MacFUSE may also need to be updated.&#x20;
* After updating MacFuse, errors may arise when trying to access the the cluster as in:
  * command:&#x20;
    * `sshfs -o defer_permissions username@ip.OR.hostname:/remote/dir/to/mount/ /local/dir/to/mount/to`
  * error message:
    * `mount_macfuse: the file system is not available (1)`
* To resolve the error, you need to load the macFUSE kernel extension manually.
  *   To do this, in terminal write: &#x20;

      ```
      sudo /usr/bin/kmutil load -p /Library/Filesystems/macfuse.fs/Contents/Extensions/14/macfuse.kext
      ```
* Then in In privacy & security section of MacOS system settings. Go to "Allow" for Benjamin Fleischer software. You may be prompted to enter password and restart your computer. After that, you should be able to sshfs as before.&#x20;
* For additional information, check out: [https://github.com/osxfuse/osxfuse/issues/992#issuecomment-1873528612](https://github.com/osxfuse/osxfuse/issues/992#issuecomment-1873528612)

