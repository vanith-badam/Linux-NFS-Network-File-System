# Linux-NFS-Network-File-System
Installation &amp; Configuration of NFS-Network File System


The Network File System (NFS) service provides, users to access Files Stored in Remote NFS Server.

An NFS server can export a Network File System that NFS Clients will then mount dynamically.

****Setting UP NFS Remote Server****

***Install required Packages for NFS***
```
$ sudo yum install nfs-utils -y
```
***Start and enable NFS Services***
```
$ sudo systemctl start rpcbind nfs-server
$ sudo systemctl enable rpcbind nfs-server
```
***Create a new Directory to share through the NFS***
```
$ mkdir /nfsshare-server
```
***Edit /etc/exports file with following lines***
```
$ vim /etc/exports 
```

> /nfsshare 192.168.100.10(rw,sync,no_root_squash,no_subtree_check) \
/nfsshare 192.168.100.25(rw,sync,no_root_squash,no_subtree_check)

This above lines indicates that **/nfsshare** Directory will share with following IP Address server with Read & Write Access to the Directoy

***Load the /etc/exports for the new changes***
```
$ exportfs -a
```

Allowing NFS Service through the Firewalld

```
$ firewall-cmd --add-service=nfs --zone=public --permanent
$ firewall-cmd --add-service=mountd --zone=public --permanent
$ firewall-cmd --add-service=rpc-bind --zone=public --permanent
$ firewall-cmd --reload
```

***Restart NFS Service***
```
$ sudo systemctl restart nfs.service
```

****Setting up NFS in Client machines side****

***Install required Packages for NFS***
```
$ sudo yum install nfs-utils -y
```

***Create nfs share directories on Client Machines to receive/send the files***
```
$ mkdir /nfsshare-client
```

***Execute the following commands from client machines to show the NFS Server mount points***
```
$ showmount -e 192.168.100.1
$ rpcinfo -p 192.168.100.1
```

IP address of the NFS Server

***To Mount NFS Share directory***

```
mount 198.168.100.1:/nfsshare-server  /nfsshare-client
```

***Check for mount points***
```
df -h
```

***To Mount NFS automatic***

**Edit following lines in the /etc/fstab file on Client Machine side**
```
$ vim /etc/fstab
```

> 192.168.100.1:/nfsshare-server  /nfsshare-client   nfs   defaults   0   0   

***Execute Following Command to mount the fstab entries***

```
mount -a
```
