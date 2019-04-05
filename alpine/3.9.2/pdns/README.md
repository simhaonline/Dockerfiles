# PowerDNS Docker Container

[![Image Size](https://images.microbadger.com/badges/image/psitrax/powerdns.svg)](https://microbadger.com/images/psitrax/powerdns)
[![Docker Stars](https://img.shields.io/docker/stars/psitrax/powerdns.svg)](https://hub.docker.com/r/psitrax/powerdns/)
[![Docker Pulls](https://img.shields.io/docker/pulls/psitrax/powerdns.svg)](https://hub.docker.com/r/psitrax/powerdns/)
[![Docker Automated buil](https://img.shields.io/docker/automated/psitrax/powerdns.svg)](https://hub.docker.com/r/psitrax/powerdns/)

* Small Alpine based Image
* MySQL (default), Postgres, SQLite and Bind backend included
* Automatic MySQL database initialization
* Latest PowerDNS version (if not pls file an issue)
* Guardian process enabled
* Graceful shutdown using pdns_control

## Supported tags

* Exact: i.e. `4.1.7`: PowerDNS Version 4.1.7
* `4.0`: PowerDNS Version 4.0.x, latest image build
* `4`: PowerDNS Version 4.x.x, latest image build

## Usage

```shell
# Start a MySQL Container
$ docker build -t alpine-pdns:4.1.7 .

$ docker run \
  --detach \
  --name pdns-mysql \
  --env MYSQL_ROOT_PASSWORD=S1mhA@ZOIG \
  --volume /srv/pdns/mysql-data:/var/lib/mysql \
  mariadb:10.1
  
$ docker run \
  --detach \
  --name pdns \
  --link pdns-mysql:mysql \
  --publish 53:53/tcp \
  --publish 53:53/udp \
  --env MYSQL_USER=root \
  --env MYSQL_PASS=S1mhA@ZOIG \
  alpine-pdns:4.1.7 \
    --cache-ttl=120 \
    --allow-axfr-ips=127.0.0.1,51.158.120.35,10.5.74.169
```

## Configuration

**Environment Configuration:**

* MySQL connection settings
  * `MYSQL_HOST=mysql`
  * `MYSQL_USER=root`
  * `MYSQL_PASS=root`
  * `MYSQL_DB=pdns`
* Want to disable mysql initialization? Use `MYSQL_AUTOCONF=false`
* Want to use own config files? Mount a Volume to `/etc/pdns/conf.d` or simply overwrite `/etc/pdns/pdns.conf`

**PowerDNS Configuration:**

Append the PowerDNS setting to the command as shown in the example above.  
See `docker run --rm psitrax/powerdns --help`


## License

[GNU General Public License v2.0](https://github.com/PowerDNS/pdns/blob/master/COPYING) applyies to PowerDNS and all files in this repository.


## Maintainer

* Simha Online Services Private Limited <simhaonline.in@gmail.com>

### Credits

* Mehulkumar Bhatt <mehulsbhatt@gmail.com>
