# Boot2Docker running on xhyve hypervisor

## Features

- boot2docker v18.01.0-ce
- Disable TLS
- Expose the official IANA registered Docker port 2375
- Support NFS synced folder: /Users is NFS-mounted on the boot2docker VM.

## Requirements

- [xhyve](https://github.com/mist64/xhyve)
  - Mac OS X Yosemite 10.10.3 or later
  - A 2010 or later Mac (i.e. a CPU that supports EPT)

## Caution

- **Kernel Panic** will occur on booting, if VirtualBox (< v5.0) has run before.
- Pay attention to **exposing the port 2375 without TLS**, as you see the features.

## Installing xhyve

```
$ git clone https://github.com/mist64/xhyve
$ cd xhyve
$ make
$ cp build/xhyve /usr/local/bin/    # You may require sudo
```

or

```
$ brew install xhyve
```

## Setting up Boot2Docker images and tools

```
$ git clone https://github.com/ailispaw/boot2docker-xhyve
$ cd boot2docker-xhyve
$ make
```

## Booting Up

```
$ sudo ./xhyverun.sh

Core Linux
boot2docker login: 
```

or

```
$ make run    # You may be asked for your sudo password
Booting up...
```

- On Terminal.app: This will open a new window, then you will see in the window as below.
- On iTerm.app: This will split the current window, then you will see in the bottom pane as below.

```
Core Linux
boot2docker login: 
```

## Logging In

- ID: docker
- Password: tcuser (in most instances you will not be prompted for a password)

```
$ make ssh
docker@192.168.64.3's password:
                        ##         .
                  ## ## ##        ==
               ## ## ## ## ##    ===
           /"""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~~ ~ /  ===- ~~~
           \______ o           __/
             \    \         __/
              \____\_______/
 _                 _   ____     _            _
| |__   ___   ___ | |_|___ \ __| | ___   ___| | _____ _ __
| '_ \ / _ \ / _ \| __| __) / _` |/ _ \ / __| |/ / _ \ '__|
| |_) | (_) | (_) | |_ / __/ (_| | (_) | (__|   <  __/ |
|_.__/ \___/ \___/ \__|_____\__,_|\___/ \___|_|\_\___|_|
Boot2Docker version 18.02.0-ce, build HEAD : 99245f4 - Thu Feb  8 17:43:39 UTC 2018
Docker version 18.02.0-ce, build fc4de44
docker@boot2docker:~$ 
```

## Shutting Down

Use `halt` command to shut down in the VM:

```
docker@boot2docker:~$ sudo halt
docker@boot2docker:~$ reboot: System halted
$ 
```

or, use `make halt` on the host:

```
$ make halt
docker@192.168.64.3's password:
Shutting down...
```

## Using Docker

You can simply run Docker within the VM. However, if you install the Docker client on the host, you can use Docker commands natively on the host Mac. Install the Docker client as follows:

```
$ curl -L https://get.docker.com/builds/Darwin/x86_64/docker-latest -o docker
$ chmod +x docker
$ mv docker /usr/local/bin/    # You may require sudo
```

Alternatively install with Homebrew:

```
$ brew install docker
```

Then, in the VM, or on the host if you have installed the Docker client:

```
$ make env
export DOCKER_HOST=tcp://192.168.64.3:2375;
unset DOCKER_CERT_PATH;
unset DOCKER_TLS_VERIFY;
$ eval $(make env)

$ docker info
Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 0
Server Version: 18.02.0-ce
Storage Driver: overlay2
 Backing Filesystem: extfs
 Supports d_type: true
 Native Overlay Diff: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins:
 Volume: local
 Network: bridge host macvlan null overlay
 Log: awslogs fluentd gcplogs gelf journald json-file logentries splunk syslog
Swarm: inactive
Runtimes: runc
Default Runtime: runc
Init Binary: docker-init
containerd version: 9b55aab90508bd389d7654c4baf173a981477d55
runc version: 9f9c96235cc97674e935002fc3d78361b696a69e
init version: 949e6fa
Security Options:
 seccomp
  Profile: default
Kernel Version: 4.4.115-boot2docker
Operating System: Boot2Docker 18.02.0-ce (TCL 8.2.1); HEAD : 99245f4 - Thu Feb  8 17:43:39 UTC 2018
OSType: linux
Architecture: x86_64
CPUs: 1
Total Memory: 995.9MiB
Name: boot2docker
ID: TPTK:6Y5H:LB5S:ZSYT:QCPS:5YBN:NJK4:4SXF:QIUZ:HGEB:DALP:3FXE
Docker Root Dir: /mnt/vda1/var/lib/docker
Debug Mode (client): false
Debug Mode (server): false
Registry: https://index.docker.io/v1/
Labels:
Experimental: false
Insecure Registries:
 127.0.0.0/8
Live Restore Enabled: false
```

## Upgrading Boot2Docker

When Boot2Docker is upgraded and boot2docker-xhyve is updated,

```
$ git pull origin master
$ make upgrade
```

## Resources

- /var/db/dhcpd_leases
- /Library/Preferences/SystemConfiguration/com.apple.vmnet.plist
  - Shared_Net_Address
  - Shared_Net_Mask
