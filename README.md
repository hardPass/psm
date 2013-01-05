psm - simple, accurate memory reporting for Linux
=================================================

`psm` makes it easy to see who is resident in memory, and who is
significantly swapped out.

`psm` is based off the ideas and implementation of
[ps_mem.py](https://github.com/pixelb/scripts/commits/master/scripts/ps_mem.py).
It requires root privileges to run.  It is implemented in go, and
since the executable is a binary it can be made setuid root so that
unprivileged users can get a quick overview of the current memory
situation.

installation
------------

If you're familiar with go and have the go toolchain installed,
installation is as easy as:

    go get github.com/bpowers/psm
    sudo `which psm`

The ``sudo `which psm` `` can get a bit tiring.  If you're on
Ubuntu, there is a PPA which install psm as setuid root:

    sudo add-apt-repository ppa:bobbypowers/psm
    sudo apt-get update
    sudo apt-get install psm

example output
--------------

    bpowers@python-worker-01:~$ psm
        MB RAM   PRIVATE   SWAPPED	PROCESS (COUNT)
           0.1       0.1          	time (1)
           0.2       0.2          	/bin/sh (1)
           0.2       0.2          	atd (1)
           0.3       0.2          	acpid (1)
           0.3       0.2          	cron (1)
           0.3       0.1          	screen (1)
           0.3       0.2          	upstart-socket-bridge (1)
           0.5       0.4          	upstart-udev-bridge (1)
           0.6       0.6          	dhclient3 (1)
           0.6       0.6          	dbus-daemon (1)
           0.7       0.2          	sshd: jeff@pts/2 (1)
           0.7       0.7          	memcached (1)
           0.8       0.6          	SCREEN (1)
           0.9       0.6          	sshd (1)
           0.9       0.2          	sshd: jeff [priv] (1)
           0.9       0.2          	sshd: bpowers [priv] (1)
           1.0       0.9          	ntpd (1)
           1.1       0.7          	sudo (2)
           1.1       1.0          	rsyslogd (1)
           1.2       0.9          	getty (6)
           1.3       1.1          	init (1)
           1.4       1.0          	udevd (3)
           2.1       1.6          	sshd: bpowers@pts/0 (1)
           2.7       2.3          	bash (1)
           3.0       2.8          	whoopsie (1)
           3.6       3.6          	./psm (1)
           6.1       5.7          	-bash (1)
          25.5      25.3          	node (1)
          61.8      60.7     134.2	[celeryd@notifications:MainProcess] (1)
         116.5     115.4          	[celeryd@default:MainProcess] (1)
         359.1     357.9      28.5	[celeryd@stats:MainProcess] (1)
    #    595.7               164.7	TOTAL USED BY PROCESSES

The `MB RAM` column is the sum of the Pss value of each mapping in
`/proc/$PID/smaps` for each process.

TODO
----

- port to the BSDs and OS X
- FreeBSD has a Linux-compatable procfs impelmentation, which would
  be trivial to use (and, indeed, ps_mem.py uses it).
- OS X looks... fun.  MacFUSE provides a lot of the info we need, but
  I don't want to depend on having that installed and manually having
  their procfs mounted.  There are Mach functions we could use, but
  I'm having trouble figuring out how to correctly pass data between
  go and C.  Specifically: https://gist.github.com/4463209 - 'patches
  welcome'.
- ps_mem.py records the md5sum of each process's smaps entry to make
  sure that we're not double-counting.  Its probably worth doing.

license
-------

psm is offered under the MIT license, see LICENSE for details.
