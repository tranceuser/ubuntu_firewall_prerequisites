# Installing IPTables, Ipset, Conntrack, and Netfilter-Persistent on Ubuntu Server 22.04

This guide will walk you through the process of installing IPTables, Ipset, Conntrack, and Netfilter-Persistent on Ubuntu Server 22.04.

## Prerequisites
Before we begin, you should have a working installation of Ubuntu Server 22.04, with root or sudo privileges. You should also have basic knowledge of the terminal and command-line interface.

## Installing IPTables
To install IPTables, use the following command:
```
sudo apt-get install iptables -y
```

Once the installation is complete, you can verify the installation by running the following command:
```
sudo iptables -v
```

## Installing Ipset
To install Ipset, use the following command:
```
sudo apt-get install ipset -y
```
Once the installation is complete, you can verify the installation by running the following command:
```
sudo ipset -version
```

## Installing Conntrack
To install Conntrack, use the following command:
```
sudo apt-get install conntrack -y
```
Once the installation is complete, you can verify the installation by running the following command:
```
sudo conntrack -V
```

## Installing Netfilter-Persistent
To install Netfilter-Persistent, use the following command:
```
sudo apt-get install netfilter-persistent -y
```
Once the installation is complete, you can verify the installation by running the following command:
```
sudo netfilter-persistent version
```

netfilter-persistent will automatically run at startup on Ubuntu Server 22.04. This means that your firewall rules, including those created using ipset, will be restored and reapplied after every reboot.
If for some reason netfilter-persistent does not start automatically, you can check the status of the service by running the following command:
```
systemctl status netfilter-persistent
```
If the service is not active, you can start it by running the following command:
```
sudo systemctl start netfilter-persistent
```
You can also configure netfilter-persistent to start automatically at boot time by running the following command:
```
sudo systemctl enable netfilter-persistent
```

Here are some useful commands for managing the firewall rules using netfilter-persistent on Ubuntu Server 22.04:
1. sudo netfilter-persistent save - This command will save the current firewall rules and configuration to disk, so that they persist across reboots.
2. sudo netfilter-persistent reload - This command will reload the firewall rules from disk, discarding any changes made since the last save.
3. sudo netfilter-persistent flush - This command will flush all firewall rules and start with a clean slate. This is useful when you want to remove all existing firewall rules and start over.
4. sudo netfilter-persistent status - This command will show the status of netfilter-persistent, including whether it is active, inactive, or inactive with errors.
5. sudo iptables -L -n -v - This command will list the current IPTables rules, including any rules created using ipset, in a human-readable format.

Note that the netfilter-persistent command should be run with root or sudo privileges, as it requires access to the firewall configuration.
