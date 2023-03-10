# Installing IPTables, Ipset, Conntrack and netfilter-persistent on Ubuntu Server 22.04

This guide will walk you through the process of installing IPTables, Ipset, Conntrack and netfilter-persistent on Ubuntu Server 22.04.

## Prerequisites
Before we begin, you should have a working installation of Ubuntu Server 22.04, with root or sudo privileges. You should also have basic knowledge of the terminal and command-line interface.

Update your package list:
```
sudo apt update -y
```

## Uninstall the default firewall in Ubuntu
1. Type the following command to stop the firewall service:
```
sudo systemctl stop ufw
```
2. Disable the firewall service by typing the following command:
```
sudo systemctl disable ufw
```
3. Finally, you can remove the ufw package by typing the following command:
```
sudo apt-get remove ufw -y
```

## Installing IPTables
To install IPTables, use the following command:
```
sudo apt-get install iptables -y
```

Once the installation is complete, you can verify the installation by running the following command:
```
sudo iptables -V
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

## netfilter-persistent

ipset-persistent, iptables-persistent, and netfilter-persistent are packages for Ubuntu Server that allow you to save and load your IPSET and IPTABLES rules automatically on boot. This ensures that your firewall settings are always loaded, even after a reboot.

1. Install the packages:
```
sudo apt install ipset-persistent iptables-persistent netfilter-persistent -y
```
2. Once the installation is complete, netfilter-persistent services should be automatically started. You can check their status using the systemctl command:
```
sudo systemctl status netfilter-persistent
```
If the services are running, you should see a message indicating that they are active (running).

3. To save your IPSET and IPTABLES rules, use the following commands:
```
sudo ipset save > /etc/ipset.rules
sudo iptables-save > /etc/iptables/rules.v4
sudo netfilter-persistent save
```
4. To start the services, use the following commands:
```
sudo systemctl start netfilter-persistent
```
5. You can save both ipset and iptables configurations by running the command:
```
service netfilter-persistent save
```

## Save & Restore service (Alternative to netfilter-persistent)
1. Create a new systemd service file by running the following command:
```
sudo nano /etc/systemd/system/iptables-save.service
```
2. Paste the following content into the file:
```
[Unit]
Description=Save and restore ipset and iptables rules
Before=network.target

[Service]
Type=oneshot
RemainAfterExit=true
ExecStart=/bin/bash -c '/sbin/ipset restore < /etc/iptables/ipset.rules && /sbin/iptables-restore < /etc/iptables/rules.v4'
ExecStop=/bin/bash -c '/sbin/ipset save > /etc/iptables/ipset.rules && /sbin/iptables-save > /etc/iptables/rules.v4'

[Install]
WantedBy=multi-user.target
```
3. Save and close the file.
4. Reload the systemd daemon to pick up the changes:
```
sudo systemctl daemon-reload
```
5. Enable the service to run at boot time:
```
sudo systemctl enable iptables-save.service
```
6. Start the service:
```
sudo systemctl start iptables-save.service
```
7. Check the status:
```
sudo systemctl status iptables-save.service
```
