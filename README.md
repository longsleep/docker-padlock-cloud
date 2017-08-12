# docker-padlock-cloud

A simple Dockerfile to run a self hosted [padlock-cloud](https://github.com/MaKleSoft/padlock-cloud) server using docker. For
help on getting started with docker see the [official getting started guide][0].

## Building docker-padlock-cloud

Running this will build you a docker image with the latest version of 
padlock-cloud.

```
git clone https://github.com/longsleep/docker-padlock-cloud
cd docker-padlock-cloud
docker build -t longsleep/padlock-cloud .
```

## Running docker-padlock-cloud

This docker image exposes the padlock-cloud server to be used with a reverse
proxy to provide external access and TLS. In addition padlock-cloud requires
a SMTP server to send email. This SMTP server needs to be TLS encrypted with
a public validateable certificate and use authentication.

The following example assumes you have created the folder `/srv/padlock-cloud`
and made sure that padlock cloud inside docker can write to it. Skip the `-v`
parameters to store data inside the container (not recommended). 

```
sudo docker run --rm=true \
    -p=127.0.0.1:8070:3000 \
    -v=/srv/padlock-cloud:/padlock-db \
    -v=/srv/padlock-cloud:/padlock-log \
    --name=padlock-cloud \
    longsleep/padlock-cloud \
    --email-server smtp.gmail.com \
    --email-port 587 --email-user yourgmail \
    --email-password yourgmailpw \
     runserver \
    --base-url https://host.to.your.padlock.local
```

This runs docker-padlock-cloud in a temporary container in foreground. To stop it,
just press CTRL+C. It runs padlock-cloud on `127.0.0.1:8070`. Use this in your 
TLS  reverse proxy and make sure the value for `--base-url` matches your setup.

[0]: http://www.docker.io/gettingstarted/

