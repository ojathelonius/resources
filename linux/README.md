## Linux

* [Run script as a service](#run-script-as-a-service)
* [Find large folders](#find-large-folders)
* [Kill zombie processes](#kill-zombie-processes)
* [Find text in files](#find-text-in-files)
* [Ping port](#ping-port)
* [Load and restore iptables](#load-and-restore-iptables)

### Run script as a service
The following has only been tested on CentOS.

#### Service file

Create the following file in `/usr/lib/systemd/system`.

```
[Unit]
Description = App service
After = network.target

[Service]
Type = forking
ExecStart = /home/my_user/my_app/start.sh
ExecStop=/bin/kill -15 $MAINPID
PIDFile=/home/my_user/my_app/my_app.pid
RestartSec=10
Restart=always
TimeoutStartSec=0
User=my_user
Group=my_user_group
Environment=EXAMPLE_ENV_VARIABLE=/home/my_user/toto

[Install]
WantedBy=multi-user.target
```

#### Sudoers

Add `my_user` to the `sudoers` file with `visudo` so that they can run the service without a sudo password :

```
my_user ALL =NOPASSWD: /usr/bin/systemctl status my_service.service,/usr/bin/systemctl start my_service.service,/usr/bin/systemctl stop my_service.service,/usr/bin/systemctl restart my_service.service
```

#### Enable and launch

To reload the service after modifying the service file, use :
```
systemctl daemon-reload
```

To enable the service, use :
```
systemctl enable geoserver.service
```

### Find large folders

Show filesizes in MB :
```bash
ls -l --block-size=MB
```

Show folder sizes in MB, sorted by size :
```bash
du -lh --max-depth=1|sort -rh
```

### Kill zombie processes

```bash
kill -9
```

Sometimes you might want to kill the parent process as killing its children does not help : find its pid with the following : 
```bash
ps -xal
```

### Find text in files

```bash
grep -rn 'path/to/file' -e 'searched_text'
```

Use `-rnw` instead to match the whole word.

### Ping port

```bash
nc -vz 192.168.1.41 5432
```

Also, on Windows : 
```bash
telnet 192.168.1.41 5432
```

### Load and restore iptables

```bash
iptables-save > etc/sysconfig/iptables-backup
```

```bash
iptables-restore > etc/sysconfig/iptables-backup
```
