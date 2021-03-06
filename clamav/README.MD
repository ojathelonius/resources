## ClamAV
ClamAV is an open source antivirus engine for detecting trojans, viruses, malware & other malicious threats.

* [Download & installation](#download--installation)
* [Signature database update](#signature-database-update)
* [Configuration](#configuration)
* [Run as a service](#run-as-a-service)
* [Test](#test)
* [Logging](#logging)

### Download & installation
The information below is only valid for CentOS.

#### EPEL repository
```
yum -y install epel-release
yum -y update
yum clean all
```

#### Package installation
```
yum install epel-release
yum install clamav-server clamav-data clamav-update clamav-filesystem clamav clamav-scanner-systemd clamav-devel clamav-lib clamav-server-systemd
```

### Signature database update
```
freshclam
```

### Configuration
The configuration usually sits at `/etc/clamd.d/clamd.conf`.

Comment the Example line :
```
sed -i '/^Example/d' /etc/clamd.d/clamd.conf
```

Check the user that should be used to run clamav :

```
User clamscan
```


Check socket path :
```
LocalSocket /var/run/clamd.service/clamd.sock
```

Check socket port ;
```
TCPSocket 3310
```

### Run as a service

Create a new `clamd.service` in `/etc/systemd/system/`.

Check that the `clamd.conf` has the proper path : 
```
[Unit]
Description = clamd scanner daemon
After = syslog.target nss-lookup.target network.target

[Service]
Type = simple
ExecStart = /usr/sbin/clamd -c /etc/clamd.d/clamd.conf --foreground=yes
Restart = on-failure
PrivateTmp = true

[Install]
WantedBy=multi-user.target
```

Enable and run the service :
```
systemctl enable clamd
systemctl start clamd
```

### Test
Check that the service runs properly :
```
journalctl -xe
```

Make sure ClamAV listens on the proper port :
```
netstat -ntpl
```


From the same machine, ping the service :
```
echo PING | nc -U /var/run/clamd.service/clamd.sock
echo PING | nc 127.0.0.1 3310
```

From a distant machine :
```
echo PING | nc clamav.address port
```

### Logging
By default, ClamAV has minimal logging features turned on. In particular, it does not log clean file scans.
Make it more verbose with the following :

```
LogClean yes
LogVerbose yes
LogTime yes

```

