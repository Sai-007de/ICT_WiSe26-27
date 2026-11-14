# Lecture 5 – Task 3: Text Processing with `sed` and `awk`

**Student:** Sainath Desai
**Working environment:** Ubuntu Linux accessed through SSH

This document records the commands used and the terminal outputs supplied for the five Task 3 exercises.

> **Evidence note:** Section 3.1 contains the first 10 lines of the modified `processes.log` as supplied. It shows the `ROOT` replacement in the file; it is not a full transcript of the `sed` command. Section 3.2 contains the resulting filtered `users.txt` contents.

## 3.1 – Replace `root` with `ROOT` in `processes.log`

**Command(s):**

```bash
sed -i 's/^root/ROOT/' processes.log
```

**Terminal output:**

```text
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
ROOT           1  0.5  0.5  25792 16864 ?        Ss   18:32   0:06 /usr/lib/systemd/systemd --switched-root --system --deserialize=56
ROOT           2  0.0  0.0      0     0 ?        S    18:32   0:00 [kthreadd]
ROOT           3  0.0  0.0      0     0 ?        S    18:32   0:00 [pool_workqueue_release]
ROOT           4  0.0  0.0      0     0 ?        I<   18:32   0:00 [kworker/R-rcu_gp]
ROOT           5  0.0  0.0      0     0 ?        I<   18:32   0:00 [kworker/R-sync_wq]
ROOT           6  0.0  0.0      0     0 ?        I<   18:32   0:00 [kworker/R-kvfree_rcu_reclaim]
ROOT           7  0.0  0.0      0     0 ?        I<   18:32   0:00 [kworker/R-slub_flushwq]
ROOT           8  0.0  0.0      0     0 ?        I<   18:32   0:00 [kworker/R-netns]
ROOT          10  0.0  0.0      0     0 ?        I<   18:32   0:00 [kworker/0:0H-kblockd]
```

## 3.2 – Create `users.txt` and remove entries containing `nologin`

**Command(s):**

```bash
cat /etc/passwd > users.txt
sed -i '/nologin/d' users.txt
```

**Terminal output:**

```text
root:x:0:0:root:/root:/bin/bash
sync:x:4:65534:sync:/bin:/bin/sync
dhcpcd:x:997:997:DHCP Client Daemon:/usr/lib/dhcpcd:/bin/false
sainath-desai:x:1000:1000:Sainath:/home/sainath-desai:/bin/bash
whoopsie:x:104:109::/nonexistent:/bin/false
lightdm:x:109:115:Light Display Manager:/var/lib/lightdm:/bin/false
hplip:x:111:7:HPLIP system user:/run/hplip:/bin/false
gdm:x:976:976:Gnome Display Manager:/var/lib/gdm3:/bin/false
```

## 3.3 – Print username, UID and GID using AWK

**Command(s):**

```bash
awk -F: '{printf "User %s has UID %s and GID %s\\n", $1, $3, $4}' /etc/passwd
```

**Terminal output:**

```text
User root has UID 0 and GID 0
User daemon has UID 1 and GID 1
User bin has UID 2 and GID 2
User sys has UID 3 and GID 3
User sync has UID 4 and GID 65534
User games has UID 5 and GID 60
User man has UID 6 and GID 12
User lp has UID 7 and GID 7
User mail has UID 8 and GID 8
User news has UID 9 and GID 9
User uucp has UID 10 and GID 10
User proxy has UID 13 and GID 13
User www-data has UID 33 and GID 33
User backup has UID 34 and GID 34
User list has UID 38 and GID 38
User irc has UID 39 and GID 39
User _apt has UID 42 and GID 65534
User nobody has UID 65534 and GID 65534
User systemd-network has UID 998 and GID 998
User dhcpcd has UID 997 and GID 997
User messagebus has UID 996 and GID 996
User systemd-resolve has UID 990 and GID 990
User _chrony has UID 989 and GID 989
User usbmux has UID 100 and GID 46
User polkitd has UID 988 and GID 988
User syslog has UID 101 and GID 103
User uuidd has UID 102 and GID 104
User tcpdump has UID 986 and GID 986
User tss has UID 985 and GID 985
User landscape has UID 103 and GID 106
User fwupd-refresh has UID 984 and GID 984
User sainath-desai has UID 1000 and GID 1000
User sshd has UID 983 and GID 65534
User rtkit has UID 981 and GID 981
User dnsmasq has UID 999 and GID 65534
User whoopsie has UID 104 and GID 109
User avahi has UID 105 and GID 111
User cups-pk-helper has UID 106 and GID 112
User saned has UID 107 and GID 113
User colord has UID 979 and GID 979
User geoclue has UID 108 and GID 114
User lightdm has UID 109 and GID 115
User cups-browsed has UID 110 and GID 112
User pipewire has UID 978 and GID 978
User hplip has UID 111 and GID 7
User gnome-remote-desktop has UID 977 and GID 977
User gdm has UID 976 and GID 976
```

## 3.4 – Print username and process name using AWK

**Command(s):**

```bash
awk 'NR==1 {print "USERNAME", "PROCESS_NAME"; next} $0 !~ / processes are running$/ {print $1, $11}' processes.log
```

**Terminal output:**

```text
USERNAME PROCESS_NAME
ROOT /usr/lib/systemd/systemd
ROOT [kthreadd]
ROOT [pool_workqueue_release]
ROOT [kworker/R-rcu_gp]
ROOT [kworker/R-sync_wq]
ROOT [kworker/R-kvfree_rcu_reclaim]
ROOT [kworker/R-slub_flushwq]
ROOT [kworker/R-netns]
ROOT [kworker/0:0H-kblockd]
ROOT [kworker/u12:0-ipv6_addrconf]
ROOT [kworker/R-mm_percpu_wq]
ROOT [ksoftirqd/0]
ROOT [rcu_preempt]
ROOT [rcu_exp_par_gp_kthread_worker/0]
ROOT [rcu_exp_gp_kthread_worker]
ROOT [migration/0]
ROOT [kprobe-optimizer]
ROOT [idle_inject/0]
ROOT [cpuhp/0]
ROOT [cpuhp/1]
ROOT [idle_inject/1]
ROOT [migration/1]
ROOT [ksoftirqd/1]
ROOT [kworker/1:0-mm_percpu_wq]
ROOT [kworker/1:0H-kblockd]
ROOT [cpuhp/2]
ROOT [idle_inject/2]
ROOT [migration/2]
ROOT [ksoftirqd/2]
ROOT [kdevtmpfs]
ROOT [kworker/R-inet_frag_wq]
ROOT [rcu_tasks_kthread]
ROOT [rcu_tasks_rude_kthread]
ROOT [kauditd]
ROOT [khungtaskd]
ROOT [oom_reaper]
ROOT [kworker/R-writeback]
ROOT [kcompactd0]
ROOT [ksmd]
ROOT [khugepaged]
ROOT [kworker/R-kblockd]
ROOT [kworker/R-blkcg_punt_bio]
ROOT [kworker/R-kintegrityd]
ROOT [irq/9-acpi]
ROOT [kworker/R-tpm_dev_wq]
ROOT [kworker/R-ata_sff]
ROOT [kworker/R-md_bitmap]
ROOT [kworker/R-md_llbitmap_io]
ROOT [kworker/R-md_llbitmap_unplug]
ROOT [kworker/R-edac-poller]
ROOT [kworker/R-devfreq_wq]
ROOT [watchdogd]
ROOT [kworker/R-quota_events_unbound]
ROOT [kworker/u14:1-ext4-rsv-conversion]
ROOT [kswapd0]
ROOT [ecryptfs-kthread]
ROOT [kworker/R-kthrotld]
ROOT [kworker/R-acpi_thermal_pm]
ROOT [scsi_eh_0]
ROOT [kworker/R-scsi_tmf_0]
ROOT [scsi_eh_1]
ROOT [kworker/R-scsi_tmf_1]
ROOT [kworker/u14:5-flush-252:0]
ROOT [kworker/2:1-cgroup_free]
ROOT [kworker/R-mld]
ROOT [kworker/R-ipv6_addrconf]
ROOT [kworker/u12:1-ipv6_addrconf]
ROOT [kworker/R-kstrp]
ROOT [kworker/u15:1-events_power_efficient]
ROOT [kworker/u16:0]
ROOT [kworker/u17:0-ttm]
ROOT [kworker/u18:0]
ROOT [kworker/u19:0]
ROOT [kworker/R-charger_manager]
ROOT [kworker/u15:2-events_unbound]
ROOT [kworker/u15:3-events_unbound]
ROOT [scsi_eh_2]
ROOT [kworker/R-scsi_tmf_2]
ROOT [kworker/1:1H-kblockd]
ROOT [kworker/R-kdmflush/252:0]
ROOT [jbd2/dm-0-8]
ROOT [kworker/R-ext4-rsv-conversion]
ROOT [kworker/0:1H-kblockd]
ROOT [kworker/0:4-events]
ROOT [psimon]
ROOT /usr/lib/systemd/systemd-journald
ROOT [kworker/2:1H-kblockd]
ROOT [kworker/R-kmpathd]
ROOT [kworker/R-kmpath_handlerd]
ROOT /usr/sbin/multipathd
systemd+ /usr/lib/systemd/systemd-resolved
ROOT /usr/lib/systemd/systemd-udevd
ROOT [psimon]
ROOT [irq/18-vmwgfx]
ROOT [kworker/R-ttm]
ROOT [kworker/u13:4-events_power_efficient]
ROOT [kworker/u13:5-events_unbound]
ROOT [jbd2/sda2-8]
ROOT [kworker/R-ext4-rsv-conversion]
systemd+ /usr/lib/systemd/systemd-networkd
ROOT [kworker/R-cfg80211]
ROOT /usr/libexec/accounts-daemon
avahi avahi-daemon:
ROOT /bin/sh
message+ @dbus-daemon
ROOT /usr/bin/python3
polkitd /usr/lib/polkit-1/polkitd
ROOT /usr/lib/snapd/snapd
ROOT /usr/libexec/switcheroo-control
ROOT /usr/lib/systemd/systemd-logind
ROOT /usr/libexec/udisks2/udisksd
avahi avahi-daemon:
syslog /usr/sbin/rsyslogd
ROOT /usr/sbin/NetworkManager
ROOT /usr/sbin/wpa_supplicant
_chrony /usr/sbin/chronyd
_chrony /usr/sbin/chronyd
ROOT /usr/sbin/ModemManager
ROOT /usr/bin/VBoxDRMClient
ROOT /usr/sbin/VBoxService
ROOT /usr/bin/python3
ROOT sshd:
ROOT /usr/sbin/cron
ROOT /usr/sbin/lightdm
ROOT login
ROOT /usr/lib/xorg/Xorg
ROOT [kworker/2:2H]
ROOT /usr/libexec/power-profiles-daemon
ROOT [kworker/u17:1-ttm]
ROOT lightdm
lightdm /usr/lib/systemd/systemd
lightdm (sd-pam)
lightdm /usr/bin/dbus-daemon
lightdm /usr/bin/pipewire
lightdm /bin/sh
lightdm /usr/sbin/lightdm-gtk-greeter
lightdm /usr/bin/wireplumber
lightdm /usr/bin/pipewire
lightdm /usr/bin/pipewire-pulse
rtkit /usr/libexec/rtkit-daemon
lightdm /usr/libexec/at-spi-bus-launcher
lightdm /usr/bin/dbus-daemon
lightdm /usr/libexec/gvfsd
lightdm /usr/libexec/gvfsd-metadata
ROOT lightdm
lightdm /usr/libexec/at-spi2-registryd
ROOT /usr/sbin/agetty
ROOT /usr/sbin/agetty
ROOT /usr/sbin/agetty
sainath+ /usr/lib/systemd/systemd
sainath+ (sd-pam)
sainath+ /usr/bin/dbus-daemon
sainath+ /usr/bin/pipewire
sainath+ -bash
sainath+ /usr/bin/mpris-proxy
sainath+ /usr/bin/wireplumber
sainath+ /usr/bin/pipewire
sainath+ /usr/bin/pipewire-pulse
ROOT sshd-session:
sainath+ sshd-session:
sainath+ -bash
ROOT [kworker/0:0-cgroup_free]
ROOT [kworker/1:1-events]
ROOT /usr/sbin/cupsd
lp /usr/lib/cups/notifier/dbus
cups-br+ /usr/sbin/cups-browsed
ROOT [kworker/u14:0-events_unbound]
ROOT [kworker/2:2-events]
ROOT [kworker/u13:0-events_power_efficient]
sainath+ ps
```

## 3.5 – Calculate total CPU usage using AWK

**Command(s):**

```bash
awk 'NR>1 && $0 !~ / processes are running$/ {sum += $3} END {printf "Total CPU usage of all listed processes: %.2f%%\\n", sum}' processes.log
```

**Terminal output:**

```text
Total CPU usage of all listed processes: 102.20%
```

## Result

The supplied AWK output reports a total CPU usage of **102.20%** for the listed processes in the captured `processes.log` data.
