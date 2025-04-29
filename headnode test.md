[ark@localhost ~]$ su -
암호:
[root@localhost ~]# sudo systemctl restart slurmctl
Failed to restart slurmctl.service: Unit slurmctl.service not found.

[root@localhost ~]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: enp0s31f6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 54:bf:64:6d:2e:6b brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.44/24 brd 192.168.0.255 scope global dynamic noprefixroute enp0s31f6
       valid_lft 5167sec preferred_lft 5167sec
    inet6 fe80::56bf:64ff:fe6d:2e6b/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
[root@localhost ~]# sudo systemctl restart slrumctld
Failed to restart slrumctld.service: Unit slrumctld.service not found.
[root@localhost ~]# sudo systemctl restart slurmctld
[root@localhost ~]# ping 192.168.0.37
PING 192.168.0.37 (192.168.0.37) 56(84) bytes of data.
64 bytes from 192.168.0.37: icmp_seq=1 ttl=64 time=0.611 ms
64 bytes from 192.168.0.37: icmp_seq=2 ttl=64 time=0.302 ms
^C
--- 192.168.0.37 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1007ms
rtt min/avg/max/mdev = 0.302/0.456/0.611/0.154 ms
[root@localhost ~]# sbatch test_job.sh
sbatch: error: Batch job submission failed: Invalid argument
[root@localhost ~]# nano test_job.sh

[root@localhost ~]# sudo systemctl start slurmd
[root@localhost ~]# sudo systemctl enable slurmd
s[root@localhost ~]# sudo systemctl stat
Unknown command verb stat.
[root@localhost ~]# sudo systemctl status slurmd
● slurmd.service - Slurm node daemon
     Loaded: loaded (/usr/lib/systemd/system/slurmd.service; enabled; preset: d>
     Active: active (running) since Tue 2025-04-29 09:05:22 KST; 4h 40min ago
   Main PID: 2300 (slurmd)
      Tasks: 2
     Memory: 4.5M
        CPU: 452ms
     CGroup: /system.slice/slurmd.service
             └─2300 /usr/sbin/slurmd -D -s

 4월 29 13:27:47 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:29:55 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:32:03 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:34:11 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:36:19 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:38:27 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:40:35 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:42:43 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:44:51 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:45:16 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
...skipping...
● slurmd.service - Slurm node daemon
     Loaded: loaded (/usr/lib/systemd/system/slurmd.service; enabled; preset: d>
     Active: active (running) since Tue 2025-04-29 09:05:22 KST; 4h 40min ago
   Main PID: 2300 (slurmd)
      Tasks: 2
     Memory: 4.5M
        CPU: 452ms
     CGroup: /system.slice/slurmd.service
             └─2300 /usr/sbin/slurmd -D -s

 4월 29 13:27:47 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:29:55 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:32:03 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:34:11 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:36:19 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:38:27 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:40:35 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:42:43 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:44:51 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
 4월 29 13:45:16 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_req:>
~
~
~
[root@localhost ~]# sudo systemctl status slurm
Unit slurm.service could not be found.
[root@localhost ~]# sudo systemctl status slurmctl
Unit slurmctl.service could not be found.
[root@localhost ~]# sudo systemctl status slurmctld
× slurmctld.service - Slurm controller daemon
     Loaded: loaded (/usr/lib/systemd/system/slurmctld.service; enabled; preset>
     Active: failed (Result: exit-code) since Tue 2025-04-29 13:44:12 KST; 2min>
   Duration: 98ms
   Main PID: 6105 (code=exited, status=1/FAILURE)
        CPU: 49ms

 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: slurmctld ve>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No memory en>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: _sync_nodes_>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Recovered st>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: read_slurm_c>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Running as p>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No parameter>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: mcs: MCSPara>
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: Main proc>
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: Failed wi>
lines 1-17/17 (END)...skipping...
× slurmctld.service - Slurm controller daemon
     Loaded: loaded (/usr/lib/systemd/system/slurmctld.service; enabled; preset: disabled)
     Active: failed (Result: exit-code) since Tue 2025-04-29 13:44:12 KST; 2min 8s ago
   Duration: 98ms
   Main PID: 6105 (code=exited, status=1/FAILURE)
        CPU: 49ms

 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: slurmctld version 22.05.9 started on cluster cluster
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No memory enforcing mechanism configured.
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: _sync_nodes_to_comp_job: completing 1 jobs
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Recovered state of 0 reservations
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: read_slurm_conf: backup_controller not specified
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Running as primary controller
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No parameter for mcs plugin, default values set
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: mcs: MCSParameters = (null). ondemand set.
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: Main process exited, code=exited, status=1/FAILURE
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: Failed with result 'exit-code'.
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
lines 1-17/17 (END)

















































× slurmctld.service - Slurm controller daemon
     Loaded: loaded (/usr/lib/systemd/system/slurmctld.service; enabled; preset: disabled)
     Active: failed (Result: exit-code) since Tue 2025-04-29 13:44:12 KST; 2min 8s ago
   Duration: 98ms
   Main PID: 6105 (code=exited, status=1/FAILURE)
        CPU: 49ms

 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: slurmctld version 22.05.9 started on cluster cluster
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No memory enforcing mechanism configured.
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: _sync_nodes_to_comp_job: completing 1 jobs
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Recovered state of 0 reservations
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: read_slurm_conf: backup_controller not specified
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Running as primary controller
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No parameter for mcs plugin, default values set
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: mcs: MCSParameters = (null). ondemand set.
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: Main process exited, code=exited, status=1/FAILURE
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: Failed with result 'exit-code'.
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
lines 1-17/17 (END)























× slurmctld.service - Slurm controller daemon
     Loaded: loaded (/usr/lib/systemd/system/slurmctld.service; enabled;>
     Active: failed (Result: exit-code) since Tue 2025-04-29 13:44:12 KS>
   Duration: 98ms
   Main PID: 6105 (code=exited, status=1/FAILURE)
        CPU: 49ms

 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: slurm>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No me>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: _sync>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Recov>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: read_>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Runni>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No pa>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: mcs: >
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: Ma>
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: Fa>
~
~
~
lines 1-17/17 (END)

























× slurmctld.service - Slurm controller daemon
     Loaded: loaded (/usr/lib/systemd/system/slurmctld.service; enabled; prese>
     Active: failed (Result: exit-code) since Tue 2025-04-29 13:44:12 KST; 2mi>
   Duration: 98ms
   Main PID: 6105 (code=exited, status=1/FAILURE)
        CPU: 49ms

 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: slurmctld v>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No memory e>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: _sync_nodes>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Recovered s>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: read_slurm_>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Running as >
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No paramete>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: mcs: MCSPar>
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: Main pro>
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: Failed w>
~
~
~
~
~
lines 1-17/17 (END)

















































× slurmctld.service - Slurm controller daemon
     Loaded: loaded (/usr/lib/systemd/system/slurmctld.service; enabled; preset: disabled)
     Active: failed (Result: exit-code) since Tue 2025-04-29 13:44:12 KST; 2min 8s ago
   Duration: 98ms
   Main PID: 6105 (code=exited, status=1/FAILURE)
        CPU: 49ms

 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: slurmctld version 22.05.9 started on cluster cluster
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No memory enforcing mechanism configured.
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: _sync_nodes_to_comp_job: completing 1 jobs
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Recovered state of 0 reservations
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: read_slurm_conf: backup_controller not specified
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Running as primary controller
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No parameter for mcs plugin, default values set
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: mcs: MCSParameters = (null). ondemand set.
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: Main process exited, code=exited, status=1/FAILURE
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: Failed with result 'exit-code'.
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
lines 1-17/17 (END)

















































× slurmctld.service - Slurm controller daemon
     Loaded: loaded (/usr/lib/systemd/system/slurmctld.service; enabled; preset: disabled)
     Active: failed (Result: exit-code) since Tue 2025-04-29 13:44:12 KST; 2min 8s ago
   Duration: 98ms
   Main PID: 6105 (code=exited, status=1/FAILURE)
        CPU: 49ms

 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: slurmctld version 22.05.9 started on cluster cluster
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No memory enforcing mechanism configured.
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: _sync_nodes_to_comp_job: completing 1 jobs
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Recovered state of 0 reservations
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: read_slurm_conf: backup_controller not specified
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Running as primary controller
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No parameter for mcs plugin, default values set
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: mcs: MCSParameters = (null). ondemand set.
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: Main process exited, code=exited, status=1/FAILURE
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: Failed with result 'exit-code'.
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
lines 1-17/17 (END)






















× slurmctld.service - Slurm controller daemon
     Loaded: loaded (/usr/lib/systemd/system/slurmctld.service; enabled>
     Active: failed (Result: exit-code) since Tue 2025-04-29 13:44:12 K>
   Duration: 98ms
   Main PID: 6105 (code=exited, status=1/FAILURE)
        CPU: 49ms

 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: slur>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No m>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: _syn>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Reco>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: read>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Runn>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No p>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: mcs:>
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: M>
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: F>
~
~
lines 1-17/17 (END)
























× slurmctld.service - Slurm controller daemon
     Loaded: loaded (/usr/lib/systemd/system/slurmctld.service; enabled; pres>
     Active: failed (Result: exit-code) since Tue 2025-04-29 13:44:12 KST; 2m>
   Duration: 98ms
   Main PID: 6105 (code=exited, status=1/FAILURE)
        CPU: 49ms

 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: slurmctld >
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No memory >
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: _sync_node>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Recovered >
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: read_slurm>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: Running as>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: No paramet>
 4월 29 13:44:12 localhost.localdomain slurmctld[6105]: slurmctld: mcs: MCSPa>
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: Main pr>
 4월 29 13:44:12 localhost.localdomain systemd[1]: slurmctld.service: Failed >
~
~
~
~
[root@localhost ~]# sudo nano /etc/slurm/slurm.conf
[root@localhost ~]# sudo mkdir -p /var/spool/slurmctld
[root@localhost ~]# sudo mkdir -p /var/spool/slurmd
[root@localhost ~]# sudo touch /var/log/slurmctld.log
[root@localhost ~]# sudo touch /var/log/slurmd.log
[root@localhost ~]# sudo chown slurm:slurm /var/spool/slurmctld /var/spool/slurmd /var/log/slurmctld.log /var
[root@localhost ~]# sudo systemctl restart slurmctld
[root@localhost ~]# sudo systemctl status slurmctld
● slurmctld.service - Slurm controller daemon
     Loaded: loaded (/usr/lib/systemd/system/slurmctld.service; enabled; pres>
     Active: active (running) since Tue 2025-04-29 13:50:02 KST; 4s ago
   Main PID: 6355 (slurmctld)
      Tasks: 14
     Memory: 2.5M
        CPU: 42ms
     CGroup: /system.slice/slurmctld.service
             ├─6355 /usr/sbin/slurmctld -D -s
             └─6356 "slurmctld: slurmscriptd"

 4월 29 13:50:02 localhost.localdomain slurmctld[6355]: slurmctld: error: Slu>
 4월 29 13:50:02 localhost.localdomain slurmctld[6355]: slurmctld: error: Slu>
 4월 29 13:50:02 localhost.localdomain slurmctld[6355]: slurmctld: error: Sta>
 4월 29 13:50:02 localhost.localdomain slurmctld[6355]: slurmctld: error: Slu>
 4월 29 13:50:02 localhost.localdomain slurmctld[6355]: slurmctld: error: Swi>
 4월 29 13:50:02 localhost.localdomain slurmctld[6355]: slurmctld: error: Tas>
 4월 29 13:50:02 localhost.localdomain slurmctld[6355]: slurmctld: error: Ign>
 4월 29 13:50:02 localhost.localdomain slurmctld[6355]: slurmctld: No memory >
 4월 29 13:50:02 localhost.localdomain slurmctld[6355]: slurmctld: No paramet>
 4월 29 13:50:02 localhost.localdomain slurmctld[6355]: slurmctld: mcs: MCSPa>
[root@localhost ~]# sudo systemctld status slurmd
sudo: systemctld: 명령이 없습니다
[root@localhost ~]# sudo systemctld status slurmd
sudo: systemctld: 명령이 없습니다
[root@localhost ~]# sudo systemctld status slurm
sudo: systemctld: 명령이 없습니다
[root@localhost ~]# sinfo
sinfo: error: Ignoring ControlMachine since SlurmctldHost is set.
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
debug*       up   infinite      1   unk* localhost.localdomain
[root@localhost ~]# sudo nano /etc/slurm/slurm.conf
[root@localhost ~]# hostnamectl
   Static hostname: (unset)                                 
Transient hostname: localhost
         Icon name: computer-desktop
           Chassis: desktop 🖥️
        Machine ID: e6da25a06b62410c87c3f1c8b7b92294
           Boot ID: 96ab72b4f50041d2881bb42c863abf80
  Operating System: Red Hat Enterprise Linux 9.5 (Plow)     
       CPE OS Name: cpe:/o:redhat:enterprise_linux:9::baseos
            Kernel: Linux 5.14.0-503.11.1.el9_5.x86_64
      Architecture: x86-64
   Hardware Vendor: Dell Inc.
    Hardware Model: Precision 7820 Tower
  Firmware Version: 1.4.1
[root@localhost ~]# sudo systemctl restart slurmctld
[root@localhost ~]# sinfo
sinfo: error: Ignoring ControlMachine since SlurmctldHost is set.
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
debug*       up   infinite      1    unk localhost.localdomain
[root@localhost ~]# sudo hostnamectl set-hostname localhost.localdomain
[root@localhost ~]# sudo nano /etc/slurm/slurm.conf
[root@localhost ~]# sudo nano /etc/slurm/slurm.conf
[root@localhost ~]# sudo nano /etc/hosts
[root@localhost ~]# sudo systemctl restart slurmctld
[root@localhost ~]# hostnamectl
   Static hostname: localhost.localdomain
Transient hostname: localhost
         Icon name: computer-desktop
           Chassis: desktop 🖥️
        Machine ID: e6da25a06b62410c87c3f1c8b7b92294
           Boot ID: 96ab72b4f50041d2881bb42c863abf80
  Operating System: Red Hat Enterprise Linux 9.5 (Plow)     
       CPE OS Name: cpe:/o:redhat:enterprise_linux:9::baseos
            Kernel: Linux 5.14.0-503.11.1.el9_5.x86_64
      Architecture: x86-64
   Hardware Vendor: Dell Inc.
    Hardware Model: Precision 7820 Tower
  Firmware Version: 1.4.1
[root@localhost ~]# sinfo
sinfo: error: Ignoring ControlMachine since SlurmctldHost is set.
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
debug*       up   infinite      1   unk* localhost.localdomain
[root@localhost ~]# sudo systemctl start slurmd
[root@localhost ~]# sudo systemctl enable slurmd
[root@localhost ~]# sudo systemctl status slurmd
● slurmd.service - Slurm node daemon
     Loaded: loaded (/usr/lib/systemd/system/slurmd.service; enabled; preset:>
     Active: active (running) since Tue 2025-04-29 09:05:22 KST; 4h 53min ago
   Main PID: 2300 (slurmd)
      Tasks: 2
     Memory: 4.5M
        CPU: 471ms
     CGroup: /system.slice/slurmd.service
             └─2300 /usr/sbin/slurmd -D -s

 4월 29 13:40:35 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:42:43 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:44:51 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:45:16 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:46:59 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:49:07 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:51:15 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:53:23 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:55:31 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:57:39 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
...skipping...
● slurmd.service - Slurm node daemon
     Loaded: loaded (/usr/lib/systemd/system/slurmd.service; enabled; preset:>
     Active: active (running) since Tue 2025-04-29 09:05:22 KST; 4h 53min ago
   Main PID: 2300 (slurmd)
      Tasks: 2
     Memory: 4.5M
        CPU: 471ms
     CGroup: /system.slice/slurmd.service
             └─2300 /usr/sbin/slurmd -D -s

 4월 29 13:40:35 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:42:43 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:44:51 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:45:16 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:46:59 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:49:07 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:51:15 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:53:23 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:55:31 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:57:39 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
~
[root@localhost ~]# sinfo
sinfo: error: Ignoring ControlMachine since SlurmctldHost is set.
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
debug*       up   infinite      1   unk* localhost.localdomain
[root@localhost ~]# sudo systemctl status slurmd
● slurmd.service - Slurm node daemon
     Loaded: loaded (/usr/lib/systemd/system/slurmd.service; enabled; preset:>
     Active: active (running) since Tue 2025-04-29 09:05:22 KST; 4h 53min ago
   Main PID: 2300 (slurmd)
      Tasks: 2
     Memory: 4.5M
        CPU: 471ms
     CGroup: /system.slice/slurmd.service
             └─2300 /usr/sbin/slurmd -D -s

 4월 29 13:40:35 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:42:43 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:44:51 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:45:16 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:46:59 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:49:07 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:51:15 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:53:23 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:55:31 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
 4월 29 13:57:39 localhost.localdomain slurmd[2300]: slurmd: error: slurmd_re>
[root@localhost ~]# grep -E "ControlMachine|NodeName|PartitionName" /etc/slurm/slurm.conf
NodeName=localhost.localdomain CPUs=1 State=UNKNOWN
PartitionName=debug Nodes=ALL Default=YES MaxTime=INFINITE State=UP
ControlMachine=locvalhost.localdomain
[root@localhost ~]# sudo nano /etc/slurm/slurm.conf
[root@localhost ~]# sudo systemctl restart slurmctld
sudo [root@localhost ~]# su
[root@localhost ~]# sudo systemctl restart slurmd
[root@localhost ~]# sinfo
sinfo: error: Ignoring ControlMachine since SlurmctldHost is set.
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
debug*       up   infinite      1   idle localhost.localdomain
[root@localhost ~]# scp /etc/slurm/slurm.conf root@192.168.0.37:/etc/slurm/
root@192.168.0.37's password: 
slurm.conf                                  100% 3279     2.9MB/s   00:00    
[root@localhost ~]# 
[root@localhost ~]# systemctl start slurmctld
[root@localhost ~]# systemctl enable slurmctld
^[[A[root@localhost ~]# systemctl status slurmctld
● slurmctld.service - Slurm controller daemon
     Loaded: loaded (/usr/lib/systemd/system/slurmctld.service; enabled; pres>
     Active: active (running) since Tue 2025-04-29 14:00:44 KST; 1h 9min ago
   Main PID: 6804 (slurmctld)
      Tasks: 10
     Memory: 4.4M
        CPU: 1.258s
     CGroup: /system.slice/slurmctld.service
             ├─6804 /usr/sbin/slurmctld -D -s
             └─6806 "slurmctld: slurmscriptd"

 4월 29 14:00:45 localhost.localdomain slurmctld[6804]: slurmctld: error: Slu>
 4월 29 14:00:45 localhost.localdomain slurmctld[6804]: slurmctld: error: Swi>
 4월 29 14:00:45 localhost.localdomain slurmctld[6804]: slurmctld: error: Tas>
 4월 29 14:00:45 localhost.localdomain slurmctld[6804]: slurmctld: error: Ign>
 4월 29 14:00:45 localhost.localdomain slurmctld[6804]: slurmctld: No memory >
 4월 29 14:00:45 localhost.localdomain slurmctld[6804]: slurmctld: No paramet>
 4월 29 14:00:45 localhost.localdomain slurmctld[6804]: slurmctld: mcs: MCSPa>
 4월 29 14:01:45 localhost.localdomain slurmctld[6804]: slurmctld: SchedulerP>
 4월 29 14:01:55 localhost.localdomain slurmctld[6804]: slurmctld: Node local>
 4월 29 14:05:45 localhost.localdomain slurmctld[6804]: slurmctld: error: Nod>
[root@localhost ~]# systemctl status slurmctld
● slurmctld.service - Slurm controller daemon
     Loaded: loaded (/usr/lib/systemd/system/slurmctld.service; enabled; pres>
     Active: active (running) since Tue 2025-04-29 14:00:44 KST; 1h 9min ago
   Main PID: 6804 (slurmctld)
      Tasks: 10
     Memory: 4.4M
        CPU: 1.260s
     CGroup: /system.slice/slurmctld.service
             ├─6804 /usr/sbin/slurmctld -D -s
             └─6806 "slurmctld: slurmscriptd"

 4월 29 14:00:45 localhost.localdomain slurmctld[6804]: slurmctld: error: Slu>
 4월 29 14:00:45 localhost.localdomain slurmctld[6804]: slurmctld: error: Swi>
 4월 29 14:00:45 localhost.localdomain slurmctld[6804]: slurmctld: error: Tas>
 4월 29 14:00:45 localhost.localdomain slurmctld[6804]: slurmctld: error: Ign>
 4월 29 14:00:45 localhost.localdomain slurmctld[6804]: slurmctld: No memory >
 4월 29 14:00:45 localhost.localdomain slurmctld[6804]: slurmctld: No paramet>
 4월 29 14:00:45 localhost.localdomain slurmctld[6804]: slurmctld: mcs: MCSPa>
 4월 29 14:01:45 localhost.localdomain slurmctld[6804]: slurmctld: SchedulerP>
 4월 29 14:01:55 localhost.localdomain slurmctld[6804]: slurmctld: Node local>
 4월 29 14:05:45 localhost.localdomain slurmctld[6804]: slurmctld: error: Nod>
[root@localhost ~]# sinfo
sinfo: error: Ignoring ControlMachine since SlurmctldHost is set.
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
debug*       up   infinite      1   idle localhost.localdomain
[root@localhost ~]# sbatch test_job.sh
sbatch: error: Ignoring ControlMachine since SlurmctldHost is set.
Submitted batch job 1
[root@localhost ~]# cat output.txt
===== 기본 시스템 정보 =====
localhost.localdomain
root
2025. 04. 29. (화) 15:10:57 KST
 15:10:57 up  6:12,  2 users,  load average: 0.35, 0.09, 0.03
===== CPU 정보 =====
Architecture:                         x86_64
CPU op-mode(s):                       32-bit, 64-bit
Address sizes:                        46 bits physical, 48 bits virtual
Byte Order:                           Little Endian
CPU(s):                               40
On-line CPU(s) list:                  0-39
Vendor ID:                            GenuineIntel
BIOS Vendor ID:                       Intel(R) Corporation
Model name:                           Intel(R) Xeon(R) Silver 4114 CPU @ 2.20GHz
BIOS Model name:                      Intel(R) Xeon(R) Silver 4114 CPU @ 2.20GHz
CPU family:                           6
Model:                                85
Thread(s) per core:                   2
Core(s) per socket:                   10
Socket(s):                            2
Stepping:                             4
CPU(s) scaling MHz:                   28%
CPU max MHz:                          3000.0000
CPU min MHz:                          800.0000
BogoMIPS:                             4400.00
Flags:                                fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc art arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc cpuid aperfmperf pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch cpuid_fault epb cat_l3 cdp_l3 pti intel_ppin ssbd mba ibrs ibpb stibp tpr_shadow flexpriority ept vpid ept_ad fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm cqm mpx rdt_a avx512f avx512dq rdseed adx smap clflushopt clwb intel_pt avx512cd avx512bw avx512vl xsaveopt xsavec xgetbv1 xsaves cqm_llc cqm_occup_llc cqm_mbm_total cqm_mbm_local dtherm ida arat pln pts hwp hwp_act_window hwp_epp hwp_pkg_req vnmi pku ospke md_clear flush_l1d arch_capabilities
Virtualization:                       VT-x
L1d cache:                            640 KiB (20 instances)
L1i cache:                            640 KiB (20 instances)
L2 cache:                             20 MiB (20 instances)
L3 cache:                             27.5 MiB (2 instances)
NUMA node(s):                         2
NUMA node0 CPU(s):                    0-9,20-29
NUMA node1 CPU(s):                    10-19,30-39
Vulnerability Gather data sampling:   Mitigation; Microcode
Vulnerability Itlb multihit:          KVM: Mitigation: VMX disabled
Vulnerability L1tf:                   Mitigation; PTE Inversion; VMX conditional cache flushes, SMT vulnerable
Vulnerability Mds:                    Mitigation; Clear CPU buffers; SMT vulnerable
Vulnerability Meltdown:               Mitigation; PTI
Vulnerability Mmio stale data:        Mitigation; Clear CPU buffers; SMT vulnerable
Vulnerability Reg file data sampling: Not affected
Vulnerability Retbleed:               Mitigation; IBRS
Vulnerability Spec rstack overflow:   Not affected
Vulnerability Spec store bypass:      Mitigation; Speculative Store Bypass disabled via prctl
Vulnerability Spectre v1:             Mitigation; usercopy/swapgs barriers and __user pointer sanitization
Vulnerability Spectre v2:             Mitigation; IBRS; IBPB conditional; STIBP conditional; RSB filling; PBRSB-eIBRS Not affected; BHI Not affected
Vulnerability Srbds:                  Not affected
Vulnerability Tsx async abort:        Mitigation; Clear CPU buffers; SMT vulnerable
===== 메모리 정보 =====
               total        used        free      shared  buff/cache   available
Mem:           187Gi       3.8Gi       182Gi        42Mi       1.5Gi       183Gi
Swap:          4.0Gi          0B       4.0Gi
===== 디스크 정보 =====
Filesystem             Size  Used Avail Use% Mounted on
devtmpfs               4.0M     0  4.0M   0% /dev
tmpfs                   94G     0   94G   0% /dev/shm
tmpfs                   38G   19M   38G   1% /run
efivarfs               512K   99K  409K  20% /sys/firmware/efi/efivars
/dev/mapper/rhel-root   70G  6.1G   64G   9% /
/dev/md126p2           960M  533M  428M  56% /boot
/dev/md126p1           599M  7.1M  592M   2% /boot/efi
/dev/mapper/rhel-home  3.4T   25G  3.4T   1% /home
tmpfs                   19G  112K   19G   1% /run/user/1000
===== 환경 변수 =====
SHELL=/bin/bash
SLURM_JOB_USER=root
SLURM_TASKS_PER_NODE=1
SLURM_JOB_UID=0
HISTCONTROL=ignoredups
SLURM_TASK_PID=8133
SLURM_LOCALID=0
SLURM_SUBMIT_DIR=/root
HISTSIZE=1000
HOSTNAME=localhost.localdomain
SLURMD_NODENAME=localhost.localdomain
SLURM_NODE_ALIASES=(null)
SLURM_CLUSTER_NAME=cluster
SLURM_CPUS_ON_NODE=1
SLURM_JOB_CPUS_PER_NODE=1
PWD=/root
SLURM_GTIDS=0
LOGNAME=root
SLURM_JOB_PARTITION=debug
SLURM_JOB_NUM_NODES=1
XAUTHORITY=/root/.xauth8jzujS
SLURM_JOBID=1
HOME=/root
LANG=ko_KR.UTF-8
LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=01;37;41:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.webp=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=01;36:*.au=01;36:*.flac=01;36:*.m4a=01;36:*.mid=01;36:*.midi=01;36:*.mka=01;36:*.mp3=01;36:*.mpc=01;36:*.ogg=01;36:*.ra=01;36:*.wav=01;36:*.oga=01;36:*.opus=01;36:*.spx=01;36:*.xspf=01;36:
SLURM_PROCID=0
TMPDIR=/tmp
SLURM_NTASKS=1
SLURM_TOPOLOGY_ADDR=localhost.localdomain
SLURM_TOPOLOGY_ADDR_PATTERN=node
SLURM_WORKING_CLUSTER=cluster:localhost.localdomain:6817:9728:102
TERM=xterm-256color
LESSOPEN=||/usr/bin/lesspipe.sh %s
USER=root
SLURM_NODELIST=localhost.localdomain
ENVIRONMENT=BATCH
SLURM_PRIO_PROCESS=0
SLURM_NPROCS=1
DISPLAY=:0
SHLVL=3
SLURM_NNODES=1
SLURM_SUBMIT_HOST=localhost.localdomain
SLURM_JOB_ID=1
SLURM_NODEID=0
which_declare=declare -f
XDG_DATA_DIRS=/root/.local/share/flatpak/exports/share:/var/lib/flatpak/exports/share:/usr/local/share:/usr/share
SLURM_CONF=/etc/slurm/slurm.conf
PATH=/root/.local/bin:/root/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin
SLURM_JOB_NAME=test
MAIL=/var/spool/mail/root
SLURM_JOB_GID=0
SLURM_JOB_NODELIST=localhost.localdomain
BASH_FUNC_which%%=() {  ( alias;
 eval ${which_declare} ) | /usr/bin/which --tty-only --read-alias --read-functions --show-tilde --show-dot $@
}
_=/usr/bin/env
===== 현재 작업 디렉토리 =====
/root
===== 네트워크 인터페이스 =====
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: enp0s31f6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 54:bf:64:6d:2e:6b brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.44/24 brd 192.168.0.255 scope global dynamic noprefixroute enp0s31f6
       valid_lft 6864sec preferred_lft 6864sec
    inet6 fe80::56bf:64ff:fe6d:2e6b/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
===== Slurm 관련 정보 =====
scontrol: error: Ignoring ControlMachine since SlurmctldHost is set.
JobId=1 JobName=test
   UserId=root(0) GroupId=root(0) MCS_label=N/A
   Priority=4294901759 Nice=0 Account=(null) QOS=(null)
   JobState=RUNNING Reason=None Dependency=(null)
   Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
   RunTime=00:00:00 TimeLimit=00:01:00 TimeMin=N/A
   SubmitTime=2025-04-29T15:10:56 EligibleTime=2025-04-29T15:10:56
   AccrueTime=2025-04-29T15:10:56
   StartTime=2025-04-29T15:10:57 EndTime=2025-04-29T15:11:57 Deadline=N/A
   SuspendTime=None SecsPreSuspend=0 LastSchedEval=2025-04-29T15:10:57 Scheduler=Main
   Partition=debug AllocNode:Sid=localhost:4052
   ReqNodeList=(null) ExcNodeList=(null)
   NodeList=localhost.localdomain
   BatchHost=localhost.localdomain
   NumNodes=1 NumCPUs=1 NumTasks=1 CPUs/Task=1 ReqB:S:C:T=0:0:*:*
   TRES=cpu=1,node=1,billing=1
   Socks/Node=* NtasksPerN:B:S:C=0:0:*:* CoreSpec=*
   MinCPUsNode=1 MinMemoryNode=0 MinTmpDiskNode=0
   Features=(null) DelayBoot=00:00:00
   OverSubscribe=NO Contiguous=0 Licenses=(null) Network=(null)
   Command=/root/test_job.sh
   WorkDir=/root
   StdErr=/root/output.txt
   StdIn=/dev/null
   StdOut=/root/output.txt
   Power=
