# Ansible tips

## Ping on localhost

```bash
ansible localhost -m ping
ansible all -i "localhost," -c local -m ping
```

## Hello world task on localhost

```bash
ansible localhost -m shell -a "echo Hello World"
ansible all -i "localhost," -c local -m shell -a "echo Hello World"
```

## Hello World playbook example

File `helloworld.yml`:

```yaml
---
- hosts: all
  tasks:
    - shell: echo "Hello World"
```

Run a playbook:

```bash
ansible-playbook -i "localhost," -c local helloworld.yml
```

## `setup` module example to gather information about host

```bash
ansible all -i "localhost," -c local -m setup
```

Sample output:
```json
localhost | SUCCESS => {
    "ansible_facts": {
        "ansible_all_ipv4_addresses": [
            "172.17.0.1", 
            "192.168.1.180", 
            "44.174.79.10", 
            "52.205.53.1"
        ], 
        "ansible_all_ipv6_addresses": [
            "fe80::e206:e6ff:fed7:92ef", 
            "fe80::6030:bd10:b61c:c1e"
        ], 
        "ansible_apparmor": {
            "status": "enabled"
        }, 
        "ansible_architecture": "x86_64", 
        "ansible_bios_date": "08/16/2012", 
        "ansible_bios_version": "A09", 
        "ansible_br_7900c128755b": {
            "active": false, 
            "device": "br-7900c128755b", 
            "features": {
                "busy_poll": "off [fixed]", 
                "fcoe_mtu": "off [fixed]", 
                "generic_receive_offload": "on", 
                "generic_segmentation_offload": "on", 
                "highdma": "on", 
                "hw_tc_offload": "off [fixed]", 
                "l2_fwd_offload": "off [fixed]", 
                "large_receive_offload": "off [fixed]", 
                "loopback": "off [fixed]", 
                "netns_local": "on [fixed]", 
                "ntuple_filters": "off [fixed]", 
                "receive_hashing": "off [fixed]", 
                "rx_all": "off [fixed]", 
                "rx_checksumming": "off [fixed]", 
                "rx_fcs": "off [fixed]", 
                "rx_vlan_filter": "off [fixed]", 
                "rx_vlan_offload": "off [fixed]", 
                "rx_vlan_stag_filter": "off [fixed]", 
                "rx_vlan_stag_hw_parse": "off [fixed]", 
                "scatter_gather": "on", 
                "tcp_segmentation_offload": "on", 
                "tx_checksum_fcoe_crc": "off [fixed]", 
                "tx_checksum_ip_generic": "on", 
                "tx_checksum_ipv4": "off [fixed]", 
                "tx_checksum_ipv6": "off [fixed]", 
                "tx_checksum_sctp": "off [fixed]", 
                "tx_checksumming": "on", 
                "tx_fcoe_segmentation": "on", 
                "tx_gre_csum_segmentation": "on", 
                "tx_gre_segmentation": "on", 
                "tx_gso_partial": "on", 
                "tx_gso_robust": "on", 
                "tx_ipxip4_segmentation": "on", 
                "tx_ipxip6_segmentation": "on", 
                "tx_lockless": "on [fixed]", 
                "tx_nocache_copy": "off", 
                "tx_scatter_gather": "on", 
                "tx_scatter_gather_fraglist": "on", 
                "tx_sctp_segmentation": "on", 
                "tx_tcp6_segmentation": "on", 
                "tx_tcp_ecn_segmentation": "on", 
                "tx_tcp_mangleid_segmentation": "on", 
                "tx_tcp_segmentation": "on", 
                "tx_udp_tnl_csum_segmentation": "on", 
                "tx_udp_tnl_segmentation": "on", 
                "tx_vlan_offload": "on", 
                "tx_vlan_stag_hw_insert": "on", 
                "udp_fragmentation_offload": "on", 
                "vlan_challenged": "off [fixed]"
            }, 
            "id": "8000.024211683484", 
            "interfaces": [], 
            "ipv4": {
                "address": "52.205.53.1", 
                "broadcast": "global", 
                "netmask": "255.255.255.0", 
                "network": "52.205.53.0"
            }, 
            "macaddress": "02:42:11:68:34:84", 
            "mtu": 1500, 
            "promisc": false, 
            "stp": false, 
            "type": "bridge"
        }, 
        "ansible_cmdline": {
            "BOOT_IMAGE": "/boot/vmlinuz-4.8.0-56-generic", 
            "quiet": true, 
            "ro": true, 
            "root": "UUID=da13ddb6-4785-413b-bc76-9d5aebd1cf9a", 
            "splash": true
        }, 
        "ansible_date_time": {
            "date": "2017-06-29", 
            "day": "29", 
            "epoch": "1498723122", 
            "hour": "12", 
            "iso8601": "2017-06-29T07:58:42Z", 
            "iso8601_basic": "20170629T125842099378", 
            "iso8601_basic_short": "20170629T125842", 
            "iso8601_micro": "2017-06-29T07:58:42.099584Z", 
            "minute": "58", 
            "month": "06", 
            "second": "42", 
            "time": "12:58:42", 
            "tz": "+05", 
            "tz_offset": "+0500", 
            "weekday": "Thursday", 
            "weekday_number": "4", 
            "weeknumber": "26", 
            "year": "2017"
        }, 
        "ansible_default_ipv4": {
            "address": "192.168.1.180", 
            "alias": "wlp2s0", 
            "broadcast": "192.168.1.255", 
            "gateway": "192.168.1.1", 
            "interface": "wlp2s0", 
            "macaddress": "e0:06:e6:d7:92:ef", 
            "mtu": 1500, 
            "netmask": "255.255.255.0", 
            "network": "192.168.1.0", 
            "type": "ether"
        }, 
        "ansible_default_ipv6": {}, 
        "ansible_devices": {
            "sda": {
                "holders": [], 
                "host": "SATA controller: Intel Corporation 7 Series Chipset Family 6-port SATA Controller [AHCI mode] (rev 04)", 
                "model": "ST1000LM024 HN-M", 
                "partitions": {
                    "sda1": {
                        "holders": [], 
                        "sectors": "610470", 
                        "sectorsize": 512, 
                        "size": "298.08 MB", 
                        "start": "16065", 
                        "uuid": "8CBA-2EF4"
                    }, 
                    "sda2": {
                        "holders": [], 
                        "sectors": "6297480", 
                        "sectorsize": 512, 
                        "size": "3.00 GB", 
                        "start": "626535", 
                        "uuid": "506A-BB3C"
                    }, 
                    "sda3": {
                        "holders": [], 
                        "sectors": "419428352", 
                        "sectorsize": 512, 
                        "size": "200.00 GB", 
                        "start": "6924288", 
                        "uuid": "1B25D156EE69C259"
                    }, 
                    "sda4": {
                        "holders": [], 
                        "sectors": "2", 
                        "sectorsize": 512, 
                        "size": "1.00 KB", 
                        "start": "426354686", 
                        "uuid": null
                    }, 
                    "sda5": {
                        "holders": [], 
                        "sectors": "420579328", 
                        "sectorsize": 512, 
                        "size": "200.55 GB", 
                        "start": "426354688", 
                        "uuid": "B6C091592C0E59C5"
                    }, 
                    "sda6": {
                        "holders": [], 
                        "sectors": "976562500", 
                        "sectorsize": 512, 
                        "size": "465.66 GB", 
                        "start": "846936064", 
                        "uuid": "2ba1ca44-72fe-4522-8687-4ff13238ca96"
                    }, 
                    "sda7": {
                        "holders": [], 
                        "sectors": "32368640", 
                        "sectorsize": 512, 
                        "size": "15.43 GB", 
                        "start": "1921155072", 
                        "uuid": "5eeac6d6-5b5c-4191-8506-ee6e1b190627"
                    }, 
                    "sda8": {
                        "holders": [], 
                        "sectors": "97648640", 
                        "sectorsize": 512, 
                        "size": "46.56 GB", 
                        "start": "1823500288", 
                        "uuid": "da13ddb6-4785-413b-bc76-9d5aebd1cf9a"
                    }
                }, 
                "removable": "0", 
                "rotational": "1", 
                "sas_address": null, 
                "sas_device_handle": null, 
                "scheduler_mode": "deadline", 
                "sectors": "1953525168", 
                "sectorsize": "512", 
                "size": "931.51 GB", 
                "support_discard": "0", 
                "vendor": "ATA"
            }, 
            "sdb": {
                "holders": [], 
                "host": "USB controller: Intel Corporation 7 Series/C210 Series Chipset Family USB xHCI Host Controller (rev 04)", 
                "model": "My Passport 259D", 
                "partitions": {
                    "sdb1": {
                        "holders": [], 
                        "sectors": "3906959360", 
                        "sectorsize": 512, 
                        "size": "1.82 TB", 
                        "start": "2048", 
                        "uuid": "44500B85500B7D44"
                    }
                }, 
                "removable": "0", 
                "rotational": "1", 
                "sas_address": null, 
                "sas_device_handle": null, 
                "scheduler_mode": "deadline", 
                "sectors": "3906963456", 
                "sectorsize": "512", 
                "size": "1.82 TB", 
                "support_discard": "0", 
                "vendor": "WD"
            }, 
            "sr0": {
                "holders": [], 
                "host": "SATA controller: Intel Corporation 7 Series Chipset Family 6-port SATA Controller [AHCI mode] (rev 04)", 
                "model": "DVD+-RW GT50N", 
                "partitions": {}, 
                "removable": "1", 
                "rotational": "1", 
                "sas_address": null, 
                "sas_device_handle": null, 
                "scheduler_mode": "deadline", 
                "sectors": "2097151", 
                "sectorsize": "512", 
                "size": "1024.00 MB", 
                "support_discard": "0", 
                "vendor": "HL-DT-ST"
            }
        }, 
        "ansible_distribution": "Ubuntu", 
        "ansible_distribution_major_version": "16", 
        "ansible_distribution_release": "xenial", 
        "ansible_distribution_version": "16.04", 
        "ansible_dns": {
            "nameservers": [
                "10.212.70.10", 
                "10.212.71.10", 
                "127.0.1.1"
            ], 
            "search": [
                "devfactory.local"
            ]
        }, 
        "ansible_docker0": {
            "active": false, 
            "device": "docker0", 
            "features": {
                "busy_poll": "off [fixed]", 
                "fcoe_mtu": "off [fixed]", 
                "generic_receive_offload": "on", 
                "generic_segmentation_offload": "on", 
                "highdma": "on", 
                "hw_tc_offload": "off [fixed]", 
                "l2_fwd_offload": "off [fixed]", 
                "large_receive_offload": "off [fixed]", 
                "loopback": "off [fixed]", 
                "netns_local": "on [fixed]", 
                "ntuple_filters": "off [fixed]", 
                "receive_hashing": "off [fixed]", 
                "rx_all": "off [fixed]", 
                "rx_checksumming": "off [fixed]", 
                "rx_fcs": "off [fixed]", 
                "rx_vlan_filter": "off [fixed]", 
                "rx_vlan_offload": "off [fixed]", 
                "rx_vlan_stag_filter": "off [fixed]", 
                "rx_vlan_stag_hw_parse": "off [fixed]", 
                "scatter_gather": "on", 
                "tcp_segmentation_offload": "on", 
                "tx_checksum_fcoe_crc": "off [fixed]", 
                "tx_checksum_ip_generic": "on", 
                "tx_checksum_ipv4": "off [fixed]", 
                "tx_checksum_ipv6": "off [fixed]", 
                "tx_checksum_sctp": "off [fixed]", 
                "tx_checksumming": "on", 
                "tx_fcoe_segmentation": "on", 
                "tx_gre_csum_segmentation": "on", 
                "tx_gre_segmentation": "on", 
                "tx_gso_partial": "on", 
                "tx_gso_robust": "on", 
                "tx_ipxip4_segmentation": "on", 
                "tx_ipxip6_segmentation": "on", 
                "tx_lockless": "on [fixed]", 
                "tx_nocache_copy": "off", 
                "tx_scatter_gather": "on", 
                "tx_scatter_gather_fraglist": "on", 
                "tx_sctp_segmentation": "on", 
                "tx_tcp6_segmentation": "on", 
                "tx_tcp_ecn_segmentation": "on", 
                "tx_tcp_mangleid_segmentation": "on", 
                "tx_tcp_segmentation": "on", 
                "tx_udp_tnl_csum_segmentation": "on", 
                "tx_udp_tnl_segmentation": "on", 
                "tx_vlan_offload": "on", 
                "tx_vlan_stag_hw_insert": "on", 
                "udp_fragmentation_offload": "on", 
                "vlan_challenged": "off [fixed]"
            }, 
            "id": "8000.0242176a3250", 
            "interfaces": [], 
            "ipv4": {
                "address": "172.17.0.1", 
                "broadcast": "global", 
                "netmask": "255.255.0.0", 
                "network": "172.17.0.0"
            }, 
            "macaddress": "02:42:17:6a:32:50", 
            "mtu": 1500, 
            "promisc": false, 
            "stp": false, 
            "type": "bridge"
        }, 
        "ansible_domain": "", 
        "ansible_effective_group_id": 1000, 
        "ansible_effective_user_id": 1000, 
        "ansible_enp3s0": {
            "active": false, 
            "device": "enp3s0", 
            "features": {
                "busy_poll": "off [fixed]", 
                "fcoe_mtu": "off [fixed]", 
                "generic_receive_offload": "on", 
                "generic_segmentation_offload": "off [requested on]", 
                "highdma": "on [fixed]", 
                "hw_tc_offload": "off [fixed]", 
                "l2_fwd_offload": "off [fixed]", 
                "large_receive_offload": "off [fixed]", 
                "loopback": "off [fixed]", 
                "netns_local": "off [fixed]", 
                "ntuple_filters": "off [fixed]", 
                "receive_hashing": "off [fixed]", 
                "rx_all": "off", 
                "rx_checksumming": "on", 
                "rx_fcs": "off", 
                "rx_vlan_filter": "off [fixed]", 
                "rx_vlan_offload": "on", 
                "rx_vlan_stag_filter": "off [fixed]", 
                "rx_vlan_stag_hw_parse": "off [fixed]", 
                "scatter_gather": "off", 
                "tcp_segmentation_offload": "off", 
                "tx_checksum_fcoe_crc": "off [fixed]", 
                "tx_checksum_ip_generic": "off [fixed]", 
                "tx_checksum_ipv4": "off", 
                "tx_checksum_ipv6": "off", 
                "tx_checksum_sctp": "off [fixed]", 
                "tx_checksumming": "off", 
                "tx_fcoe_segmentation": "off [fixed]", 
                "tx_gre_csum_segmentation": "off [fixed]", 
                "tx_gre_segmentation": "off [fixed]", 
                "tx_gso_partial": "off [fixed]", 
                "tx_gso_robust": "off [fixed]", 
                "tx_ipxip4_segmentation": "off [fixed]", 
                "tx_ipxip6_segmentation": "off [fixed]", 
                "tx_lockless": "off [fixed]", 
                "tx_nocache_copy": "off", 
                "tx_scatter_gather": "off", 
                "tx_scatter_gather_fraglist": "off [fixed]", 
                "tx_sctp_segmentation": "off [fixed]", 
                "tx_tcp6_segmentation": "off", 
                "tx_tcp_ecn_segmentation": "off [fixed]", 
                "tx_tcp_mangleid_segmentation": "off", 
                "tx_tcp_segmentation": "off", 
                "tx_udp_tnl_csum_segmentation": "off [fixed]", 
                "tx_udp_tnl_segmentation": "off [fixed]", 
                "tx_vlan_offload": "on", 
                "tx_vlan_stag_hw_insert": "off [fixed]", 
                "udp_fragmentation_offload": "off [fixed]", 
                "vlan_challenged": "off [fixed]"
            }, 
            "macaddress": "5c:f9:dd:4e:4c:1d", 
            "module": "r8169", 
            "mtu": 1500, 
            "pciid": "0000:03:00.0", 
            "promisc": false, 
            "speed": 10, 
            "type": "ether"
        }, 
        "ansible_env": {
            "CLUTTER_IM_MODULE": "xim", 
            "COLORTERM": "gnome-terminal", 
            "COMPIZ_BIN_PATH": "/usr/bin/", 
            "COMPIZ_CONFIG_PROFILE": "ubuntu", 
            "DBUS_SESSION_BUS_ADDRESS": "unix:abstract=/tmp/dbus-DmzwpnTQha", 
            "DEFAULTS_PATH": "/usr/share/gconf/ubuntu.default.path", 
            "DERBY_HOME": "/usr/lib/jvm/java-8-oracle/db", 
            "DESKTOP_SESSION": "ubuntu", 
            "DISPLAY": ":0", 
            "GDMSESSION": "ubuntu", 
            "GDM_LANG": "en_US", 
            "GIO_LAUNCHED_DESKTOP_FILE": "/usr/share/applications/terminator.desktop", 
            "GIO_LAUNCHED_DESKTOP_FILE_PID": "4764", 
            "GNOME_DESKTOP_SESSION_ID": "this-is-deprecated", 
            "GNOME_KEYRING_CONTROL": "", 
            "GNOME_KEYRING_PID": "", 
            "GPG_AGENT_INFO": "/home/umidjons/.gnupg/S.gpg-agent:0:1", 
            "GTK2_MODULES": "overlay-scrollbar", 
            "GTK_IM_MODULE": "ibus", 
            "GTK_MODULES": "gail:atk-bridge:unity-gtk-module", 
            "HOME": "/home/umidjons", 
            "IBUS_DISABLE_SNOOPER": "1", 
            "IM_CONFIG_PHASE": "1", 
            "INSTANCE": "", 
            "J2REDIR": "/usr/lib/jvm/java-8-oracle/jre", 
            "J2SDKDIR": "/usr/lib/jvm/java-8-oracle", 
            "JAVA_HOME": "/usr/lib/jvm/java-8-oracle", 
            "JOB": "unity-settings-daemon", 
            "LANG": "en_US.UTF-8", 
            "LANGUAGE": "en_US", 
            "LC_ADDRESS": "en_US.UTF-8", 
            "LC_IDENTIFICATION": "en_US.UTF-8", 
            "LC_MEASUREMENT": "en_US.UTF-8", 
            "LC_MONETARY": "en_US.UTF-8", 
            "LC_NAME": "en_US.UTF-8", 
            "LC_NUMERIC": "en_US.UTF-8", 
            "LC_PAPER": "en_US.UTF-8", 
            "LC_TELEPHONE": "en_US.UTF-8", 
            "LC_TIME": "en_US.UTF-8", 
            "LESSCLOSE": "/usr/bin/lesspipe %s %s", 
            "LESSOPEN": "| /usr/bin/lesspipe %s", 
            "LOGNAME": "umidjons", 
            "LS_COLORS": "rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.Z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.jpg=01;35:*.jpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:", 
            "MANDATORY_PATH": "/usr/share/gconf/ubuntu.mandatory.path", 
            "NVM_BIN": "/home/umidjons/.nvm/versions/node/v7.10.0/bin", 
            "NVM_CD_FLAGS": "", 
            "NVM_DIR": "/home/umidjons/.nvm", 
            "OLDPWD": "/home/umidjons/projects/tests", 
            "ORBIT_SOCKETDIR": "/tmp/orbit-umidjons", 
            "PAPERSIZE": "letter", 
            "PATH": "/home/umidjons/.nvm/versions/node/v7.10.0/bin:/home/umidjons/bin:/home/umidjons/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/usr/lib/jvm/java-8-oracle/bin:/usr/lib/jvm/java-8-oracle/db/bin:/usr/lib/jvm/java-8-oracle/jre/bin", 
            "PWD": "/home/umidjons/projects/tests/t5", 
            "QT4_IM_MODULE": "xim", 
            "QT_ACCESSIBILITY": "1", 
            "QT_IM_MODULE": "ibus", 
            "QT_LINUX_ACCESSIBILITY_ALWAYS_ON": "1", 
            "QT_QPA_PLATFORMTHEME": "appmenu-qt5", 
            "SESSION": "ubuntu", 
            "SESSIONTYPE": "gnome-session", 
            "SESSION_MANAGER": "local/uzbumid:@/tmp/.ICE-unix/2934,unix/uzbumid:/tmp/.ICE-unix/2934", 
            "SHELL": "/bin/bash", 
            "SHLVL": "1", 
            "SSH_AUTH_SOCK": "/run/user/1000/keyring/ssh", 
            "TERM": "xterm", 
            "TERMINATOR_UUID": "urn:uuid:03b7c79c-acdc-4348-aef5-b1a97120480c", 
            "UPSTART_EVENTS": "xsession started", 
            "UPSTART_INSTANCE": "", 
            "UPSTART_JOB": "unity7", 
            "UPSTART_SESSION": "unix:abstract=/com/ubuntu/upstart-session/1000/2602", 
            "USER": "umidjons", 
            "WINDOWID": "75497476", 
            "XAUTHORITY": "/home/umidjons/.Xauthority", 
            "XDG_CONFIG_DIRS": "/etc/xdg/xdg-ubuntu:/usr/share/upstart/xdg:/etc/xdg", 
            "XDG_CURRENT_DESKTOP": "Unity", 
            "XDG_DATA_DIRS": "/usr/share/ubuntu:/usr/share/gnome:/usr/local/share/:/usr/share/:/var/lib/snapd/desktop", 
            "XDG_GREETER_DATA_DIR": "/var/lib/lightdm-data/umidjons", 
            "XDG_MENU_PREFIX": "gnome-", 
            "XDG_RUNTIME_DIR": "/run/user/1000", 
            "XDG_SEAT": "seat0", 
            "XDG_SEAT_PATH": "/org/freedesktop/DisplayManager/Seat0", 
            "XDG_SESSION_DESKTOP": "ubuntu", 
            "XDG_SESSION_ID": "c2", 
            "XDG_SESSION_PATH": "/org/freedesktop/DisplayManager/Session0", 
            "XDG_SESSION_TYPE": "x11", 
            "XDG_VTNR": "7", 
            "XMODIFIERS": "@im=ibus", 
            "_": "/usr/bin/ansible"
        }, 
        "ansible_fips": false, 
        "ansible_form_factor": "Portable", 
        "ansible_fqdn": "uzbumid", 
        "ansible_gather_subset": [
            "hardware", 
            "network", 
            "virtual"
        ], 
        "ansible_hostname": "uzbumid", 
        "ansible_interfaces": [
            "docker0", 
            "wlp2s0", 
            "lo", 
            "tun0", 
            "enp3s0", 
            "br-7900c128755b"
        ], 
        "ansible_kernel": "4.8.0-56-generic", 
        "ansible_lo": {
            "active": true, 
            "device": "lo", 
            "features": {
                "busy_poll": "off [fixed]", 
                "fcoe_mtu": "off [fixed]", 
                "generic_receive_offload": "on", 
                "generic_segmentation_offload": "on", 
                "highdma": "on [fixed]", 
                "hw_tc_offload": "off [fixed]", 
                "l2_fwd_offload": "off [fixed]", 
                "large_receive_offload": "off [fixed]", 
                "loopback": "on [fixed]", 
                "netns_local": "on [fixed]", 
                "ntuple_filters": "off [fixed]", 
                "receive_hashing": "off [fixed]", 
                "rx_all": "off [fixed]", 
                "rx_checksumming": "on [fixed]", 
                "rx_fcs": "off [fixed]", 
                "rx_vlan_filter": "off [fixed]", 
                "rx_vlan_offload": "off [fixed]", 
                "rx_vlan_stag_filter": "off [fixed]", 
                "rx_vlan_stag_hw_parse": "off [fixed]", 
                "scatter_gather": "on", 
                "tcp_segmentation_offload": "on", 
                "tx_checksum_fcoe_crc": "off [fixed]", 
                "tx_checksum_ip_generic": "on [fixed]", 
                "tx_checksum_ipv4": "off [fixed]", 
                "tx_checksum_ipv6": "off [fixed]", 
                "tx_checksum_sctp": "on [fixed]", 
                "tx_checksumming": "on", 
                "tx_fcoe_segmentation": "off [fixed]", 
                "tx_gre_csum_segmentation": "off [fixed]", 
                "tx_gre_segmentation": "off [fixed]", 
                "tx_gso_partial": "off [fixed]", 
                "tx_gso_robust": "off [fixed]", 
                "tx_ipxip4_segmentation": "off [fixed]", 
                "tx_ipxip6_segmentation": "off [fixed]", 
                "tx_lockless": "on [fixed]", 
                "tx_nocache_copy": "off [fixed]", 
                "tx_scatter_gather": "on [fixed]", 
                "tx_scatter_gather_fraglist": "on [fixed]", 
                "tx_sctp_segmentation": "on", 
                "tx_tcp6_segmentation": "on", 
                "tx_tcp_ecn_segmentation": "on", 
                "tx_tcp_mangleid_segmentation": "on", 
                "tx_tcp_segmentation": "on", 
                "tx_udp_tnl_csum_segmentation": "off [fixed]", 
                "tx_udp_tnl_segmentation": "off [fixed]", 
                "tx_vlan_offload": "off [fixed]", 
                "tx_vlan_stag_hw_insert": "off [fixed]", 
                "udp_fragmentation_offload": "on", 
                "vlan_challenged": "on [fixed]"
            }, 
            "ipv4": {
                "address": "127.0.0.1", 
                "broadcast": "host", 
                "netmask": "255.0.0.0", 
                "network": "127.0.0.0"
            }, 
            "ipv6": [
                {
                    "address": "::1", 
                    "prefix": "128", 
                    "scope": "host"
                }
            ], 
            "mtu": 65536, 
            "promisc": false, 
            "type": "loopback"
        }, 
        "ansible_lsb": {
            "codename": "xenial", 
            "description": "Ubuntu 16.04.2 LTS", 
            "id": "Ubuntu", 
            "major_release": "16", 
            "release": "16.04"
        }, 
        "ansible_machine": "x86_64", 
        "ansible_machine_id": "5f71dc4467d04291a63aaac656c86227", 
        "ansible_memfree_mb": 668, 
        "ansible_memory_mb": {
            "nocache": {
                "free": 1980, 
                "used": 3843
            }, 
            "real": {
                "free": 668, 
                "total": 5823, 
                "used": 5155
            }, 
            "swap": {
                "cached": 0, 
                "free": 15804, 
                "total": 15804, 
                "used": 0
            }
        }, 
        "ansible_memtotal_mb": 5823, 
        "ansible_mounts": [
            {
                "device": "/dev/sda8", 
                "fstype": "ext4", 
                "mount": "/", 
                "options": "rw,relatime,errors=remount-ro,data=ordered", 
                "size_available": 29536129024, 
                "size_total": 49076379648, 
                "uuid": "da13ddb6-4785-413b-bc76-9d5aebd1cf9a"
            }, 
            {
                "device": "/dev/sda6", 
                "fstype": "ext4", 
                "mount": "/home", 
                "options": "rw,relatime,data=ordered", 
                "size_available": 462130786304, 
                "size_total": 492018982912, 
                "uuid": "2ba1ca44-72fe-4522-8687-4ff13238ca96"
            }, 
            {
                "device": "/dev/sda8", 
                "fstype": "ext4", 
                "mount": "/var/lib/docker/aufs", 
                "options": "rw,relatime,errors=remount-ro,data=ordered,bind", 
                "size_available": 29536129024, 
                "size_total": 49076379648, 
                "uuid": "da13ddb6-4785-413b-bc76-9d5aebd1cf9a"
            }, 
            {
                "device": "/dev/sdb1", 
                "fstype": "fuseblk", 
                "mount": "/media/umidjons/My\\040Passport1", 
                "options": "rw,nosuid,nodev,relatime,user_id=0,group_id=0,default_permissions,allow_other,blksize=4096", 
                "size_available": null, 
                "size_total": null, 
                "uuid": "44500B85500B7D44"
            }
        ], 
        "ansible_nodename": "uzbumid", 
        "ansible_os_family": "Debian", 
        "ansible_pkg_mgr": "apt", 
        "ansible_processor": [
            "GenuineIntel", 
            "Intel(R) Core(TM) i7-3612QM CPU @ 2.10GHz", 
            "GenuineIntel", 
            "Intel(R) Core(TM) i7-3612QM CPU @ 2.10GHz", 
            "GenuineIntel", 
            "Intel(R) Core(TM) i7-3612QM CPU @ 2.10GHz", 
            "GenuineIntel", 
            "Intel(R) Core(TM) i7-3612QM CPU @ 2.10GHz", 
            "GenuineIntel", 
            "Intel(R) Core(TM) i7-3612QM CPU @ 2.10GHz", 
            "GenuineIntel", 
            "Intel(R) Core(TM) i7-3612QM CPU @ 2.10GHz", 
            "GenuineIntel", 
            "Intel(R) Core(TM) i7-3612QM CPU @ 2.10GHz", 
            "GenuineIntel", 
            "Intel(R) Core(TM) i7-3612QM CPU @ 2.10GHz"
        ], 
        "ansible_processor_cores": 4, 
        "ansible_processor_count": 1, 
        "ansible_processor_threads_per_core": 2, 
        "ansible_processor_vcpus": 8, 
        "ansible_product_name": "Inspiron 5720", 
        "ansible_product_serial": "NA", 
        "ansible_product_uuid": "NA", 
        "ansible_product_version": "NA", 
        "ansible_python": {
            "executable": "/usr/bin/python", 
            "has_sslcontext": true, 
            "type": "CPython", 
            "version": {
                "major": 2, 
                "micro": 12, 
                "minor": 7, 
                "releaselevel": "final", 
                "serial": 0
            }, 
            "version_info": [
                2, 
                7, 
                12, 
                "final", 
                0
            ]
        }, 
        "ansible_python_version": "2.7.12", 
        "ansible_real_group_id": 1000, 
        "ansible_real_user_id": 1000, 
        "ansible_selinux": false, 
        "ansible_service_mgr": "systemd", 
        "ansible_swapfree_mb": 15804, 
        "ansible_swaptotal_mb": 15804, 
        "ansible_system": "Linux", 
        "ansible_system_capabilities": [
            ""
        ], 
        "ansible_system_capabilities_enforced": "True", 
        "ansible_system_vendor": "Dell Inc.", 
        "ansible_tun0": {
            "active": true, 
            "device": "tun0", 
            "features": {
                "busy_poll": "off [fixed]", 
                "fcoe_mtu": "off [fixed]", 
                "generic_receive_offload": "on", 
                "generic_segmentation_offload": "on", 
                "highdma": "off [fixed]", 
                "hw_tc_offload": "off [fixed]", 
                "l2_fwd_offload": "off [fixed]", 
                "large_receive_offload": "off [fixed]", 
                "loopback": "off [fixed]", 
                "netns_local": "off [fixed]", 
                "ntuple_filters": "off [fixed]", 
                "receive_hashing": "off [fixed]", 
                "rx_all": "off [fixed]", 
                "rx_checksumming": "off [fixed]", 
                "rx_fcs": "off [fixed]", 
                "rx_vlan_filter": "off [fixed]", 
                "rx_vlan_offload": "off [fixed]", 
                "rx_vlan_stag_filter": "off [fixed]", 
                "rx_vlan_stag_hw_parse": "off [fixed]", 
                "scatter_gather": "on", 
                "tcp_segmentation_offload": "off", 
                "tx_checksum_fcoe_crc": "off [fixed]", 
                "tx_checksum_ip_generic": "off [requested on]", 
                "tx_checksum_ipv4": "off [fixed]", 
                "tx_checksum_ipv6": "off [fixed]", 
                "tx_checksum_sctp": "off [fixed]", 
                "tx_checksumming": "off", 
                "tx_fcoe_segmentation": "off [fixed]", 
                "tx_gre_csum_segmentation": "off [fixed]", 
                "tx_gre_segmentation": "off [fixed]", 
                "tx_gso_partial": "off [fixed]", 
                "tx_gso_robust": "off [fixed]", 
                "tx_ipxip4_segmentation": "off [fixed]", 
                "tx_ipxip6_segmentation": "off [fixed]", 
                "tx_lockless": "on [fixed]", 
                "tx_nocache_copy": "off", 
                "tx_scatter_gather": "on", 
                "tx_scatter_gather_fraglist": "on", 
                "tx_sctp_segmentation": "off [fixed]", 
                "tx_tcp6_segmentation": "off [requested on]", 
                "tx_tcp_ecn_segmentation": "off [requested on]", 
                "tx_tcp_mangleid_segmentation": "off", 
                "tx_tcp_segmentation": "off [requested on]", 
                "tx_udp_tnl_csum_segmentation": "off [fixed]", 
                "tx_udp_tnl_segmentation": "off [fixed]", 
                "tx_vlan_offload": "on", 
                "tx_vlan_stag_hw_insert": "on", 
                "udp_fragmentation_offload": "off [requested on]", 
                "vlan_challenged": "off [fixed]"
            }, 
            "ipv4": {
                "address": "44.174.79.10", 
                "broadcast": "44.174.79.255", 
                "netmask": "255.255.248.0", 
                "network": "44.174.72.0"
            }, 
            "ipv6": [
                {
                    "address": "fe80::6030:bd10:b61c:c1e", 
                    "prefix": "64", 
                    "scope": "link"
                }
            ], 
            "mtu": 1500, 
            "promisc": false, 
            "speed": 10, 
            "type": "tunnel"
        }, 
        "ansible_uptime_seconds": 2911, 
        "ansible_user_dir": "/home/umidjons", 
        "ansible_user_gecos": "umidjons,,,", 
        "ansible_user_gid": 1000, 
        "ansible_user_id": "umidjons", 
        "ansible_user_shell": "/bin/bash", 
        "ansible_user_uid": 1000, 
        "ansible_userspace_architecture": "x86_64", 
        "ansible_userspace_bits": "64", 
        "ansible_virtualization_role": "host", 
        "ansible_virtualization_type": "kvm", 
        "ansible_wlp2s0": {
            "active": true, 
            "device": "wlp2s0", 
            "features": {
                "busy_poll": "off [fixed]", 
                "fcoe_mtu": "off [fixed]", 
                "generic_receive_offload": "on", 
                "generic_segmentation_offload": "off [requested on]", 
                "highdma": "off [fixed]", 
                "hw_tc_offload": "off [fixed]", 
                "l2_fwd_offload": "off [fixed]", 
                "large_receive_offload": "off [fixed]", 
                "loopback": "off [fixed]", 
                "netns_local": "on [fixed]", 
                "ntuple_filters": "off [fixed]", 
                "receive_hashing": "off [fixed]", 
                "rx_all": "off [fixed]", 
                "rx_checksumming": "off [fixed]", 
                "rx_fcs": "off [fixed]", 
                "rx_vlan_filter": "off [fixed]", 
                "rx_vlan_offload": "off [fixed]", 
                "rx_vlan_stag_filter": "off [fixed]", 
                "rx_vlan_stag_hw_parse": "off [fixed]", 
                "scatter_gather": "off", 
                "tcp_segmentation_offload": "off", 
                "tx_checksum_fcoe_crc": "off [fixed]", 
                "tx_checksum_ip_generic": "off [fixed]", 
                "tx_checksum_ipv4": "off [fixed]", 
                "tx_checksum_ipv6": "off [fixed]", 
                "tx_checksum_sctp": "off [fixed]", 
                "tx_checksumming": "off", 
                "tx_fcoe_segmentation": "off [fixed]", 
                "tx_gre_csum_segmentation": "off [fixed]", 
                "tx_gre_segmentation": "off [fixed]", 
                "tx_gso_partial": "off [fixed]", 
                "tx_gso_robust": "off [fixed]", 
                "tx_ipxip4_segmentation": "off [fixed]", 
                "tx_ipxip6_segmentation": "off [fixed]", 
                "tx_lockless": "off [fixed]", 
                "tx_nocache_copy": "off", 
                "tx_scatter_gather": "off [fixed]", 
                "tx_scatter_gather_fraglist": "off [fixed]", 
                "tx_sctp_segmentation": "off [fixed]", 
                "tx_tcp6_segmentation": "off [fixed]", 
                "tx_tcp_ecn_segmentation": "off [fixed]", 
                "tx_tcp_mangleid_segmentation": "off [fixed]", 
                "tx_tcp_segmentation": "off [fixed]", 
                "tx_udp_tnl_csum_segmentation": "off [fixed]", 
                "tx_udp_tnl_segmentation": "off [fixed]", 
                "tx_vlan_offload": "off [fixed]", 
                "tx_vlan_stag_hw_insert": "off [fixed]", 
                "udp_fragmentation_offload": "off [fixed]", 
                "vlan_challenged": "off [fixed]"
            }, 
            "ipv4": {
                "address": "192.168.1.180", 
                "broadcast": "192.168.1.255", 
                "netmask": "255.255.255.0", 
                "network": "192.168.1.0"
            }, 
            "ipv6": [
                {
                    "address": "fe80::e206:e6ff:fed7:92ef", 
                    "prefix": "64", 
                    "scope": "link"
                }
            ], 
            "macaddress": "e0:06:e6:d7:92:ef", 
            "module": "wl", 
            "mtu": 1500, 
            "pciid": "0000:02:00.0", 
            "promisc": false, 
            "type": "ether"
        }, 
        "module_setup": true
    }, 
    "changed": false
}
```

## Fix `FAILED! => {"failed": true, "msg": "ERROR! Timeout (12s) waiting for privilege escalation prompt: "}` issue

In `ansible.cfg` set following settings:

```
[defaults]
transport=paramiko
timeout = 30
; ...
```
