#Onsidian notes
# Installing openshift Local

Hypervisor : VMWare Fusion
Enable virtualization in BIOS
Rocky8.9
Installation type  : Virtualization Host (Minimal virtualization Host)
IP: 192.168.59.173

create a user - jut and give him sudo rights
```
adduser jut
passwd jut

visdo
```

Update system packages
```
sudo dnf update
```

Disable SELinux
```
sudo vi /etc/sysconfig/selinux

# Change SELINUX=enforcing to permissive
SELINUX=permissive
```

Requirements
* Network manager
* libvirt
* Qemu (qemu-kvm)

Download openshift Local for linux
```
mkdir Downloads
cd Downloads/
wget https://developers.redhat.com/content-gateway/rest/mirror/pub/openshift-v4/clients/crc/latest/crc-linux-amd64.tar.xz

ls .
crc-linux-amd64.tar.xz

# Uncompress the archive

$ tar xvf crc-linux-amd64.tar.xz 
crc-linux-2.16.0-amd64/
crc-linux-2.16.0-amd64/LICENSE
crc-linux-2.16.0-amd64/crc

$ mkdir -p ~/local/bin

$ mv crc-linux-*-amd64/crc ~/local/bin/

$ export PATH=$HOME/local/bin:$PATH

$ crc version

CRC version: 2.46.0+8f40e8

OpenShift version: 4.17.10

MicroShift version: 4.17.10

$ echo 'export PATH=$HOME/local/bin:$PATH' >> ~/.bashrc 



```

Setup the machine, I don't need telemetry as this will require more disk space

```
$ crc config set consent-telemetry no
Successfully configured consent-telemetry to no

$ crc config view
- consent-telemetry                     : no
```

Run the setup to configure the machine
```
$ crc setup
# if everything goes well, you have this message at the end
Your system is correctly setup for using CRC. Use 'crc start' to start the instance
```

Start Openshift Local
```
$ crc config set cpus 8
$ crc config set memory 11000
# Recommended memory is 16384

crc config view
- consent-telemetry                     : no
- cpus                                  : 8
- memory                                : 11000
```


Collect secret from redhat and start the crc
```
crc start -p ./pull-secret
INFO Loading bundle: crc_libvirt_4.17.10_amd64... 

INFO A CRC VM for OpenShift 4.17.10 is already running 

Started the OpenShift cluster.

  

The server is accessible via web console at:
  https://console-openshift-console.apps-crc.testing

  

Log in as administrator:
  Username: kubeadmin
  Password: ahh67-47mEH-GUMDt-hCDbX

  

Log in as user:
  Username: developer
  Password: developer

  

Use the 'oc' command line interface:
  $ eval $(crc oc-env)
  $ oc login -u developer https://api.crc.testing:6443

# Make sure it runs
$ crc status

CRC VM:          Running
OpenShift:       Unreachable (v4.17.10)
RAM Usage:       17.7GB of 32.68GB
Disk Usage:      5.029GB of 11.21GB (Inside the CRC VM)
Cache Usage:     25.89GB
Cache Directory: /home/jut/.crc/cache
