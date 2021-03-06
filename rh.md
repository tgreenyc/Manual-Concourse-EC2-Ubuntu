
# install Postgres 
```
$ sudo yum install postgresql-server.x86_64 postgresql-contrib.x86_64 -y 
$ sudo service postgresql initdb
$ sudo service postgresql start
$ sudo -u postgres createuser concourse
$ sudo -u postgres createdb --owner=concourse atc
$ sudo vi /var/lib/pgsql/data/pg_hba.conf
ADD the two lines below 
local all all              trust
host  all all 127.0.0.1/32 trust
```
# Install concourse
```
$ wget https://github.com/concourse/concourse/releases/download/v3.3.4/concourse_linux_amd64
$ chmod +x concourse_linux_amd64 
$ sudo mv concourse* /usr/local/bin/concourse
$ sudo mkdir /etc/concourse
$ sudo groupadd concourse
$ sudo adduser concourse -g concourse -r
$ sudo chown -R concourse:concourse /etc/concourse
```

# Generate Keys
```
$ sudo ssh-keygen -t rsa -q -N '' -f /etc/concourse/tsa_host_key
$ sudo ssh-keygen -t rsa -q -N '' -f /etc/concourse/worker_key
$ sudo ssh-keygen -t rsa -q -N '' -f /etc/concourse/session_signing_key
$ sudo cp /etc/concourse/worker_key.pub /etc/concourse/authorized_worker_keys
$ sudo chown -R concourse:concourse /etc/concourse
```


# Start Web
```
$ sudo -H -u concourse /usr/local/bin/concourse web \
  --basic-auth-username myuser \
  --basic-auth-password mypass \
  --session-signing-key /etc/concourse/session_signing_key \
  --tsa-host-key /etc/concourse/tsa_host_key \
  --tsa-authorized-keys /etc/concourse/authorized_worker_keys \
  --external-url http://ec2-54-163-21-180.compute-1.amazonaws.com
```
# Start Worker
```
sudo  /usr/local/bin/concourse web \
  --basic-auth-username myuser \
  --basic-auth-password mypass \
  --session-signing-key /etc/concourse/session_signing_key \
  --tsa-host-key /etc/concourse/tsa_host_key \
  --tsa-authorized-keys /etc/concourse/authorized_worker_keys \
  --external-url http://ec2-54-163-21-180.compute-1.amazonaws.com
```
# Misc
```
disable firewall
$ sudo systemctl stop firewalld
``
