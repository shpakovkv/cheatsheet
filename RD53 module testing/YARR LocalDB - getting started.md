mYARR LocalDB
============

Docs: https://localdb-docs.readthedocs.io

Git: https://gitlab.cern.ch/YARR/localdb-tools.git

Installation
------------

https://localdb-docs.readthedocs.io/en/master/installation/manual-centos/

#### Basic requirements

- [ ] **Install YARR requirements first!**
```sh
$ sudo yum install centos-release-scl
$ sudo yum install devtoolset-7
$ sudo yum install epel-release
$ sudo yum install cmake3
$ cmake3 --version
cmake3 version 3.14.6
```

- [ ] Set up gcc 7.0:
```sh
$ source /opt/rh/devtoolset-7/enable
$ g++ --version
g++ (GCC) 7.3.1 20180303 (Red Hat 7.3.1-5)
```

- [ ] Install other packages needed for YARR SW compilation:
```sh
sudo yum install git screen gnuplot texlive-epstopdf ghostscript
```

- [ ] Install python3 and pip
```sh
$ sudo yum install epel-release
$ sudo yum install python3.x86_64
$ python3 --version
Python 3.6.8
```

- [ ] LocalDB requires several pip modules for Local DB tools compilation.
```sh
$ sudo python3 -m pip install \
arguments \
coloredlogs \
pdf2image \
Pillow \
pymongo \
python-dateutil \
PyYAML \
pytz \
matplotlib \
numpy \
requests \
tzlocal \
influxdb \
pandas
```

#### Installation for DB machine

- [ ] For DAQ machine install addition yum packages
```sh
\$ sudo yum install git screen gnuplot texlive-epstopdf ghostscript
```

#### Installation for DB machine

- [ ] Install all packages needed for DAQ machine (listed above).

- [ ] For DB machine install addition python3 packages
```sh
$ sudo python3 -m pip install \
Flask \
Flask-PyMongo \
Flask-HTTPAuth \
Flask-Mail \
prettytable \
plotly \
itkdb \
```

- [ ] LocalDB requires MongoDB version 4.2 or higher for Local DB.
You can run the upgrade_mongodb_centos.sh shell to install/upgrade MongoDB on centOS7. You need to specify the port number parameter for the script (default 27017).
```sh
$ cd localdb-tools/scripts/shell
$ ./upgrade_mongoDB_centos.sh -p <port_number>
[sudo] password for dbuser: XXXXXXX
OK!
Install MongoDB version 4.2? [y/n]
y
< Installing packages with many texts >
$ mongo [--port <port number>]
MongoDB shell version v4.2.1
...
> db
test
> exit
bye
```

- [ ] Start MongoDB?
- [ ] Install InfluxDB. See [influxDB Installation](https://docs.influxdata.com/influxdb/v1.7/introduction/installation/) to get more detail.
```sh
# add repository
$ cat <<EOF | sudo tee /etc/yum.repos.d/influxdb.repo
[influxdb]
name = InfluxDB Repository - RHEL \$releasever
baseurl = https://repos.influxdata.com/rhel/\$releasever/\$basearch/stable
enabled = 1
gpgcheck = 1
gpgkey = https://repos.influxdata.com/influxdb.key
EOF
# install
$ sudo yum install influxdb
# start service
$ sudo systemctl start influxdb
# check
$ influx
Influx DB shell version X.X.X
> help
> quit
```
- [ ] Install Grafana. See [Grafana Installation](https://grafana.com/docs/grafana/latest/installation/requirements/) to get more detail.
```sh
#add repository
$ cat <<EOF | sudo tee /etc/yum.repos.d/grafana.repo
[grafana]
name=grafana
baseurl=https://packages.grafana.com/oss/rpm
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://packages.grafana.com/gpg.key
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
EOF
# install
$ sudo yum install grafana
# start service
$ sudo systemctl start grafana-server
# check
$ grafana-server -v
Version 6.5.2 (commit: 742d165, branch: HEAD)
```
- [ ]

```
MongoDB shell version v4.2.9
connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("88c84ee4-6a35-410d-83d2-0537eb9795b5") }
MongoDB server version: 4.2.9
{ "ok" : 1 }
Success! Upgraded MongoDB version to 4.2!
You can start mongodb service by
    sudo systemctl start mongod.service

```
