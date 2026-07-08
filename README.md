# Practices

A personal monorepo of hands-on practices across infrastructure, DevOps, networking,
programming, and software design.

## Organization

Each **domain** has its own top-level folder. Inside, practices are grouped by
sub-category, and **every individual practice lives in its own folder with a `README.md`**
describing what it demonstrates, how to run it, and what was learned.

| Domain | Description |
| --- | --- |
| [infrastructure/](./infrastructure) | IaC and provisioning — Terraform, Ansible, Kubernetes |
| [nginx/](./nginx) | Reverse proxy, load balancing, TLS/SSL, rate limiting |
| [docker/](./docker) | Images, Compose stacks, container networking |
| [networking/](./networking) | TCP/IP, DNS, subnetting — labs and theory notes |
| [programming/](./programming) | Language practice — Python, Go, JavaScript, Rust |
| [design-patterns/](./design-patterns) | Creational, structural, behavioral patterns |

## Conventions

- **One practice = one folder** with its own `README.md`.
- Folder names are **lowercase, hyphen-separated** (`rate-limiting`, not `Rate Limiting`).
- Theory notes live alongside labs as `notes.md`.
- Starting something new? Copy [`_templates/practice-README.md`](./_templates/practice-README.md).

## Getting started

```bash
git clone <this-repo>
cd practices
```

Browse to any practice folder and follow its `README.md`.
