# Installing IPTables, Ipset and Conntrack on Ubuntu Server 22.04

This guide will walk you through the process of installing IPTables, Ipset and Conntrack on Ubuntu Server 22.04.

## Prerequisites
Before we begin, you should have a working installation of Ubuntu Server 22.04, with root or sudo privileges. You should also have basic knowledge of the terminal and command-line interface.

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
sudo apt-get remove ufw
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
## Save & Restore service
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
