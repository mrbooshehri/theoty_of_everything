# Install stable/LTS haproxy in centos 7

> **Note:** 2.5 is stable version and 2.6 is LTS.

## Download and compile the package

For running stable release run the following script:
```bash
LATEST_HAPROXY=$(wget -qO-  http://www.haproxy.org/download/2.5/src/ | egrep -o "haproxy-2\.[0-9]+\.[0-9]+" | head -1)
cd /usr/src/
wget http://www.haproxy.org/download/2.4/src/${LATEST_HAPROXY}.tar.gz
tar xzvf ${LATEST_HAPROXY}.tar.gz
yum install gcc-c++ openssl-devel pcre-static pcre-devel systemd-devel -y
cd /usr/src/${LATEST_HAPROXY}
make TARGET=linux-glibc USE_PCRE=1 USE_OPENSSL=1 USE_ZLIB=1 USE_CRYPT_H=1 USE_LIBCRYPT=1 USE_SYSTEMD=1
mkdir /etc/haproxy
make instal
```

For running LTS release run the following script:
```bash
LATEST_HAPROXY=$(wget -qO-  http://www.haproxy.org/download/2.6/src/ | egrep -o "haproxy-2\.[0-9]+\.[0-9]+" | head -1)
cd /usr/src/
wget http://www.haproxy.org/download/2.4/src/${LATEST_HAPROXY}.tar.gz
tar xzvf ${LATEST_HAPROXY}.tar.gz
yum install gcc-c++ openssl-devel pcre-static pcre-devel systemd-devel -y
cd /usr/src/${LATEST_HAPROXY}
make TARGET=linux-glibc USE_PCRE=1 USE_OPENSSL=1 USE_ZLIB=1 USE_CRYPT_H=1 USE_LIBCRYPT=1 USE_SYSTEMD=1
mkdir /etc/haproxy
make instal
```

## Create a systemd service ```/usr/lib/systemd/system/haproxy.service```
```bash
cat > /usr/lib/systemd/system/haproxy.service << 'EOL'
   
    [Unit]
    Description=HAProxy Load Balancer
    After=syslog.target network.target
    
    
    [Service]
    Environment="CONFIG=/etc/haproxy/haproxy.cfg" "PIDFILE=/run/haproxy.pid"
    ExecStartPre=/usr/local/sbin/haproxy -f $CONFIG -c -q
    ExecStart=/usr/local/sbin/haproxy -Ws -f $CONFIG -p $PIDFILE
    ExecReload=/usr/local/sbin/haproxy -f $CONFIG -c -q
    ExecReload=/bin/kill -USR2 $MAINPID
    KillMode=mixed
    Restart=always
    SuccessExitStatus=143
    Type=notify
    
    
    [Install]
    WantedBy=multi-user.target
EOL
```

## Create ```/etc/haproxy/haproxy.cfg```
```bash
mkdir /etc/haproxy
cat > /etc/haproxy/haproxy.cfg << 'EOL'

    global
     log /dev/log local0
     log /dev/log local1 notice
     daemon
    
    
    defaults
     log global
     option dontlognull
     timeout connect 50000
     timeout client  50000
     timeout server  50000
    
    
    listen ListenName
            bind *:80
            mode tcp
            server YourServer 127.0.0.1:80
EOL
```

## Restart HAProxy and verify bash

```bash
systemctl start haproxy
systemctl status haproxy
```
Tags:
```
#haproxy #centos
```


Related:
```
* https://serverfault.com/questions/1002144/install-haproxy-2-on-centos-7
```
