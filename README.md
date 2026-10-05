# skarverakk-docker

The Skarveråkk 2027 concept demo (a React single-page app, built output only) served from an nginx container. It is a demo by Laden AS, not the official festival site ([skarverakk.com](https://skarverakk.com/)). Unknown paths fall back to index.html so app routes work. The app source is in [kim-laden/skarverakk-demo](https://github.com/kim-laden/skarverakk-demo).

Live Docker copy: [https://laden.no/docker/skarverakk/](https://laden.no/docker/skarverakk/)

Copyright Laden AS (Org.nr. 937 285 833). The code is open source under the MIT License. See [LICENSE](LICENSE). The Laden name, mark and brand are trademarks of Laden AS and are not given away by that licence.

## What is here

- `docker-compose.yml` runs one `web` service (nginx 1.27 alpine), read-only, with `no-new-privileges`.
- `Dockerfile` copies `nginx.conf` and the `site/` folder into the image.
- `nginx.conf` serves only `/docker/skarverakk/`. Every other path returns 404.
- `site/docker/skarverakk/` is the site exactly as the live container serves it.
- `deploy/compose.live.yml` and `deploy/Dockerfile.live` are the files the live stack on the Laden VPS uses (`/opt/skarverakk-docker`). There the HTML sits in the external named volume `skarverakk_site` instead of being baked into the image.

## Run it

```bash
cp .env.example .env   # optional, only to change the port
docker compose up -d --build
```

Open [http://127.0.0.1:18082/docker/skarverakk/](http://127.0.0.1:18082/docker/skarverakk/). 

The port is `18082` on `127.0.0.1` by default. Change `HOST_PORT` (and `BIND_ADDR`) in `.env` to use something else.

Stop it with `docker compose down`.

## Host proxy

The container speaks plain HTTP and only listens on localhost. On the live server the host web server (Apache on the Laden VPS) owns the domain and the HTTPS certificate and proxies `https://laden.no/docker/skarverakk/` to `http://127.0.0.1:18082/docker/skarverakk/`. Put your own Nginx, Apache or Caddy in front in the same way if you run this on a public host. Do not publish the port to `0.0.0.0` without a proxy in front.

The Skarveråkk name and logo, artist names and artist photos belong to the festival and the artists. The MIT License does not cover them.

## Not in this repo

No secrets. This stack needs none: no `.env` with real values, no keys, no database.
