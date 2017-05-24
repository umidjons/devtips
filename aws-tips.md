# AWS Tips

## Connect to the Amazon EC2 Instance

```bash
sudo ssh -i <key.pem> ec2-user@<instance-ip>
```

## Work with packages

List available packages:
```bash
yum --showduplicates list php
yum --showduplicates list php-5.3.29
```

Search packages:
```bash
yum search all <package name>
yum search all mod_ssl
```

Find out package dependencies without installing it:
```bash
# We need repoquery from yum-utils
yum install -y yum-utils
# Query package dependencies
repoquery --requires --resolve <desired-package>
```

Alternative way to find out package dependencies:
```bash
yum deplist <desired-package>
```
