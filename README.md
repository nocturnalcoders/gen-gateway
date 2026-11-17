# gen-gateway

Caddy in front of everything GEN runs on the droplet. It is the only container with a
public port; the API, the website and the CMS are separate projects that it reaches over
the docker network `gen-gateway`.

| Hostname | Goes to |
|---|---|
| the site's hostname | website (`gen-web:3000`), API under `/api` (`gen-api:8000`) |
| hostnames in `API_HOSTS` | API only |
| `CMS_HOST` | Strapi (`gen-cms:1337`) |

## Run

```sh
cp .env.example .env      # set SITE_ADDRESS, API_HOSTS, CMS_HOST
docker compose up -d      # creates the gen-gateway network
```

Then start the projects behind it (`gen-backend/backend`: `docker compose up -d --build`).
Start the gateway first: the others expect its network to exist.

After editing `Caddyfile`: `docker compose exec gateway caddy reload --config /etc/caddy/Caddyfile`.
After editing `.env`: `docker compose up -d`.

A hostname whose project is not running answers 502 until it is.
