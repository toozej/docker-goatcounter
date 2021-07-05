# baethon/goatcounter

Unofficial Docker image for [goatcounter](https://github.com/zgoat/goatcounter) - simple web statistics

## How to use this image

### GitHub Container Registry
```
docker pull ghcr.io/toozej/docker-goatcounter:latest
```

### Local Build
```
git clone https://github.com/toozej/docker-goatcounter.git
cd docker-goatcounter/v2.0
docker build -t goatcounter:2.0 .
```

```bash
docker run --name goatcounter \
  -e GOATCOUNTER_DOMAIN=stats.domain.com \
  -e GOATCOUNTER_EMAIL=admin@domain.com \
  -e GOATCOUNTER_PASSWORD=admin1234 \
  -p 127.0.0.0:8080:8080 \
  goatcounter:2.0
```

This command will start a single instance with pre-configured `stats.domain.com` site.

`GOATCOUNTER_DOMAIN` and `GOATCOUNTER_EMAIL` are mandatory.

## Environment Variables

### `GOATCOUNTER_DOMAIN`

This mandatory environment variable is used to create the initial site.

### `GOATCOUNTER_EMAIL`

This mandatory environment variable defines the e-mail address of the admin user.

It's used to create the initial site and is passed as an `-auth` option to the `serve` command.

### `GOATCOUNTER_PASSWORD`

_Available since v1.4_.

This mandatory environment variable defines the password of the admin user.

### `GOATCOUNTER_SMTP`

This optional environment variable defines the SMTP server (e.g., `smtp://user:pass@server.com:587`) which will be used by the server.

_Default:_ `stdout` - print email contents to stdout

### `GOATCOUNTER_DB`

This optional environment variable defines the location of the database. By default, the server will use SQLite database which is recommended solution.

It's possible to use the Postgres DB however, the image was not tested against it.

Don't change this value unless you know what you're doing.

_Default:_ `sqlite:///goatcounter/db/goatconter.sqlite3`

## Troubleshooting

### The server displays migration info

During startup, the container will try to execute all available migrations. What you see, is the output of `goatcounter db migrate` command.

### line X: GOATCOUNTER_*: unbound variable

You forgot to set one of the mandatory env variables.

### zdb.TX fn: cname: already exists.

The entrypoint script tries to create the initial site on every container startup. Once it's created, the `goatcounter db create` command will report this error. It's safe to ignore it.

### zdb.TX fn: password: must be between 8 and 50 bytes.
The provided password must be between 8 and 50 bytes. Choose an appropriate password.
