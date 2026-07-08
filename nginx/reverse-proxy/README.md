# Nginx Reverse Proxy

> Put Nginx in front of a backend service and forward requests to it with proper headers.

## Goal

Understand the core reverse-proxy building blocks:
- `proxy_pass` — where requests get forwarded
- `proxy_set_header` — passing the original request info (`Host`, client IP, protocol)
- service discovery via Docker's internal DNS (`backend` resolves by service name)

## Prerequisites

- Docker Desktop running

## How to run

```bash
docker compose up -d
```

Then make requests to Nginx (port 8080 on your host):

```bash
curl http://localhost:8080/          # proxied to the backend (whoami)
curl http://localhost:8080/healthz   # served directly by Nginx
```

Open http://localhost:8080 in a browser too. The `whoami` backend prints the
request as it *arrived at the backend* — look for the `X-Forwarded-For`,
`X-Real-Ip`, and `Host` headers you set in `nginx.conf`.

### Experiment

1. Comment out the `proxy_set_header` lines, `docker compose restart proxy`,
   and re-request — notice the forwarded headers disappear.
2. Change `proxy_pass` to a bad host and watch the `502 Bad Gateway`.
3. Add a second `location /api/` that proxies somewhere else.

Tear down when done:

```bash
docker compose down
```

## What I learned

- (fill in after doing the practice)

## References

- https://nginx.org/en/docs/http/ngx_http_proxy_module.html
- https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/
