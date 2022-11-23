# Ansible Role: Base Configuration for Debian/RHEL

[![CI](https://github.com/radtkedev/ansible-role-base/workflows/CI/badge.svg?event=push)](https://github.com/radtkedev/ansible-role-base/actions?query=workflow%3ACI)

A role for configuring common server settings on Debian and RHEL-based systems.

## Usage
To be completed.

## Tasks
- Update system and install basic packages
- Reduce bootloader wait time from 5s to 1s (grub)
- Automated security updates (dnf-automatic or unattended-upgrades)
- SSH keys and configuration
- NTP time servers specified (chronyd or systemd-timesyncd)
- Configure SELinux
- Create swapfile

## Configuration
Variables for configuration are provided in vars/main.yml.

Packages can be configured via the list. Note that these packages are available under both Debian and RHEL-based systems, others you add may not be.
```
packages:
  - net-tools
  - vim
  - zip
  - unzip
```

The SSH directory allows you to specify where to install the ssh key(s).
```
ssh_dir: "/root"
ssh_keys:
  - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDPiflDGONaY4ppQqOWh9tlo6bDwY5BfRqWcHyAv/dev
```

Common SSH settings are configurable too, but just the following variables are configurable.
```
ssh_permitrootlogin: "prohibit-password"
ssh_passwordauthentication: "no"
ssh_challengeresponseauthentication: "no"
ssh_printmotd: "no"
ssh_printlastlog: "no"
```

NTP will be configured using chronyd on RHEL or systemd-timesyncd on Debian if enabled
```
ntp_configure: "true"
ntp_servers:
  - time.cloudflare.com
```

SELinux will be configured if RHEL is used
```
selinux_policy: "targeted"
selinux_state: "enforcing"
```

Swapfiles are also used, this setting can be disabled if swap partitions are used
```
swapfile_enabled: "true"
swapfile_size: "2G"
```

Autoupdate will be configured using dnf-automatic on RHEL or unattended-upgrades on Debian where automatic reboot time can be specified
```
autoupdate_enabled: "true"
autoupdate_debian_reboot: "false"
autoupdate_debian_reboot_time: "03:00"
```

To allow boot menu selection while not extending the boot time by 5 seconds, the role configures the timeout to 1s by default
```
grub_timeout: "1"
```
