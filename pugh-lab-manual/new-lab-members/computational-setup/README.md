---
description: Here is a to do list for new members to get set up in Pughlab
---

# Computational Setup

## The lab systems

Get acquainted with the Pugh Lab resources. These include:

* [GIT repository](https://github.com/pughlab)
* [Samwise](./#samwise)
* [Hamfast](./#hamfast)
* [H4H](./#hpc4health-h4h)
* [Valinor](./#valinor)

## Set up access to servers

From a terminal, run:&#x20;

```
ssh -p 10022 uid@172.27.23.163 # to log into H4H slurm cluster
ssh <username>@valinor.uhnresearch.ca # to log into Valinor
```

## Valinor

{% hint style="info" %}
For a detailed overview of the Valinor cluster environment, please refer to: [cluster guide](https://1drv.ms/w/s!AinqViGDqMQDjEcMe7a47xLW2lpb?e=vU9Xcd)
{% endhint %}

### Aliasing IP

All laptops in the Pugh Lab are tethered to the internal network and therefore, an internal IP is required to access Mordor. In the simplest case, Mordor can be accessed through ssh using the IP provided by Carl/Zhibin:

```
ssh userid@192.168.198.99 <- Mordor
```

However, the static IP is long, easily forgettable, and makes scripts hard to maintain if the IP ever changes. Therefore, it makes more sense to add `Valinor` to your hosts file as follows:

1. On your local system, at the command line, open your hosts mapping file using the Nano text editor:

```
nano -w /private/etc/hosts
```

2. Add the Valinor IP alias to the bottom of this file, so that it looks like this:

```
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1       localhost
255.255.255.255 broadcasthost
::1             localhost
fe80::1%lo0     localhost
192.168.198.99  mordor
```

3. Use Ctrl-X to exit and save the file when prompted.

### Setting up your SSH Key

When you attempt to connect to `Valinor`, you will be prompted to enter a password. This can be tedious, particularly if you have a strong, long password and access the system multiple times a day. To avoid having to enter your password from your secure, tethered laptop, you can generate a local ssh key and save it to your account on `Valinor`.

1. On your local laptop, run the key-generation command in a Terminal window. When prompted for file and passphrases, just hit return to accept defaults.

```
     ssh-keygen -t rsa
     Generating public/private rsa key pair.
     Enter file in which to save the key (~/.ssh/id_rsa): (hit return)
     Enter passphrase (empty for no passphrase): (hit return)
     Enter same passphrase again: (hit return)
     Your identification has been saved in ~/.ssh/id_rsa
     Your public key has been saved in ~/.ssh/id_rsa.pub
     The key fingerprint is:
     A long string of random-looking characters
     The key’s randomart image is:
     A 2D matrix of random-looking characters
```

2. This should make steps 3 to 6 unnecessary:

```
ssh-copy-id -i ~/.ssh/id_rsa user@192.168.198.99
```

3. Copy the key is stored in `~/.ssh/id_rsa.pub` by running the following command and copying the long string of random-looking characters using Ctrl-V.

```
cat ~/.ssh/id_rsa.pub
```

4. Log on to `Valinor` as usual and open the ssh keys file using the Nano text editor:

```
nano -w ~/.ssh/authorized_keys
```

5. Paste the long string of random-looking characters as a new line in this file using Ctrl-V.
6. Use Ctrl-X to exit and save the file when prompted.

### Setting up an SSH Alias

Now that the host file is setup, you can define an alias to quickly access Mordor without having to type the extended ssh command. A –X flag is needed to enable an X-windows session, in case a program with a graphical user interface is used on Mordor. To define this alias:&#x20;

1. Open your .profile file (run automatically every time you start a new terminal) using the Nano text editor:

```
nano -w ~/.profile
```

2. Add the following line to this file:

```
alias val='ssh @valinor.uhnresearch.ca
```

3. Use Ctrl-X to exit and save the file when prompted.

### Issues with SSH Keys - Using Config File (Instructions by Stuart Watt)

If you have problems with ssh forwarding keys between systems, you might find it easier to use a config file. For example, you might have a key for both Github and Mordor, and without a config file, ssh will actually try to use your Github key to sign in on Mordor. A config file for ssh allows you to specify which key is used for which host. It also makes it easier to do two-hop forwarding, which is often an issue with ssh key forwarding.

Here’s my config file (they live in `.ssh/config` – and don’t worry, nothing here is confidential, but your user name will obviously be different, so remember to change the `stuartw` to whatever you need)

```
Host github.com
 IdentityFile ~/.ssh/github_rsa

Host mordor
  User stuartw
  IdentityFile ~/.ssh/id_rsa

Host samwise
  ProxyCommand ssh -i ~/.ssh/id_rsa -W %h:%p stuartw@mordor
  IdentityFile ~/.ssh/id_rsa

Host cbioportal
  HostName samwise
  User cbioportal
  ProxyCommand ssh -i ~/.ssh/id_rsa -W %h:%p stuartw@mordor
  IdentityFile ~/.ssh/id_rsa
```

Let’s look at this in some detail. First, I have a Github key that’s not called the usual `id_rsa`, so I used the `IdentityFile` setting to specify that is the key to use with `github.com`. That way, my Github key doesn’t interfere with anything else – it’s only used for `github.com`.&#x20;

Next up is Mordor, another easy one. Again I specify which key to use, in this case `id_rsa`, in line with the earlier notes. I also specify which username to sign in through. I can then simply use a command:

```
ssh mordor
```

to connect, so that’s all I need in my `alias` command in bash.&#x20;

Samwise is more interesting. We want to connect through Mordor, so I use the `ProxyCommand` setting to specify how. This is an `ssh` command that goes through Mordor, before we get to Samwise. Interestingly, there are two key files used: one for `samwise` and one for the `ProxyCommand` (which is for `mordor`). In principle, these could be different keys, even though here they aren’t. Explicitly specifying both means there’s much less risk of either one being wrong. To connect, it’s just as easy:

```
ssh samwise
```

That’s all you need, so you can alias it (or not) as you choose.&#x20;

And finally, `cbioportal` – this is actually an alias. There is no host `cbioportal`, but I can still use `ssh cbioportal`. The explicit `HostName` says to use `samwise` anyway, with the same `ProxyCommand` – the difference is that we specify a different username. So in effect, `ssh cbioportal` is the same as `ssh samwise` except that we connect as a different user: in this case one that still prompts for a password (unless we have a key for it). To use, it’s just the same:

```
ssh cbioportal
```

### Accessing the cluster using a non UHN computer

In order to access the cluster from a non-uhn computer or over a non-uhn network (without the VPN) you will first need to obtain a list of one time passwords from RIS. Using your UHN username and one of these passwords you can log in here: [http://www.uhnresearch.ca/remote](http://www.uhnresearch.ca/remote). Once the connection is successful, you can ssh into mordor using the following command:

```
ssh -p 5059 username@192.75.165.28
```

For more information on ssh config files, see: [http://linux.die.net/man/5/ssh\_config](http://linux.die.net/man/5/ssh_config)

## Samwise

**Samwise** is a private server on mordor dedicated to Pughlab. It can be used for interactive analysis works, data visualization (e.g. IGV) , database hosting, and the kind of work which is not permitted on Mordor head node. Samwise is accessible through Mordor by SSH and it is also accessible in a single shot by doing:

```
ssh -X -A -t userid@mordor ssh -X -A -t userid@samwise
or
ssh -Y -A -t userid@205.210.128.25
```

`-A` enables agent forwarding `-t` allows you to run interactive commands on Samwise through Mordor

And you can alias the above command to a shorter command for quick access.

## Hamfast

### What is Hamfast? <a href="#what_is_hamfast" id="what_is_hamfast"></a>

Samwise is our interactive high-memory node that exists on the genomics center cluster running on an CentOS 6 operating system. It's original 130Gb of memory was split into 2 partitions:

* Samwise 64Gb memory - CentOS 6
* Hamfast 64Gb memory - CentOS 7

### From internal network <a href="#from_internal_network" id="from_internal_network"></a>

Log on to **samwise** or **mordor** and use the following command:

```
ssh hamfast
```

### From client computer <a href="#from_client_computer" id="from_client_computer"></a>

Use the **ProxyCommand** approach to tunnel directly to hamfast through samwise.

1. Modify your the `~/.ssh/config` file on your local computer

```
Host hamfast
User [USERNAME]
ProxyCommand ssh -Y -A -t -W %h:%p [USERNAME]@205.210.128.25
```

2. To enable your ssh-key to work on hamfast from your local, make sure to clone your ssh-key from mordor/samwise to your local

```
ssh-add <(ssh [USERNAME]@205.210.128.25 'cat ~/.ssh/id_rsa')
```

3. At this point you can use `ssh hamfast` on your local to connect to hamfast through samwise. However, to add an alias you can modify your `.bash_profile`

```
echo "alias ham=\"ssh hamfast\"" >> ~/.bash_profile 
```

You should now be able to connect to hamfast directly using `ham` on your local computer

## HPC4Health (H4H)

HPC4Health consortium is a Compute Canada node dedicated to support biomedical research. The consortium is a collaborative effort between Toronto’s Downtown Hospitals and related health research institutions to address high performance computing (HPC) needs in research environments encompassing patient and other sensitive data. Please consult the Queue Resources document for memory and time management prior to job submissions.&#x20;

{% hint style="info" %}
For more detailed information, please take a look at this documentation: [H4H Slurm cluster](https://onedrive.live.com/view.aspx?resid=3C4A8832156EA29!1115\&ithint=file%2cdocx\&authkey=!AAMab1i2E2R-m8M)
{% endhint %}

### Macfusion - Mount H4H <a href="#macfusion_-_mount_h4h" id="macfusion_-_mount_h4h"></a>

Macfusion settings:

```
IP: 172.27.23.173
User: YourID
Macfusion tab Mount point: ~/Desktop/H4H
```

1\. Setup SSH key on local

```
ssh-keygen -t rsa 
Generating public/private rsa key pair. 
Enter file in which to save the key (~/.ssh/id_rsa): (hit return) 
Enter passphrase (empty for no passphrase): (hit return) 
Enter same passphrase again: (hit return) 
Your identification has been saved in ~/.ssh/id_rsa 
Your public key has been saved in ~/.ssh/id_rsa.pub 
The key fingerprint is: 
A long string of random-looking characters
The key's randomart image is:
A 2D matrix of random-looking characters
```

2\. Copy key stored in \~/.ssh/id\_rsa to h4h

```
ssh-copy-id -i ~/.ssh/id_rsa YourID@172.27.23.173
```

Note: if you have an earlier version of mac OS and ssh-copy-id doesn't work, use this [ssh-copy-id](https://www.pmgenomics.ca/pughlabwiki/doku.php?id=personal:nwang:ssh-copy-id)

3\. Log in on terminal with “ssh YourID@172.27.23.173”. A pop-up should appear asking for your password, which will add your identity.

### Superhimem node

Now we have a node with 1.4T memory and 3.2T local ssd storage. I put this node on a separate partition “superhimem”.

When a job is launched on this node, a scratch directory called “/scratch/\<jobid>” will be created on the ssd storage and the following environmental variables will be set to this path, TMP, TEMP, TMPDIR and SLURM\_TMPDIR. User can use this directory for scratch space and the directory will be deleted after the job is finished. Please note, all jobs on the node will share this 3.2T scratch space.

Since we only have one node on the partition, It only accepts jobs that require more than 60G and the max runtime is 5 days.
