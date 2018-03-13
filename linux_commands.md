
Use the following command to determine physical RAM size on the server

`# grep MemTotal /proc/meminfo`
> MemTotal:       32664752 kB

Determine the size of the configured swap space:

`# grep SwapTotal /proc/meminfo`
>SwapTotal:       2097148 kB

Determine the amount of space

`# df -h`

Filesystem | Size | Used | Avail | Use% | Mounted on
-------------|-------:|-----:|----:|---:|-------------
devtmpfs     |   16G  |  0   |  16G|   0% |/dev
tmpfs        |   16G  |   0  | 16G |  0% |/dev/shm
tmpfs        |   16G  | 17M  | 16G |  1% |/run
tmpfs        |   16G  |   0  | 16G |  0% |/sys/fs/cgroup
/dev/sda2    |   30G  | 1018M| 27G |  4% |/
/dev/sda1    |  497M  |125M  | 372M|  26% |/boot
/dev/sdb1    |   63G  |2.1G  | 58G  | 4% |/mnt/resource
tmpfs        |  3.2G  |   0  |3.2G  | 0% |/run/user/1000

Determine the amount of free RAM and disk swap space on the system:

`# free`

Mem/Swap | total| used | free | shared | buff/cache | available
----------|-----------:|--------:|-----------:|--------:|----------:|-----------
Mem:      | 32664752    |  197348  |  32267284   |    16700     | 200120  |  32239248
Swap:      | 2097148   |        0   |  2097148|||

Determine the system architecture

`# uname -m`
>x86_64

Determine the distribution and version of Linux installed

`# cat /etc/oracle-release`

>Oracle Linux Server release 7.4


