# Home Assistant Addons

- [Deepstack](https://github.com/johnolafenwa/DeepStack)
- [CompreFace](https://github.com/exadel-inc/CompreFace)
- [Frigate]()
- [Double-Take]()
- [Samba]()

## LXC Template

Template for home assistant lxc addons

### Resources

Resource|Size
---|---
Template | ubuntu-22.04-standard_22.04-1_amd64.tar.zst
Unpriviliged | No
Disk | 2G
Memory | 512Mb

### Install

Install packages
```
apt update
apt install curl git zsh
```

Install docker 
```
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

Install docker compose
```
apt install docker-compose
```

Install oh-my-zsh
```
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### Create Template

Stop container and backup as gzip

Run in pve
```
mv /mnt/<shared>/dump/<backup>.tar.gz /var/lib/vz/template/cache/ubuntu-22.04-standard_22.04-1_amd64-custom.tar.gz
```

## LXC Deepstack

Resource|Size
---|---
Template | ubuntu-22.04-standard_22.04-1_amd64-custom.tar.gz
Unpriviliged | No
Disk | 8G
Memory | 2G

## LXC Compreface

Resource|Size
---|---
Template | ubuntu-22.04-standard_22.04-1_amd64-custom.tar.gz
Unpriviliged | No
Disk | 
Memory | 

## LXC NVR

Resource|Size
---|---
Template | ubuntu-22.04-standard_22.04-1_amd64-custom.tar.gz
Unpriviliged | No
Disk | 
Memory |

### Configure

Check usb devices
```
lsusb
```

Add to pve lxc conf (/etc/pve/lxc/<ctid>.conf)
```
lxc.mount.entry: /dev/bus/usb/002/ dev/bus/usb/002/ none bind,optional,create=dir 0,0
lxc.cgroup2.devices.allow: c 189:* rwm
lxc.apparmor.profile: unconfined
lxc.cgroup2.devices.allow: a
lxc.cap.drop: 
lxc.mount.auto: cgroup:rw
```
  
Share drive with lxc, run in pve
```
pct set <ctid> -mp0 /mnt/ext1/share,mp=/share
```

Restart lxc

### Install

Get addons compose
```
git clone https://github.com/eyalcha/home-assistant-addons.git
cd home-assistant-addons
```

Start addon
```
docker-compose --profile nvr up -d
```

## LXC Calendar

### Resources
  
Resource|Size
---|---
Template | ubuntu-22.04-standard_22.04-1_amd64-custom.tar.gz
Disk | 2G
Memory | 512Mb
  
### Configure

Mount share to container, run in pve
```
pct set <ctid> -mp0 /mnt/ext1/share,mp=/share
```

Remove security profile, add to pve lxc conf (/etc/pve/lxc/<ctid>.conf)
```
lxc.apparmor.profile: unconfined
```

### Install

Get addons compose
```
git clone https://github.com/eyalcha/home-assistant-addons.git
```

Create addon folder (see https://github.com/ckulka/baikal-docker/blob/master/examples/docker-compose.localvolumes.yaml)
```
cd /share/addon
mkdir -p config data/db
chown -R 33:33 config data
cd -
```
  
Start addon
```
docker-compose --profile calendar up -d
```
  
## LXC Samaba

### Resources
  
Resource|Size
---|---
Template | ubuntu-22.04-standard_22.04-1_amd64-custom.tar.gz
Disk | 2G
Memory | 512Mb
  
### Configure

Mount share to container, run in pve
```
pct set <ctid> -mp0 /mnt/ext1/share,mp=/share
```

Remove security profile, add to pve lxc conf (/etc/pve/lxc/\<ctid\>.conf)
```
lxc.apparmor.profile: unconfined
```

Restart container
```
pct reboot <ctid>
```
