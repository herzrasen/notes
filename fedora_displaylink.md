# Installing displaylink on Fedora 35

## Install dependencies

```shell
sudo dnf install make rpm-build gcc-c++ libdrm-devel
```

## Clone displaylink-rpm

```
git clone https://github.com/displaylink-rpm/displaylink-rpm.git
```

## Install displaylink-rpm

```shell
cd displaylink-rpm
make srpm
```

This creates rpms in `i386` and `x86_64`.

## Install the rpm

```shell
cd x86_64
sudo dnf install ./displaylink-1.9.1-2.x86_64.rpm
```

Starting the systemd service fails with: 
> Dec 22 22:56:52 colors modprobe[57987]: modprobe: FATAL: Module evdi not found in directory /lib/modules/5.15.10-200.fc35>

## Install patched evdi driver

```shell
git clone https://github.com/nilathedragon/evdi
```

## Copy evdi sources to dkms directory

```shell
cd evdi
sudo cp -r module/* /var/lib/dkms/evdi/1.9.1/source/
````

## Install evdi

```shell
sudo dkms autoinstall
```
