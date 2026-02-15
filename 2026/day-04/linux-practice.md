     Docs: man:nginx(8)
    Process: 2022 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 2024 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
   Main PID: 2054 (nginx)
      Tasks: 3 (limit: 1008)
     Memory: 2.4M (peak: 5.3M)
        CPU: 26ms
     CGroup: /system.slice/nginx.service
             ├─2054 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
             ├─2056 "nginx: worker process"
             └─2057 "nginx: worker process"

Feb 15 19:01:56 ip-172-31-40-58 systemd[1]: Starting nginx.service - A high performance web server and a reverse proxy server...
Feb 15 19:01:56 ip-172-31-40-58 systemd[1]: Started nginx.service - A high performance web server and a reverse proxy server.
root@ip-172-31-40-58:/home/ubuntu/sample_linux# sytemctl stop nginx
Command 'sytemctl' not found, did you mean:
  command 'systemctl' from deb systemd (255.4-1ubuntu8.12)
  command 'systemctl' from deb systemctl (1.4.4181-1.1)
Try: apt install <deb name>
root@ip-172-31-40-58:/home/ubuntu/sample_linux# systemctl stop nginx
root@ip-172-31-40-58:/home/ubuntu/sample_linux# systemctl status nginx
○ nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: inactive (dead) since Sun 2026-02-15 19:05:12 UTC; 3s ago
   Duration: 3min 16.430s
       Docs: man:nginx(8)
    Process: 2022 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 2024 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 2134 ExecStop=/sbin/start-stop-daemon --quiet --stop --retry QUIT/5 --pidfile /run/nginx.pid (code=exited, status=0/SUCCESS)
   Main PID: 2054 (code=exited, status=0/SUCCESS)
        CPU: 29ms

Feb 15 19:01:56 ip-172-31-40-58 systemd[1]: Starting nginx.service - A high performance web server and a reverse proxy server...
Feb 15 19:01:56 ip-172-31-40-58 systemd[1]: Started nginx.service - A high performance web server and a reverse proxy server.
Feb 15 19:05:12 ip-172-31-40-58 systemd[1]: Stopping nginx.service - A high performance web server and a reverse proxy server...
Feb 15 19:05:12 ip-172-31-40-58 systemd[1]: nginx.service: Deactivated successfully.
Feb 15 19:05:12 ip-172-31-40-58 systemd[1]: Stopped nginx.service - A high performance web server and a reverse proxy server.
root@ip-172-31-40-58:/home/ubuntu/sample_linux# systemctl start  nginx
root@ip-172-31-40-58:/home/ubuntu/sample_linux# systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Sun 2026-02-15 19:05:32 UTC; 4s ago
       Docs: man:nginx(8)
    Process: 2145 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 2147 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
   Main PID: 2148 (nginx)
      Tasks: 3 (limit: 1008)
     Memory: 2.4M (peak: 2.7M)
        CPU: 17ms
     CGroup: /system.slice/nginx.service
             ├─2148 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
             ├─2149 "nginx: worker process"
             └─2150 "nginx: worker process"

Feb 15 19:05:32 ip-172-31-40-58 systemd[1]: Starting nginx.service - A high performance web server and a reverse proxy server...
Feb 15 19:05:32 ip-172-31-40-58 systemd[1]: Started nginx.service - A high performance web server and a reverse proxy server.
root@ip-172-31-40-58:/home/ubuntu/sample_linux# systemctl disable nginx
Synchronizing state of nginx.service with SysV service script with /usr/lib/systemd/systemd-sysv-install.
Executing: /usr/lib/systemd/systemd-sysv-install disable nginx
Removed "/etc/systemd/system/multi-user.target.wants/nginx.service".
root@ip-172-31-40-58:/home/ubuntu/sample_linux# systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; preset: enabled)
     Active: active (running) since Sun 2026-02-15 19:05:32 UTC; 40s ago
       Docs: man:nginx(8)
   Main PID: 2148 (nginx)
      Tasks: 3 (limit: 1008)
     Memory: 2.4M (peak: 2.7M)
        CPU: 17ms
     CGroup: /system.slice/nginx.service
             ├─2148 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
             ├─2149 "nginx: worker process"
             └─2150 "nginx: worker process"

Feb 15 19:05:32 ip-172-31-40-58 systemd[1]: Starting nginx.service - A high performance web server and a reverse proxy server...
Feb 15 19:05:32 ip-172-31-40-58 systemd[1]: Started nginx.service - A high performance web server and a reverse proxy server.
root@ip-172-31-40-58:/home/ubuntu/sample_linux# systemctl enable nginx
Synchronizing state of nginx.service with SysV service script with /usr/lib/systemd/systemd-sysv-install.
Executing: /usr/lib/systemd/systemd-sysv-install enable nginx
Created symlink /etc/systemd/system/multi-user.target.wants/nginx.service → /usr/lib/systemd/system/nginx.service.
root@ip-172-31-40-58:/home/ubuntu/sample_linux# systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Sun 2026-02-15 19:05:32 UTC; 1min 19s ago
       Docs: man:nginx(8)
   Main PID: 2148 (nginx)
      Tasks: 3 (limit: 1008)
     Memory: 2.4M (peak: 2.7M)
        CPU: 17ms
     CGroup: /system.slice/nginx.service
             ├─2148 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
             ├─2149 "nginx: worker process"
             └─2150 "nginx: worker process"

Feb 15 19:05:32 ip-172-31-40-58 systemd[1]: Starting nginx.service - A high performance web server and a reverse proxy server...
Feb 15 19:05:32 ip-172-31-40-58 systemd[1]: Started nginx.service - A high performance web server and a reverse proxy server.
root@ip-172-31-40-58:/home/ubuntu/sample_linux# systemctl list_units
Unknown command verb 'list_units', did you mean 'list-units'?
root@ip-172-31-40-58:/home/ubuntu/sample_linux# systemctl list-units
  UNIT                                                                         LOAD   ACTIVE SUB       DESCRIPTION                                                                  
  proc-sys-fs-binfmt_misc.automount                                            loaded active running   Arbitrary Executable File Formats File System Automount Point                
  sys-devices-pci0000:00-0000:00:04.0-nvme-nvme0-nvme0n1-nvme0n1p1.device      loaded active plugged   Amazon Elastic Block Store cloudimg-rootfs
  sys-devices-pci0000:00-0000:00:04.0-nvme-nvme0-nvme0n1-nvme0n1p14.device     loaded active plugged   Amazon Elastic Block Store 14
  sys-devices-pci0000:00-0000:00:04.0-nvme-nvme0-nvme0n1-nvme0n1p15.device     loaded active plugged   Amazon Elastic Block Store UEFI
  sys-devices-pci0000:00-0000:00:04.0-nvme-nvme0-nvme0n1-nvme0n1p16.device     loaded active plugged   Amazon Elastic Block Store BOOT
  sys-devices-pci0000:00-0000:00:04.0-nvme-nvme0-nvme0n1.device                loaded active plugged   Amazon Elastic Block Store
  sys-devices-pci0000:00-0000:00:05.0-net-ens5.device                          loaded active plugged   Elastic Network Adapter (ENA)
  sys-devices-platform-serial8250-serial8250:0-serial8250:0.1-tty-ttyS1.device loaded active plugged   /sys/devices/platform/serial8250/serial8250:0/serial8250:0.1/tty/ttyS1
  sys-devices-platform-serial8250-serial8250:0-serial8250:0.2-tty-ttyS2.device loaded active plugged   /sys/devices/platform/serial8250/serial8250:0/serial8250:0.2/tty/ttyS2
  sys-devices-platform-serial8250-serial8250:0-serial8250:0.3-tty-ttyS3.device loaded active plugged   /sys/devices/platform/serial8250/serial8250:0/serial8250:0.3/tty/ttyS3
  sys-devices-pnp0-00:04-00:04:0-00:04:0.0-tty-ttyS0.device                    loaded active plugged   /sys/devices/pnp0/00:04/00:04:0/00:04:0.0/tty/ttyS0
  sys-devices-virtual-block-loop0.device                                       loaded active plugged   /sys/devices/virtual/block/loop0
  sys-devices-virtual-block-loop1.device                                       loaded active plugged   /sys/devices/virtual/block/loop1
  sys-devices-virtual-block-loop2.device                                       loaded active plugged   /sys/devices/virtual/block/loop2
  sys-devices-virtual-misc-rfkill.device                                       loaded active plugged   /sys/devices/virtual/misc/rfkill
  sys-devices-virtual-tty-ttyprintk.device                                     loaded active plugged   /sys/devices/virtual/tty/ttyprintk
  sys-module-configfs.device                                                   loaded active plugged   /sys/module/configfs
  sys-module-fuse.device                                                       loaded active plugged   /sys/module/fuse
  sys-subsystem-net-devices-ens5.device                                        loaded active plugged   Elastic Network Adapter (ENA)                                                
  -.mount                                                                      loaded active mounted   Root Mount
  boot-efi.mount                                                               loaded active mounted   /boot/efi
  boot.mount                                                                   loaded active mounted   /boot
  dev-hugepages.mount                                                          loaded active mounted   Huge Pages File System
  dev-mqueue.mount                                                             loaded active mounted   POSIX Message Queue File System
  proc-sys-fs-binfmt_misc.mount                                                loaded active mounted   Arbitrary Executable File Formats File System
  run-user-1000.mount                                                          loaded active mounted   /run/user/1000
  snap-amazon\x2dssm\x2dagent-11797.mount                                      loaded active mounted   Mount unit for amazon-ssm-agent, revision 11797
  snap-core22-2163.mount                                                       loaded active mounted   Mount unit for core22, revision 2163
  snap-snapd-25577.mount                                                       loaded active mounted   Mount unit for snapd, revision 25577
  sys-fs-fuse-connections.mount                                                loaded active mounted   FUSE Control File System
  sys-kernel-config.mount                                                      loaded active mounted   Kernel Configuration File System
  sys-kernel-debug.mount                                                       loaded active mounted   Kernel Debug File System
  sys-kernel-tracing.mount                                                     loaded active mounted   Kernel Trace File System                                                     
  acpid.path                                                                   loaded active running   ACPI Events Check
  systemd-ask-password-console.path                                            loaded active waiting   Dispatch Password Requests to Console Directory Watch
  systemd-ask-password-wall.path                                               loaded active waiting   Forward Password Requests to Wall Directory Watch                            
  init.scope                                                                   loaded active running   System and Service Manager
  session-1.scope                                                              loaded active running   Session 1 of User ubuntu                                                     
  acpid.service                                                                loaded active running   ACPI event daemon
  apparmor.service                                                             loaded active exited    Load AppArmor profiles
  apport.service                                                               loaded active exited    automatic crash report generation
  blk-availability.service                                                     loaded active exited    Availability of block devices
  chrony.service                                                               loaded active running   chrony, an NTP client/server
  cloud-config.service                                                         loaded active exited    Cloud-init: Config Stage
  cloud-final.service                                                          loaded active exited    Cloud-init: Final Stage
  cloud-init-local.service                                                     loaded active exited    Cloud-init: Local Stage (pre-network)
  cloud-init.service                                                           loaded active exited    Cloud-init: Network Stage
  console-setup.service                                                        loaded active exited    Set console font and keymap
  cron.service                                                                 loaded active running   Regular background program processing daemon
  dbus.service                                                                 loaded active running   D-Bus System Message Bus
  finalrd.service                                                              loaded active exited    Create final runtime dir for shutdown pivot root
  getty@tty1.service                                                           loaded active running   Getty on tty1
  irqbalance.service                                                           loaded active running   irqbalance daemon
  keyboard-setup.service                                                       loaded active exited    Set the console keyboard layout
  kmod-static-nodes.service                                                    loaded active exited    Create List of Static Device Nodes
  ldconfig.service                                                             loaded active exited    Rebuild Dynamic Linker Cache
  lvm2-monitor.service                                                         loaded active exited    Monitoring of LVM2 mirrors, snapshots etc. using dmeventd or progress polling
  ModemManager.service                                                         loaded active running   Modem Manager
  multipathd.service                                                           loaded active running   Device-Mapper Multipath Device Controller
  networkd-dispatcher.service                                                  loaded active running   Dispatcher daemon for systemd-networkd
  nginx.service                                                                loaded active running   A high performance web server and a reverse proxy server
  plymouth-quit-wait.service                                                   loaded active exited    Hold until boot process finishes up
  plymouth-quit.service                                                        loaded active exited    Terminate Plymouth Boot Screen
  plymouth-read-write.service                                                  loaded active exited    Tell Plymouth To Write Out Runtime Data
  polkit.service                                                               loaded active running   Authorization Manager
  rsyslog.service                                                              loaded active running   System Logging Service
  serial-getty@ttyS0.service                                                   loaded active running   Serial Getty on ttyS0
  setvtrgb.service                                                             loaded active exited    Set console scheme
  snap.amazon-ssm-agent.amazon-ssm-agent.service                               loaded active running   Service for snap application amazon-ssm-agent.amazon-ssm-agent
  snapd.apparmor.service                                                       loaded active exited    Load AppArmor profiles managed internally by snapd
  snapd.seeded.service                                                         loaded active exited    Wait until snapd is fully seeded

root@ip-172-31-40-58:/home/ubuntu/sample_linux# journalctl -u nginx
Feb 15 19:01:56 ip-172-31-40-58 systemd[1]: Starting nginx.service - A high performance web server and a reverse proxy server...
Feb 15 19:01:56 ip-172-31-40-58 systemd[1]: Started nginx.service - A high performance web server and a reverse proxy server.
Feb 15 19:05:12 ip-172-31-40-58 systemd[1]: Stopping nginx.service - A high performance web server and a reverse proxy server...
Feb 15 19:05:12 ip-172-31-40-58 systemd[1]: nginx.service: Deactivated successfully.
Feb 15 19:05:12 ip-172-31-40-58 systemd[1]: Stopped nginx.service - A high performance web server and a reverse proxy server.
Feb 15 19:05:32 ip-172-31-40-58 systemd[1]: Starting nginx.service - A high performance web server and a reverse proxy server...
Feb 15 19:05:32 ip-172-31-40-58 systemd[1]: Started nginx.service - A high performance web server and a reverse proxy server.
root@ip-172-31-40-58:/home/ubuntu/sample_linux# 
