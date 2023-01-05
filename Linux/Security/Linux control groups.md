Control groups (`cgroup`) limits the resource usage of processes

- when process starts it inherits the control group of the invoking process
- List of cgroups are found in `/sys/fs/cgroup`
- Groups are presented as directories; files represent kernel data about the group
- a new subgroup can be created using `mkdir`; kernel data is automatically created
- some files can be used to set limits for the groups; ie. max memory usage
- Docker greates a subgroup in `/sys/fs/cgroup/docker`

## Assing a process to a control group

get PID
```bash
ps
```

assing process to group
```bash
echo PID > /sys/fs/cgroup/GROUPNAME/cgroup.procs
```

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