# System Requirements

- Operating System and OSSIM

`OSSIM is distributed by Alienvault (https://www.alienvault.com/products/ossim) usually as a standalone Debian based Operating System. It must be previously installed and configured in the machine where the Service Level SIEM is going to be running. Detailed instructions about the installation can be found below. [# Installation]

OSSIM can be also installed from the source code on a standard Debian or Ubuntu linux distribution. The following kernel parameters need to be added to the file /etc/sysctl.d/ossim.conf and applied:`
```
kernal.printk = 1 4 1 7
net.ipv4.tcp_timestamps = 0
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.all.autoconf = 0
net.ipv6.conf.default.autoconf = 0
kernal.panic = 10
fs.file-max=1000000
vm.swappiness=10
```
`
- Java VM
The service level SIEM requires Java release 1.6 or greater installed and the JAVA_HOME variable set in the user profile. In particular, the jdk1.6.0_37 has been used in the tests.

- Database
The Service Level SIEM requires a MySQL database. It is already installed with the OSSIM distribution.

- Storm Cluster
Storm 0.9.0-wip16 has been used in the Service Level SIEM. It is a free and open source project licensed under the Eclipse Public License (EPL) that can be downloaded from http://storm-project.net/downloads.html

NOTE: The Stable version v0.8.1 can be used if the Service level SIEM is going to be used only in Local Cluster. Some bugs have been found during its execution in Remote Cluster mode that is fixed in v0.8.1.
Storm has the following Requirements:
 - Apache Zookeeper Server package v3.4.9[stable] (http://zookeeper.apache.org/)
 - ZeroMQ v4.2.0[stable] (http://zeromq.org/intro:get-the-software)

 If you want to have a supervisory process that manages each of your ZooKeeper/Storm processes, it is required a supervisory process such as Daemon-tools (http://cr.yp.to/daemontools/install.html)
`
# Installation

>> OSSIM Installation

The most important points for a clean installation are listed here:

1. Install OSSIM from a bootable DVD/ISO
```
OSSIM is distributed as a standalone Debian based Operating System. The version used for the Service Level SIEM is v4.1.0.
You can download and burn the ISO from here: https://www.alienvault.com/products/ossim/ or http://downloads.alienvault.com/c/download?version=current_ossim_iso
Boot from the ISO to install the system. OSSIM requires a new partition in a physical or a virtual machine.
You can choose two types of Installation
- ALL-In-ONE OSSIM Installation
- OSSIM Sensors Installation
The wizard will require some parameters to configure the OSSIM (such as the IP, password, etc).
These parameters can be changed at any time after the installation is completed, and also the wizard provides an automated installation that will configure the OSSIM with the default parameters with root user.
An automated installation is enough for a beginner user, so you can safely go with this option.
```

2. Install OSSIM from the source
```
Although it is not the easiest way, if you want to install OSSIM on an existent standard Operating System such as Ubuntu or Debian, you can download the source code from Git and checkout the release 4.1.0:
```
```
# apt-get install -y git
# git clone https://git@git.assembla.com/os-sim.2.git
# cd os-sim.2
# git checkout tags/release-4.1.0
```
