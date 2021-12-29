# A collection of Linux related stuff I do in situations

## Mouse cursor freezes after waking up from being suspended

### Create a script in `/usr/local/bin/psmouse-fix`

```bash
#!/bin/bash

/sbin/modprobe -r psmouse
/sbin/modprobe psmouse
```

Make it executable

### Create a systemd service in `/etc/systemd/system/psmouse_fix.service`

```systemd
[Unit]
Description=Fix freezing psmouse
After=suspend.target hibernate.target hybrid-sleep.target suspend-then-hibernate.target

[Service]
ExecStart=/usr/local/bin/psmouse-fix

[Install]
WantedBy=suspend.target hibernate.target hybrid-sleep.target suspend-then-hibernate.target
```

### Enable the service

```bash
systemctl enable psmouse_fix
```

## Installing displaylink on Fedora 35

### Install dependencies

```bash
sudo dnf install make rpm-build gcc-c++ libdrm-devel
```

### Clone displaylink-rpm

```
git clone https://github.com/displaylink-rpm/displaylink-rpm.git
```

### Install displaylink-rpm

```bash
cd displaylink-rpm
make srpm
```

This creates rpms in `i386` and `x86_64`.

### Install the rpm

```bash
cd x86_64
sudo dnf install ./displaylink-1.9.1-2.x86_64.rpm
```

Starting the systemd service fails with: 
> Dec 22 22:56:52 colors modprobe[57987]: modprobe: FATAL: Module evdi not found in directory /lib/modules/5.15.10-200.fc35>

### Install patched evdi driver

```bash
git clone https://github.com/nilathedragon/evdi
```

### Copy evdi sources to dkms directory

```bash
cd evdi
sudo cp -r module/* /var/lib/dkms/evdi/1.9.1/source/
```

### Install evdi

```bash
sudo dkms autoinstall
```
