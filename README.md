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
