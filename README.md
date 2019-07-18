# Nextcloud

This setup is inspired by libre.sh which is used in production by IndieHost

## Installation

### Insecure Mode

```
git clone https://github.com/indiehosters/nextcloud.git
cp env-sample .env
docker-compose up -d
```
Change the env variables to your needs

You can now access your instance on the port 80 of the IP of your machine (not recommended for production).

### Production

```
git clone https://github.com/indiehosters/nextcloud.git
cd haproxy-tls
cp env-sample .env
```
Change the env variables to your needs

```
docker network create lb_web
docker-compose up -d
```

### Configuration

For more information on the configuration, check [nextcloud doc](https://docs.nextcloud.com/server/14/admin_manual/configuration_server/index.html) and for auto-configuration with env variables[docker-nextcloud doc](https://github.com/nextcloud/docker#auto-configuration-via-environment-variables)

### Persistence
The following folders needs to be mounted as volume in order to persist data for nextcloud

```
nextcloud:/var/www/html
apps:/var/www/html/custom_apps
config:/var/www/html/config
data:/var/www/html/data
```
For mariadb:
```
/var/lib/mysql
```
### Log rotation

For log rotation you can use logrotate

Example:
configure  `/etc/logrotate.d/docker-container` with:

```
/var/lib/docker/containers/*/*.log {
  rotate 7
  daily
  compress
  size=1M
  missingok
  delaycompress
  copytruncate
}
```

*We recommand to use tools such as flutend or ELK stack*

### Backup
In order to backup, just run the ./pre-backup script. And copy all the data to a safe place.
