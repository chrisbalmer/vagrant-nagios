# Nagios Vagrant Build

This is a simple Vagrant build that will install and configure a basic Nagios system. The file is meant for CentOS/RHEL 7 or 7.1 but should work with any yum and systemd based system.

## Settings You Should Change

- `box` name
- `private_network` IP or removing the extra adapter
- The salt configuration or removing it
- `hostname`
- `ram`

## Requirements

- Internet on one of the network adapters
- Yum based Vagrant box with systemd

## How To Use

- Change settings to match your needs
- Run `vagrant up`
- Once built, browse to `http://localhost:8080/nagios` in your web browser
- Default login username is `nagiosadmin` and password is `nagiosadmin`
