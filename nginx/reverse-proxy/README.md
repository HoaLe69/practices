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

## Gotcha: proxy loop → "Request Header Or Cookie Too Large"

If you set `proxy_pass http://localhost:80;` inside the Nginx container, you get a
`400 Request Header Or Cookie Too Large` — even though your browser sent nothing big.

**Why:** `localhost` inside a container refers to *that same container*, not the host
and not the `backend` service. So Nginx proxies the request back into its own `server`
block, which proxies to `localhost` again → an infinite proxy loop. Because
`proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;` appends the client IP on
every hop, that header grows on each loop iteration until it exceeds Nginx's header
buffer (`large_client_header_buffers`, default `4 8k`). Nginx then rejects the request.
The error is a *symptom of the loop*, not a real oversized cookie.

**Fix:** proxy to the backend by its Docker service name, which resolves to the *other*
container:

```nginx
proxy_pass http://backend:80;
```

To reach a service running on the **Windows host** (a different goal), use
`http://host.docker.internal:<port>` instead.

## What I learned

- (fill in after doing the practice)

## References

- https://nginx.org/en/docs/http/ngx_http_proxy_module.html
- https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/
