# whatwedo base image - GitLab

## Usage

```
docker run \
    -p 80:80 \
    -p 443:443 \
    -p 22:22 \
    -e REDIS_URL=redis://redis:6379
    -e GITLAB_ROOT_PASSWORD=mysecretpassword
    -e UNICORN_WORKER_PROCESSES=5
    -e GITLAB_HOST=example.com
    -e GITLAB_PORT=443
    -e GITLAB_HTTPS=true
    -e GITLAB_EMAIL_FROM=utc
    -e GITLAB_EMAIL_FROM=gitlab@example.com
    -e GITLAB_EMAIL_DISPLAY_NAME=GitLab
    -e GITLAB_EMAIL_REPLY_TO=noreply@example.com
    -e GITLAB_EMAIL_SMTP_ADDRESS=smtp.gmail.com
    -e GITLAB_EMAIL_SMTP_USERNAME=example@example.com
    -e GITLAB_EMAIL_SMTP_PASSWORD=mysecretpassword
    -e GITLAB_EMAIL_SMTP_PORT=587
    -e GITLAB_EMAIL_SMTP_AUTHENTICATION=plain
    -e GITLAB_EMAIL_SMTP_DOMAIN=example.com
    -e GITLAB_EMAIL_SMTP_VERIFY_MODE=peer
    -e GITLAB_DEFAULT_THEME=6
    -e GITLAB_PROJECT_FEATURES_ISSUES=true
    -e GITLAB_PROJECT_FEATURES_MERGE_REQUEST=true
    -e GITLAB_PROJECT_FEATURES_WIKI=true
    -e GITLAB_PROJECT_FEATURES_SNIPPETS=true
    -e GITLAB_PROJECT_FEATURES_BUILDS=true
    -e GITLAB_DATABASE_SECRET_KEY=mysecretkey
    -e GITLAB_SECRET_KEY_BASE=mysecretkey
    -e GITLAB_DATABASE_OTP_KEY_BASE=mysecretkey
    -e GITLAB_BACKUP_KEEP_TIME=3600
    -e GITLAB_BACKUP_CRON=20 * * * *
    -e DATABASE_USER=root
    -e DATABASE_PASSWORD=mysecretpassword
    -e DATABASE_NAME=gitlabhq_production
    -e DATABASE_HOST=db
    -e GITLAB_PROJECT_FEATURES_CONTAINER_REGISTRY=false
    -e GITLAB_INCOMING_EMAIL_ENABLED=false
    -e GITLAB_INCOMING_EMAIL_ADDRESS=example@example.com 
    -e GITLAB_INCOMING_EMAIL_USER=exanoke
    -e GITLAB_INCOMING_EMAIL_PASSWORD=mysecretpassword
    -e GITLAB_INCOMING_EMAIL_IMAP_HOST=imap.example.com
    -e GITLAB_INCOMING_EMAIL_IMAP_PORT=143
    -e GITLAB_INCOMING_EMAIL_IMAP_SSL=false
    -e GITLAB_INCOMING_EMAIL_IMAP_STARTTLS=false
    -e GITLAB_INCOMING_EMAIL_IMAP_MAILBOX= "inbox"        
    -e CONTAINER_REGISTRY_HOST=registry.gitlab.example.com
    -e CONTAINER_REGISTRY_PORT=5005
    -e CONTAINER_REGISTRY_API_URL=http://localhost:5000/
    -e CONTAINER_REGISTRY_ISSUER=gitlab-issuer
    whatwedo/gitlab
```

## docker-compose

```
gitlab:
  restart: always
  image: whatwedo/gitlab
  links:
    - "db:db"
    - "redis:redis"
  ports:
    - "443:443"
    - "80:80"
    - "22:22"
  environment:
    - REDIS_URL=redis://redis:6379
    - GITLAB_ROOT_PASSWORD=mysecretpassword
    - UNICORN_WORKER_PROCESSES=5
    - GITLAB_HOST=example.com
    - GITLAB_PORT=443
    - GITLAB_HTTPS=true
    - GITLAB_EMAIL_FROM=gitlab@example.com
    - GITLAB_EMAIL_SMTP_ADDRESS=smtp.gmail.com
    - GITLAB_EMAIL_SMTP_USERNAME=example@example.com
    - GITLAB_EMAIL_SMTP_PASSWORD=mysecretpassword
    - GITLAB_EMAIL_SMTP_PORT=587
    - GITLAB_EMAIL_SMTP_AUTHENTICATION=plain
    - GITLAB_EMAIL_SMTP_DOMAIN=example.com
    - GITLAB_EMAIL_DISPLAY_NAME=GitLab
    - GITLAB_EMAIL_REPLY_TO=noreply@example.com
    - GITLAB_EMAIL_SMTP_VERIFY_MODE=peer
    - GITLAB_DEFAULT_THEME=6
    - GITLAB_PROJECT_FEATURES_ISSUES=true
    - GITLAB_PROJECT_FEATURES_MERGE_REQUEST=true
    - GITLAB_PROJECT_FEATURES_WIKI=true
    - GITLAB_PROJECT_FEATURES_SNIPPETS=true
    - GITLAB_PROJECT_FEATURES_BUILDS=true
    - GITLAB_DATABASE_SECRET_KEY=mysecretkey
    - GITLAB_BACKUP_KEEP_TIME=3600
    - GITLAB_BACKUP_CRON=20 * * * *
    - DATABASE_USER=root
    - DATABASE_PASSWORD=mysecretpassword
    - DATABASE_NAME=gitlabhq_production
    - DATABASE_HOST=db
    - GITLAB_PROJECT_FEATURES_CONTAINER_REGISTRY=false
    - GITLAB_INCOMING_EMAIL_ENABLED=false
    - GITLAB_INCOMING_EMAIL_ADDRESS=example@example.com
    - GITLAB_INCOMING_EMAIL_USER=exanoke
    - GITLAB_INCOMING_EMAIL_PASSWORD=mysecretpassword
    - GITLAB_INCOMING_EMAIL_IMAP_HOST=imap.example.com
    - GITLAB_INCOMING_EMAIL_IMAP_PORT=143
    - GITLAB_INCOMING_EMAIL_IMAP_SSL=false
    - GITLAB_INCOMING_EMAIL_IMAP_STARTTLS=false
    - GITLAB_INCOMING_EMAIL_IMAP_MAILBOX= "inbox"
    - CONTAINER_REGISTRY_HOST=registry.gitlab.example.com
    - CONTAINER_REGISTRY_PORT=5005
    - CONTAINER_REGISTRY_API_URL=http://localhost:5000/
    - CONTAINER_REGISTRY_ISSUER=gitlab-issuer 
  volumes:
    - /srv/docker/gitlab/gitlab/data:/data
db:
  restart: always
  image: whatwedo/mariadb:latest
  ports:
    - "3306:3306"
  environment:
    - MYSQL_ROOT_PASSWORD=mysecretpassword
    - MYSQL_DATABASE=gitlabhq_production
  volumes:
    - /srv/docker/gitlab/db/var/lib/mysql:/var/lib/mysql
redis:
  restart: always
  image: whatwedo/redis:latest
```

## Running commands

### Checking Application Status
```
docker exec -i -t ID bash -c "cd /home/git/gitlab && sudo -u git -H bundle exec rake gitlab:check RAILS_ENV=production"

```

### Backing up GitLab manually
```
docker exec -i -t ID bash -c "cd /home/git/gitlab && sudo -u git -H bundle exec rake gitlab:backup:create RAILS_ENV=production"

```

### Restore GitLab
```
docker exec -i -t ID bash -c "cd /home/git/gitlab && sudo -u git -H bundle exec rake gitlab:backup:restore RAILS_ENV=production force=yes BACKUP=timestamp_of_backup"

```

### Rebuild authorized_keys file
```
docker exec -i -t ID bash -c "cd /home/git/gitlab && sudo -u git -H bundle exec rake gitlab:shell:setup RAILS_ENV=production"

```

## Environment Variables

* `REDIS_URL` - URL of the redis server
* `GITLAB_ROOT_PASSWORD` - GitLab root password
* `UNICORN_WORKER_PROCESSES` - number of Unicorn worker processes
* `GITLAB_HOST` - GitLab host
* `GITLAB_PORT` - GitLab port
* `GITLAB_HTTPS` - `true`/`false` if GitLab is running SSL encrypted
* `GITLAB_TIMEZONE` - GitLab timezone
* `GITLAB_EMAIL_FROM` - GitLab mails from address
* `GITLAB_EMAIL_SMTP_ADDRESS` - SMTP Server used by GitLab
* `GITLAB_EMAIL_SMTP_USERNAME` - SMTP Server username
* `GITLAB_EMAIL_SMTP_PASSWORD` - SMTP Server password
* `GITLAB_EMAIL_SMTP_PORT` - SMTP Port
* `GITLAB_EMAIL_SMTP_AUTHENTICATION` - SMTP Authentication Method (plain, login, ...)
* `GITLAB_EMAIL_SMTP_DOMAIN` - SMTP Server e-mail domain
* `GITLAB_EMAIL_DISPLAY_NAME` - E-Mail display name
* `GITLAB_EMAIL_REPLY_TO` - E-Mail reply to
* `GITLAB_DEFAULT_THEME` - default GitLab theme for new users
* `GITLAB_PROJECT_FEATURES_ISSUES` - `true`/`false` if issues should be enabled
* `GITLAB_PROJECT_FEATURES_MERGE_REQUEST` - `true`/`false` if merge requests should be enabled
* `GITLAB_PROJECT_FEATURES_WIKI` - `true`/`false` if wikis should be enabled
* `GITLAB_PROJECT_FEATURES_SNIPPETS` - `true`/`false` if snippets should be enabled
* `GITLAB_PROJECT_FEATURES_BUILDS` - `true`/`false` if builds should be enabled
* `GITLAB_DATABASE_SECRET_KEY` - GitLab database secret
* `GITLAB_BACKUP_KEEP_TIME` - how long GitLab backups should be kept
* `GITLAB_BACKUP_CRON` - crontable time (f.ex `20 * * * *` for the 20th minute of every hour)
* `DATABASE_USER` - database username
* `DATABASE_PASSWORD` - database password
* `DATABASE_NAME` - database name
* `DATABASE_HOST` - database host
* `CONTAINER_TIMEZONE` - timezone which should be used, default: `Europe/Zurich` ([see Wikipedia](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones))
* `GITLAB_PROJECT_FEATURES_CONTAINER_REGISTRY` - `true`/`false` if registry should be enabled 
* `GITLAB_INCOMING_EMAIL_ENABLED` - Allow users to comment on issues and merge requests by replying to notification emails.
* `GITLAB_INCOMING_EMAIL_ADDRESS` - The email address including the `%{key}` placeholder that will be replaced to reference the item being replied to. The placeholder can be omitted but if present, it must appear in the "user" part of the address (before the `@`).
* `GITLAB_INCOMING_EMAIL_USER` -  Email account username      
* `GITLAB_INCOMING_EMAIL_PASSWORD` - Email account password
* `GITLAB_INCOMING_EMAIL_IMAP_HOST` - IMAP server host
* `GITLAB_INCOMING_EMAIL_IMAP_PORT` - IMAP server port
* `GITLAB_INCOMING_EMAIL_IMAP_SSL` -  Whether the IMAP server uses SSL
* `GITLAB_INCOMING_EMAIL_IMAP_STARTTLS` - Whether the IMAP server uses StartTLS
* `GITLAB_INCOMING_EMAIL_IMAP_MAILBOX` - The mailbox where incoming mail will end up. Usually "inbox".
* `CONTAINER_REGISTRY_HOST` - Registry Server Host
* `CONTAINER_REGISTRY_PORT` - Registry Server Port
* `CONTAINER_REGISTRY_API_URL` - Registry Server API url
* `CONTAINER_REGISTRY_ISSUER` - This should be the same value as configured in Registry's issuer


## Volumes

* /etc/firstboot

## Exposed Ports

* 22
* 80

## Built

Because we are using several base images with recurring tasks in the Dockerfile, we are using a script to include commands. This script is available under [https://github.com/whatwedo/docker-base-images/blob/master/docker-builder.sh](https://github.com/whatwedo/docker-base-images/blob/master/docker-builder.sh)

## Bugs and Issues

### known limitations

* we're encrypting the traffic via a reverse proxy on the docker hosts, so there is no SSL configuration in this container
* there is only the backup directory as a volume available - otherwise we can't restore backups because of a bug
* we're using MariaDB only, so there is no option to configure PostgreSQL at the moment

If you have any problems with this image, feel free to open a new issue in our issue tracker [https://github.com/whatwedo/docker-base-images/issues](https://github.com/whatwedo/docker-base-images/issues)

## License

This image is licensed under the MIT License. The full license text is available under [https://github.com/whatwedo/docker-base-images/blob/master/LICENSE](https://github.com/whatwedo/docker-base-images/blob/master/LICENSE).

## Further information

There are a number of images we are using at [https://example.com/](whatwedo). Feel free to use them. More information about the other images are available in [our Github repo](https://github.com/whatwedo/docker-base-images).
