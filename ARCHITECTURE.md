# Architecture Plan — Status Page Workshop

## Running Platform
- Ubuntu 22.04 VM (single-node for demo).
- Application deployed under `/opt/status-page`.
- Services managed with **systemd** (Gunicorn, Scheduler, RQ worker).

## Databases
- **PostgreSQL 14**: stores application data.
- **Redis 6**: used for caching and background jobs.
- For demo: both run in Docker containers on the same VM.
- In production: separate VMs or managed DB services.

## CI/CD
- GitHub Actions workflow triggered on push to `main`.
- SSH deploy to the VM: pulls repo, runs `upgrade.sh`, restarts services.

## High Availability
- Demo: single-node (no HA).
- Future: Load balancer + multiple app servers; managed Postgres with replication; Redis cluster.

## Monitoring
- External uptime check (e.g., https://uptimerobot.com).
- Local health checks: `systemctl is-active` for services.

## Logging
- Application and workers: `journalctl` for systemd units.
- Web server: Nginx access/error logs (rotated automatically).

## Security
- HTTPS via Nginx (self-signed cert for demo; Let’s Encrypt in production).
- UFW firewall: allow 22 (SSH) and 443 (HTTPS). Port 8000 open only during testing.
- SSH key authentication, no password login.
- Dedicated Linux system user `status-page`.

