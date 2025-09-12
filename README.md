# MeshCommander

A web-based Intel AMT/vPro management interface for remote computer administration with support for both standalone and Traefik reverse proxy deployments.

## Services Included

- **MeshCommander**: Web-based Intel AMT/vPro management console
- **Resource Limits**: CPU (0.25 cores) and Memory (50MB) constraints for efficiency

## Quick Start

1. **Copy environment template:**
```bash
cp .env.example .env
```

2. **Configure your domain:**
```bash
# Edit .env and set your domain
TRAEFIK_HOST=mc.yourdomain.com
```

## Deployment Options

### Standalone Deployment
Direct access via host ports:
```bash
docker compose up -d
```
Access at: http://localhost:3000 (or configured TRAEFIK_PORT)

### Traefik Integration
Deploy with reverse proxy integration:
```bash
docker compose -f docker-compose.yml -f docker-compose.traefik.yml up -d
```
Access at: https://mc.yourdomain.com (with automatic TLS)

## Portainer Deployment

When deploying from GitHub in Portainer:

1. **Compose path**: `docker-compose.yml`
2. **Additional paths**: Click "Add file" and enter `docker-compose.traefik.yml`

![Portainer Configuration](docs/portainer.png)

This configures Portainer to merge both files, equivalent to:
```bash
docker compose -f docker-compose.yml -f docker-compose.traefik.yml up -d
```

## Prerequisites

### Standalone
- Docker & Docker Compose

### Traefik Integration  
- Docker & Docker Compose
- External `traefik` network
- Traefik instance with Let's Encrypt configured

## Configuration

### Environment Variables
See `.env.example` for all available configuration options.

**Key Settings:**
- `MC_VER`: MeshCommander version (default: v0.8.8b-p0)
- `TRAEFIK_HOST`: Your domain name
- `TRAEFIK_PORT`: Service port (default: 3000)

## About MeshCommander

MeshCommander is a tool for managing Intel AMT/vPro enabled computers. It provides:

- **Remote Desktop**: KVM (Keyboard, Video, Mouse) control
- **Power Management**: Remote power on/off, reset
- **File Transfer**: Upload/download files to remote systems
- **Terminal Access**: SOL (Serial over LAN) console
- **Hardware Info**: System information and monitoring

## Intel AMT Requirements

To use MeshCommander effectively:

1. **Intel AMT/vPro Hardware**: Target computers must have Intel AMT/vPro support
2. **AMT Configuration**: Intel AMT must be enabled and configured on target systems
3. **Network Access**: MeshCommander needs network connectivity to managed computers
4. **AMT Credentials**: You'll need AMT username/password for each managed system

## Security Considerations

⚠️ **Important Security Notes:**

1. **Network Isolation**: Consider running on isolated management network
2. **Access Control**: Use Traefik authentication if exposing publicly
3. **AMT Security**: Ensure AMT passwords are strong and rotated regularly
4. **Resource Limits**: Built-in CPU/memory limits prevent resource abuse

## Resource Management

This container is configured with resource constraints:
- **CPU Limit**: 25% of one CPU core
- **Memory Limit**: 50MB
- **CPU Reservation**: 10% of one CPU core guaranteed
- **Memory Reservation**: 8MB guaranteed

## Troubleshooting

**Connection Issues:**
- Verify target systems have Intel AMT enabled
- Check network connectivity to managed computers
- Ensure AMT credentials are correct

**Access Issues:**
- For standalone: Check port 3000 is accessible
- For Traefik: Verify domain DNS and SSL certificate
- Check Docker logs: `docker compose logs meshcommander`

## Data Persistence

MeshCommander is stateless - no persistent data volumes required. Configuration is done through the web interface per session.