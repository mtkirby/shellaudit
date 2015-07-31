# shellaudit

Shellaudit will parse Linux audit logs for executed commands and display them in a human readable format.
It can show the date, user, and tty as such: 
        09/14/2013 14:17:30 pts3 mkirby@/home/mkirby$ uname -a
        09/14/2013 14:17:38 pts3 mkirby@/home/mkirby$ cat /etc/passwd
        09/14/2013 14:18:20 pts3 mkirby@/home/mkirby$ ps -efww


A typical single command will look like the following in the audit logs
----
time->Fri Sep 13 14:56:45 2013
type=PATH msg=audit(1379102205.473:38568): item=1 name=(null) inode=2237038 dev=fd:00 mode=0100755 ouid=0 ogid=0 rdev=00:00 obj=system_u:object_r:ld_so_t:s0
type=PATH msg=audit(1379102205.473:38568): item=0 name="/usr/bin/uname" inode=2228517 dev=fd:00 mode=0100755 ouid=0 ogid=0 rdev=00:00 obj=system_u:object_r:bin_t:s0
type=CWD msg=audit(1379102205.473:38568):  cwd="/home/mkirby"
type=EXECVE msg=audit(1379102205.473:38568): argc=2 a0="uname" a1="-a"
type=SYSCALL msg=audit(1379102205.473:38568): arch=c000003e syscall=59 success=yes exit=0 a0=e3cfa0 a1=e3d220 a2=e3c920 a3=7ffff2c5d900 items=2 ppid=917 pid=2048 auid=1000 uid=1000 gid=1000 euid=1000 suid=1000 fsuid=1000 egid=1000 sgid=1000 fsgid=1000 ses=1 tty=pts0 comm="uname" exe="/usr/bin/uname" subj=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023 key=(null)
----

Shellaudit will format it to read as such
09/14/2013 14:17:30 mkirby@/home/mkirby$ uname -a



Before you can use this program, you must first install and enable the Linux auditd service and add the following to your audit.rules file and restart the auditd service (remove the arch=64 if you are running in 32bit):
-a exit,always -F arch=b32 -S execve
-a exit,always -F arch=b64 -S execve
