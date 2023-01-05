Linux Namespaces define what resources an application can access.

List namespaces of current user
```bash
lsns
```

List namespaces of all users
```bash
sudo lsns
```

| Namespace | Description                 |
| --------- | --------------------------- |
| CGROUP    | [[Linux control groups]]    |
| USER      | User IDs and Group IDs      |
| NET       | Network                     |
| IPC       | Inter-process-communication |
| MNT       | Mount points                |
| UTS       | Unix Timesharing System     |
| PID       | Process IDs                 |

## UTS Isolation

By providing a process its own UTS, it gets isolated from the host system and can manage all resources like the hostname independently from the host system. In docker the hostname is set to a random number when the container starts.

A new process with a dedicated UTS can be created using `unshare --uts`:
```bash
sudo unshare --uts /bin/appname
```

## PID Isolation

Using PID isolation, the program can only create a single process ID and not spawn subprocesses or access the parent process:
```bash
sudo unshare --pid /bin/appname
```

Using `--pid --fork`, the program can spawn subprocesses. The process executes as a subprocess of the `unshare` process.
```bash
sudo unshare --pid --fork /bin/appname
```

Using `chroot` or `pivot_root` the subprocess can set its root folder for the file system to a subfolder of the host system; isolating the file system of the host system. The folder must replicate the linux file system structure. Ie. in Alpine Linux there is a folder `\alpine` that is dedicated for this purpose for use in docker containers. (`chroot` keeps the host system accessible using mount points; `pivot_root` does not keep mount points to the host system).

## NET Isolation

Allows isolation of network interfaces and tables using virtual network interfaces
```bash
sudo unshare --net /bin/appname
```

## USER Isolation

Provides applications with their own view on users and groups.
```bash
sudo unshare --user /bin/appname
```

There need to be a mapping between user and group IDs in the isolation to the user and group IDs of the host system.

```bash
# 0 = nobody user-ID of the isolation
# 1000 = user-ID of the hos system
sudo echo '0 1000 1' > /proc/PID/uid_map 
```

Use the `id` command to get the ids of the currently available users and groups
```bash
id
```
```
uid=1000(jd) gid=1000(jd) groups=1000(jd),4(adm),20(dialout),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),116(netdev),1001(docker)
```

## IPC isolation

Processes communicate using a shared memory space or a shared message queue.

IPCs are listed using the `ipcs` command
```bash
ipcs
```
```
------ Message Queues --------
key        msqid      owner      perms      used-bytes   messages

------ Shared Memory Segments --------
key        shmid      owner      perms      bytes      nattch     status

------ Semaphore Arrays --------
key        semid      owner      perms      nsems
```

New IPC resources can be created using the `ipcmk` command.

A process can be isolated from other IPC resources using
```bash
sudo unshare -ipc /bin/appname
```

## CGROUP Isolation

Prevents the process to access the cgroup configuration (see [[Linux control groups]]) from the parent.

The current cgroups can be listed using:
```bash
cat /proc/self/cgroup
```
```
14:misc:/
13:rdma:/
12:pids:/
11:hugetlb:/
10:net_prio:/
9:perf_event:/
8:net_cls:/
7:freezer:/
6:devices:/
5:blkio:/
4:cpuacct:/
3:cpu:/
2:cpuset:/
1:memory:/
0::/
```

## Multiple isolations

Its possible to chain multiple isolations
```bash
sudo unshare --uts --user /bin/appname
```

