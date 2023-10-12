---
layout: default
title: SOP - server updates
parent: SOPs
---

# Server updates and upgrades

## Update a server

### 1. Check load, usage and logs

- [ ] disk space (`df -h`)
- [ ] cpu load (`htop`)
- [ ] memory (`htop`)
- [ ] container list (`docker ps`)
- [ ] dmesg (`dmesg -T`)
- [ ] syslog (`tail -f -n500 /var/log/syslog`)
- [ ] logged in users (`w and lastlog`)

- [ ] check if external monitoring is active
- [ ] check the firewall (`nmap -A | grep open`)
- [ ] check the backups

### 2. Reboot

Before we change anything we want to know if we can
still reboot. So that if that goes wrong we know it
is not the updates. Also have a quick look at the
application or verify that the server is doing what
it is supposed to do.

### 3. Snapshot / backups

A full snapshot of the disk or a quick run of the
backups. So that if something goes wrong we can easily
go back to the way it was. This depends on the server
and the kind of updates we're doing. For container hosts
this is not needed but for a complicated Ubuntu install
it definitely is.

### 4. Update

On Ubuntu and Debian use the following command as root:

```bash
apt update && apt dist-upgrade && apt autoremove && apt clean && date && reboot
```

Or without the questions if you are comfortable with that:

```bash
apt update && apt dist-upgrade -y && apt autoremove -y && apt clean && date && reboot
```

Or with screen on unstable connections:

```bash
sudo screen sh -c "apt update && apt dist-upgrade && apt autoremove && apt clean && date && reboot"
```

The date is there so it outputs the time before the reboot,
if you are waiting 5 minutes for it to come back you know
something is wrong.

Use `ssh -o 'ConnectionAttempts 999' <yourserver>` to reconnect automatically.

### 5. Final checks

- [ ] check if the application or the server is still doing its job
- [ ] check if [automatic security updates](https://docs.aikedejongste.nl/linux/apt.html) are enabled
- [ ] check the load, usage and logs
- [ ] register the update in the log for compliance
