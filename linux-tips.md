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