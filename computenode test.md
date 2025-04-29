[ark@computenode ~]$ su -
암호:
[root@computenode ~]# hostname
computenode
[root@computenode ~]# sudo systemctl daemon-reexec
[root@computenode ~]# sudo systemctl restart slurmd
[root@computenode ~]# sudo systemctl enable slurmd
[root@computenode ~]# sudo systemctl status slurmd
● slurmd.service - Slurm Daemon
     Loaded: loaded (/etc/systemd/system/slurmd.service; enabled; preset: disab>
     Active: deactivating (stop-sigterm) since Tue 2025-04-29 15:03:59 KST; 13s>
   Main PID: 9206 (code=exited, status=0/SUCCESS)
      Tasks: 2 (limit: 408454)
     Memory: 1.6M
        CPU: 61ms
     CGroup: /system.slice/slurmd.service
             └─9208 /usr/sbin/slurmd

 4월 29 15:03:59 computenode slurmd[9206]: error: MpiDefault 1 specified more t>
 4월 29 15:03:59 computenode slurmd[9206]: error: ProctrackType 1 specified mor>
 4월 29 15:03:59 computenode slurmd[9206]: error: ReturnToService 1 specified m>
 4월 29 15:03:59 computenode slurmd[9206]: error: SlurmctldPidFile 1 specified >
 4월 29 15:03:59 computenode slurmd[9206]: error: SlurmdPidFile 1 specified mor>
 4월 29 15:03:59 computenode slurmd[9206]: error: StateSaveLocation 1 specified>
 4월 29 15:03:59 computenode slurmd[9206]: error: SlurmdSpoolDir 1 specified mo>
 4월 29 15:03:59 computenode slurmd[9206]: error: SwitchType 1 specified more t>
 4월 29 15:03:59 computenode slurmd[9206]: error: TaskPlugin 1 specified more t>
 4월 29 15:03:59 computenode slurmd[9206]: error: Ignoring ControlMachine since>
lines 1-20/20 (END)...skipping...
● slurmd.service - Slurm Daemon
     Loaded: loaded (/etc/systemd/system/slurmd.service; enabled; preset: disabled)
     Active: deactivating (stop-sigterm) since Tue 2025-04-29 15:03:59 KST; 13s ago
   Main PID: 9206 (code=exited, status=0/SUCCESS)
      Tasks: 2 (limit: 408454)
     Memory: 1.6M
        CPU: 61ms
     CGroup: /system.slice/slurmd.service
             └─9208 /usr/sbin/slurmd

 4월 29 15:03:59 computenode slurmd[9206]: error: MpiDefault 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: ProctrackType 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: ReturnToService 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: SlurmctldPidFile 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: SlurmdPidFile 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: StateSaveLocation 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: SlurmdSpoolDir 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: SwitchType 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: TaskPlugin 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: Ignoring ControlMachine since SlurmctldHost is set.
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
lines 1-20/20 (END)


































● slurmd.service - Slurm Daemon
     Loaded: loaded (/etc/systemd/system/slurmd.service; enabled; preset: disabled)
     Active: deactivating (stop-sigterm) since Tue 2025-04-29 15:03:59 KST; 13s ago
   Main PID: 9206 (code=exited, status=0/SUCCESS)
      Tasks: 2 (limit: 408454)
     Memory: 1.6M
        CPU: 61ms
     CGroup: /system.slice/slurmd.service
             └─9208 /usr/sbin/slurmd

 4월 29 15:03:59 computenode slurmd[9206]: error: MpiDefault 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: ProctrackType 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: ReturnToService 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: SlurmctldPidFile 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: SlurmdPidFile 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: StateSaveLocation 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: SlurmdSpoolDir 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: SwitchType 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: TaskPlugin 1 specified more than once, latest value used
 4월 29 15:03:59 computenode slurmd[9206]: error: Ignoring ControlMachine since SlurmctldHost is set.
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
lines 1-20/20 (END)























● slurmd.service - Slurm Daemon
     Loaded: loaded (/etc/systemd/system/slurmd.service; enabled; preset>
     Active: deactivating (stop-sigterm) since Tue 2025-04-29 15:03:59 K>
   Main PID: 9206 (code=exited, status=0/SUCCESS)
      Tasks: 2 (limit: 408454)
     Memory: 1.6M
        CPU: 61ms
     CGroup: /system.slice/slurmd.service
             └─9208 /usr/sbin/slurmd

 4월 29 15:03:59 computenode slurmd[9206]: error: MpiDefault 1 specified>
 4월 29 15:03:59 computenode slurmd[9206]: error: ProctrackType 1 specif>
 4월 29 15:03:59 computenode slurmd[9206]: error: ReturnToService 1 spec>
 4월 29 15:03:59 computenode slurmd[9206]: error: SlurmctldPidFile 1 spe>
 4월 29 15:03:59 computenode slurmd[9206]: error: SlurmdPidFile 1 specif>
 4월 29 15:03:59 computenode slurmd[9206]: error: StateSaveLocation 1 sp>
 4월 29 15:03:59 computenode slurmd[9206]: error: SlurmdSpoolDir 1 speci>
 4월 29 15:03:59 computenode slurmd[9206]: error: SwitchType 1 specified>
 4월 29 15:03:59 computenode slurmd[9206]: error: TaskPlugin 1 specified>
 4월 29 15:03:59 computenode slurmd[9206]: error: Ignoring ControlMachin>
lines 1-20/20 (END)

























● slurmd.service - Slurm Daemon
     Loaded: loaded (/etc/systemd/system/slurmd.service; enabled; preset: disa>
     Active: deactivating (stop-sigterm) since Tue 2025-04-29 15:03:59 KST; 13>
   Main PID: 9206 (code=exited, status=0/SUCCESS)
      Tasks: 2 (limit: 408454)
     Memory: 1.6M
        CPU: 61ms
     CGroup: /system.slice/slurmd.service
             └─9208 /usr/sbin/slurmd

 4월 29 15:03:59 computenode slurmd[9206]: error: MpiDefault 1 specified more >
 4월 29 15:03:59 computenode slurmd[9206]: error: ProctrackType 1 specified mo>
 4월 29 15:03:59 computenode slurmd[9206]: error: ReturnToService 1 specified >
 4월 29 15:03:59 computenode slurmd[9206]: error: SlurmctldPidFile 1 specified>
 4월 29 15:03:59 computenode slurmd[9206]: error: SlurmdPidFile 1 specified mo>
 4월 29 15:03:59 computenode slurmd[9206]: error: StateSaveLocation 1 specifie>
 4월 29 15:03:59 computenode slurmd[9206]: error: SlurmdSpoolDir 1 specified m>
 4월 29 15:03:59 computenode slurmd[9206]: error: SwitchType 1 specified more >
 4월 29 15:03:59 computenode slurmd[9206]: error: TaskPlugin 1 specified more >
 4월 29 15:03:59 computenode slurmd[9206]: error: Ignoring ControlMachine sinc>
~
~

[root@computenode ~]# sudo nano /etc/slurm/slurm.conf
[root@computenode ~]# sudo nano /etc/slurm/slurm.conf
[root@computenode ~]# 
[root@computenode ~]# sudo systemctl status slurm
Unit slurm.service could not be found.
[root@computenode ~]# sudo systemctl status slurmd
● slurmd.service - Slurm Daemon
     Loaded: loaded (/etc/systemd/system/slurmd.service; enabled; preset: disa>
     Active: deactivating (stop-sigterm) since Tue 2025-04-29 15:07:01 KST; 1m>
    Process: 9356 ExecStart=/usr/sbin/slurmd (code=exited, status=0/SUCCESS)
   Main PID: 9356 (code=exited, status=0/SUCCESS)
      Tasks: 2 (limit: 408454)
     Memory: 1.5M
        CPU: 66ms
     CGroup: /system.slice/slurmd.service
             └─9358 /usr/sbin/slurmd

 4월 29 15:07:01 computenode slurmd[9356]: error: ProctrackType 1 specified mo>
 4월 29 15:07:01 computenode slurmd[9356]: slurmd: error: Ignoring ControlMach>
 4월 29 15:07:01 computenode slurmd[9356]: error: ReturnToService 1 specified >
 4월 29 15:07:01 computenode slurmd[9356]: error: SlurmctldPidFile 1 specified>
 4월 29 15:07:01 computenode slurmd[9356]: error: SlurmdPidFile 1 specified mo>
 4월 29 15:07:01 computenode slurmd[9356]: error: StateSaveLocation 1 specifie>
 4월 29 15:07:01 computenode slurmd[9356]: error: SlurmdSpoolDir 1 specified m>
 4월 29 15:07:01 computenode slurmd[9356]: error: SwitchType 1 specified more >
 4월 29 15:07:01 computenode slurmd[9356]: error: TaskPlugin 1 specified more >
 4월 29 15:07:01 computenode slurmd[9356]: error: Ignoring ControlMachine sinc>

[root@computenode ~]# sudo systemctl restart slurmd
[root@computenode ~]# sudo systemctl status slurmd
● slurmd.service - Slurm Daemon
     Loaded: loaded (/etc/systemd/system/slurmd.service; enabled; preset: disa>
     Active: deactivating (stop-sigterm) since Tue 2025-04-29 15:08:32 KST; 7s>
    Process: 9433 ExecStart=/usr/sbin/slurmd (code=exited, status=0/SUCCESS)
   Main PID: 9433 (code=exited, status=0/SUCCESS)
      Tasks: 2 (limit: 408454)
     Memory: 1.7M
        CPU: 51ms
     CGroup: /system.slice/slurmd.service
             └─9435 /usr/sbin/slurmd

 4월 29 15:08:32 computenode systemd[1]: Started Slurm Daemon.
 4월 29 15:08:32 computenode slurmd[9433]: slurmd: error: Ignoring ControlMach>
 4월 29 15:08:32 computenode slurmd[9433]: error: Ignoring ControlMachine sinc>
lines 1-14/14 (END)...skipping...
● slurmd.service - Slurm Daemon
     Loaded: loaded (/etc/systemd/system/slurmd.service; enabled; preset: disabled)
     Active: deactivating (stop-sigterm) since Tue 2025-04-29 15:08:32 KST; 7s ago
    Process: 9433 ExecStart=/usr/sbin/slurmd (code=exited, status=0/SUCCESS)
   Main PID: 9433 (code=exited, status=0/SUCCESS)
      Tasks: 2 (limit: 408454)
     Memory: 1.7M
        CPU: 51ms
     CGroup: /system.slice/slurmd.service
             └─9435 /usr/sbin/slurmd

 4월 29 15:08:32 computenode systemd[1]: Started Slurm Daemon.
 4월 29 15:08:32 computenode slurmd[9433]: slurmd: error: Ignoring ControlMachine since SlurmctldHost is set.
 4월 29 15:08:32 computenode slurmd[9433]: error: Ignoring ControlMachine since SlurmctldHost is set.
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
lines 1-14/14 (END)


































● slurmd.service - Slurm Daemon
     Loaded: loaded (/etc/systemd/system/slurmd.service; enabled; preset: disabled)
     Active: deactivating (stop-sigterm) since Tue 2025-04-29 15:08:32 KST; 7s ago
    Process: 9433 ExecStart=/usr/sbin/slurmd (code=exited, status=0/SUCCESS)
   Main PID: 9433 (code=exited, status=0/SUCCESS)
      Tasks: 2 (limit: 408454)
     Memory: 1.7M
        CPU: 51ms
     CGroup: /system.slice/slurmd.service
             └─9435 /usr/sbin/slurmd

 4월 29 15:08:32 computenode systemd[1]: Started Slurm Daemon.
 4월 29 15:08:32 computenode slurmd[9433]: slurmd: error: Ignoring ControlMachine since SlurmctldHost is set.
 4월 29 15:08:32 computenode slurmd[9433]: error: Ignoring ControlMachine since SlurmctldHost is set.
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
lines 1-14/14 (END)






















● slurmd.service - Slurm Daemon
     Loaded: loaded (/etc/systemd/system/slurmd.service; enabled; prese>
     Active: deactivating (stop-sigterm) since Tue 2025-04-29 15:08:32 >
    Process: 9433 ExecStart=/usr/sbin/slurmd (code=exited, status=0/SUC>
   Main PID: 9433 (code=exited, status=0/SUCCESS)
      Tasks: 2 (limit: 408454)
     Memory: 1.7M
        CPU: 51ms
     CGroup: /system.slice/slurmd.service
             └─9435 /usr/sbin/slurmd

 4월 29 15:08:32 computenode systemd[1]: Started Slurm Daemon.
 4월 29 15:08:32 computenode slurmd[9433]: slurmd: error: Ignoring Cont>
 4월 29 15:08:32 computenode slurmd[9433]: error: Ignoring ControlMachi>
~
~
~
~
~
lines 1-14/14 (END)
























● slurmd.service - Slurm Daemon
     Loaded: loaded (/etc/systemd/system/slurmd.service; enabled; preset: dis>
     Active: deactivating (stop-sigterm) since Tue 2025-04-29 15:08:32 KST; 7>
    Process: 9433 ExecStart=/usr/sbin/slurmd (code=exited, status=0/SUCCESS)
   Main PID: 9433 (code=exited, status=0/SUCCESS)
      Tasks: 2 (limit: 408454)
     Memory: 1.7M
        CPU: 51ms
     CGroup: /system.slice/slurmd.service
             └─9435 /usr/sbin/slurmd

 4월 29 15:08:32 computenode systemd[1]: Started Slurm Daemon.
 4월 29 15:08:32 computenode slurmd[9433]: slurmd: error: Ignoring ControlMac>
 4월 29 15:08:32 computenode slurmd[9433]: error: Ignoring ControlMachine sin>
~
