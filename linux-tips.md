# Linux Tips

## Find out installed packages

List installed packages into file and download it via `scp` to the local machine:

```bash
yum list installed > /tmp/installed
apt list --installed > /tmp/installed

# sudo scp -C -i <private-key.pem> user@ip:/remote-file /local-file
# -C enable compression
# -r - to copy directory recursively
sudo scp -C -i ~/Downloads/i-0a895945becf5aab4.pem ec2-user@10.66.48.73:/tmp/installed /home/me/Downloads/installed.txt
```

## Archive and download necessary directories

### Zip Archiver

```bash
zip -r resulting_archive.zip /input/dir/
```

Example:
```bash
zip -r init_d.zip /etc/init.d/
zip -r cron_daily.zip /etc/cron.daily/
```

### Tar Archiver

Archiving:
```bash
# Archive discovery/ directory into discovery.tar.gz
tar -czf discovery.tar.gz discovery

# Exclude folder itself
cd folder
tar -czf ../folder.tar.gz *

# Archive gbuildserver/ into gbuildserver.tar.gz excluding directory log/
tar --exclude='./gbuildserver/log' -czvf gbuildserver.tar.gz ./gbuildserver/
```

Extracting:
```bash
# Extracting discovery.tar.gz into current directory
tar -xzf discovery.tar.gz -C .
```

## Cron Jobs

Save cron jobs into a file:
```bash
sudo crontab -l > crontab-jobs.txt
sudo crontab -u me -l > crontab-me-jobs.txt
```

## File System Architecture

Commands to find out system architecture:

```bash
uname -a
uname -m
arch
file /sbin/init
file /lib/systemd/systemd
```

Output for `uname -a`:

```
Linux uzbumid 4.8.0-52-generic #55~16.04.1-Ubuntu SMP Fri Apr 28 14:36:29 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
```

Output for `uname -m` and `arch`:

```
x86_64
```

Output for `file /sbin/init` and `file /lib/systemd/systemd`:

```
/lib/systemd/systemd: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=e85ece34384dcfcb866039125bcad65ebb13d55b, stripped
```

## LVM

Displaying volume groups

```bash
sudo vgs
sudo vgdisplay
sudo vgscan
```

Displaying physical volumes

```bash
sudo pvs
sudo pvdisplay
sudo pvscan
```

Displaying logical volumes

```bash
sudo lvs
sudo lvdisplay
sudo lvscan
```

## Network configurations

```bash
ip a
ip r
```

### iptables

List firewall rules

```bash
sudo iptables -S
sudo ip6tables -S

# display as table
sudo iptables -L
```

Export `iptables` rules

```bash
sudo iptables-save > iptables-rules.txt
sudo ip6tables-save > ip6tables-rules.txt
```

Import `iptables` rules

```bash
sudo iptables-restore < iptables-rules.txt
sudo ip6tables-restore < ip6tables-rules.txt
```

Persist `iptables` rules upon reboot:

```bash
# On Ubuntu
sudo invoke-rc.d iptables-persistent save

# On CentOS
sudo service iptables save
```

## Bash/Shell flags

- `set -e` exits immediately when a command fails
- `set -u` treats unset variables as an error and exits immediately
- `set -x` prints each command before executing it
- `set -o pipefail` sets the exit code of a pipeline to the rightmost command to exit with a non-zero status, or zero if all commands of the pipeline exit successfully

## Using shell variable inside `sed`

```bash
# $var is a shell variable
sed -i "s|$var|r_str|g" file_name
```

## Signals

- [Handling signals in bash](http://linuxcommand.org/wss0160.php)

## Find word from files content inside current directory and its subdirectories

```bash
grep -r word *
```

## Convert *.ppk to *.pem

```bash
sudo apt-get install putty-tools
puttygen mykey.ppk -O private-openssh -o mykey.pem
ssh -i mykey.pem someuser@somehost
```

## Remove ssh key entry for host from ssh config file

```bash
ssh-keygen -f "/home/umidjons/.ssh/known_hosts" -R 172.17.0.3
```

## Network monitoring with `bmon`

Install `bmon` with `sudo apt-get install bmon`.

Then just invoke `bmon` from your terminal. You will see something like this:
```
 lo                                                                                                                                                                                                                bmon 3.8 
Interfaces                     │ RX bps       pps     %│ TX bps       pps     %
 >lo                           │   1.61KiB      6      │   1.61KiB      6
    qdisc none (noqueue)       │      0         0      │      0         0
  enp3s0                       │      0         0      │      0         0
    qdisc none (pfifo_fast)    │      0         0      │      0         0
  wlp2s0                       │     70B        0      │     67B        0
    qdisc none (pfifo_fast)    │      0         0      │     65B        0
  br-7900c128755b              │      0         0      │      0         0
    qdisc none (noqueue)       │      0         0      │      0         0
  docker0                      │      0         0      │      0         0
    qdisc none (noqueue)       │      0         0      │      0         0
  tun0                         │      0         0      │      0         0
    qdisc none (pfifo_fast)    │      0         0      │      0         0
  veth515454c                  │      0         0      │      0         0
    qdisc none (noqueue)       │      0         0      │      0         0
───────────────────────────────┴───────────────────────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
     KiB                      (RX Bytes/second)                                                                    KiB                      (TX Bytes/second)
    1.66 ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||                                             1.66 ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
    1.38 ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||                                             1.38 ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
    1.11 ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||                                             1.11 ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
    0.83 ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||                                             0.83 ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
    0.55 ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||                                             0.55 ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
    0.28 ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||                                             0.28 ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
         1   5   10   15   20   25   30   35   40   45   50   55   60                                                  1   5   10   15   20   25   30   35   40   45   50   55   60


─────────────────────────────────────────────────────────────────────────────────────────── Press d to enable detailed statistics ──────────────────────────────────────────────────────────────────────────────────────────
───────────────────────────────────────────────────────────────────────────────────────── Press i to enable additional information ─────────────────────────────────────────────────────────────────────────────────────────
 Mon Jun 26 10:40:48 2017                                                                                                                                                                                  Press ? for help 
```

# My public IP address

```bash
curl ipinfo.io/ip
```

Sample output:
```
213.230.79.199
```

# rsync command example

```bash
rsync -e="ssh -ttt -i /tmp/1" \
    --protocol=30 \
    --rsync-path="sudo rsync" \
    -aAHXvzr --stats --human-readable --delete --itemize-changes \
    saasops@192.168.1.3:/ftp /opt/st1/ftp
```

`rsync` with resume option
```bash
sudo rsync --rsh='ssh -i 192.168.1.3.pem' -av --progress --partial user@192.168.1.3:/opt/bitnami/bit_file.tar.gz .
```

# wget with retry and resume options

```bash
# --continue, -c - resume download if server supports Range header
# --tries, -t    - retry 100 times on fails
# --waitretry    - wait 1s between retries
# --timeout, -T  - timeout 10s
#
# --progress=bar - bar indicator, like the following
# 25% [++++++++++++++++++++++++++++++++++++=>    ] 10,111,389   117KB/s  eta 4m 17s ^
#
# --progress=dot - dot indicator, like the following
#       0K ,,,,,,,, ,,,,,,,, ,,,,,,,, ,,,,,,,, ,,,,,,,, ,,,,,...  7% 5.49M 2m29s
#    3072K ........ ........ ........ .....
wget --progress=bar:force:noscroll --continue --tries=100 --waitretry=1 --timeout=10 -O moodle-latest-32.tgz https://download.moodle.org/stable32/moodle-latest-32.tgz
wget --progress=dot:mega --continue --tries=100 --waitretry=1 --timeout=10 -O moodle-latest-32.tgz https://download.moodle.org/stable32/moodle-latest-32.tgz
```

# Mount remote windows folder on ubuntu

Share some folder on a remote windows machine with read-write permission.

On ubuntu, invoke following commands:

```bash
# prepare target mount point
sudo mkdir /media/some_folder

# install required packages
sudo apt-get update
sudo apt-get install samba smbclient cifs-utils

# check connection to the remote windows with samba client
# this is also useful to determine compatible samba version
# if you get negotiation error, then try to specify samba version with -m option
smbclient -L 192.168.0.105 -U user1 -d 256

# specify samba version -m SMB2|SMB3
smbclient -L 192.168.0.105 -U user1 -m SMB2

# mount shared remote windows folder into target mount point
sudo mount -t cifs -o username=user1 //192.168.0.105/SomeSharedFolder /media/some_folder -o vers=2.0,rw

# check mount
sudo mount | grep some_folder

# Here you can ready to trasfer files
# for example
sudo mc
cd /media/some_folder

# or
cp ~/my_file /media/some_folder/
```

# List recently created/modified files

```bash
find <your dir> -type f -print0 | xargs -0 stat --format '%Y :%y %n' | sort -nr | cut -d: -f2- | head
# for example:
find /data/html -type f -print0 | xargs -0 stat --format '%Y :%y %n' | sort -nr | cut -d: -f2- | head
```
