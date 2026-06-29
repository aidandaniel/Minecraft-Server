# Minecraft Server with Monitoring

A complete Minecraft server hosting solution using Docker Compose with integrated monitoring stack (Prometheus, Grafana, cAdvisor) to track hardware metrics and visualize server performance.

## Architecture

This stack consists of:
- **Minecraft Server** - The game server running on port 25565
- **cAdvisor** - Collects container metrics on port 8080
- **Prometheus** - Time-series database scraping metrics on port 9090
- **Grafana** - Visualization dashboard on port 3000

The monitoring stack tracks CPU, memory, network, and disk I/O metrics for the Minecraft server and all containers.

## Quick Start

### Prerequisites
- Docker and Docker Compose installed
- At least 2GB of free disk space
- Ports 25565, 3000, 8080, 9090 available

### Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/aidandaniel/Minecraft-Server.git
   cd Minecraft-Server
   ```

2. **Configure environment variables:**
   ```bash
   cp .env.example .env
   # Edit .env and set a secure Grafana admin password
   nano .env
   ```

3. **Start all services:**
   ```bash
   docker-compose up -d
   ```

4. **Verify services are running:**
   ```bash
   docker-compose ps
   ```

## Accessing Services

| Service | URL | Default Credentials |
|---------|-----|---------------------|
| Minecraft Server | `minecraft.local:25565` | N/A - Add to server list in game |
| Grafana Dashboard | `http://localhost:3000` | admin / password from `.env` |
| Prometheus | `http://localhost:9090` | N/A - Metrics only |
| cAdvisor | `http://localhost:8080` | N/A - Metrics only |

## Configuration

### Minecraft Server
Edit environment variables in `docker-compose.yml` under the `mc` service. See [itzg/minecraft-server](https://github.com/itzg/docker-minecraft-server) documentation for available options.

### Prometheus
Modify scrape intervals and jobs in `prometheus/prometheus.yml`.

### Grafana
After logging in, create dashboards using Prometheus as a data source. Import community dashboards or create custom ones using PromQL queries.

## Useful Commands

```bash
# View logs
docker-compose logs -f minecraft-server
docker-compose logs -f grafana

# Stop all services
docker-compose down

# Remove volumes (WARNING: deletes data)
docker-compose down -v

# Restart a service
docker-compose restart grafana

# Execute command in container
docker-compose exec minecraft-server say "Server message"
```

## Security Notes

- Always change the Grafana admin password in `.env` before deploying
- Keep the `.env` file out of version control (it's in `.gitignore`)
- Consider using firewall rules to restrict access to monitoring ports
- Regularly update Docker images for security patches

## Troubleshooting

### Services won't start
```bash
docker-compose logs
```
Check logs to see detailed error messages.

### Prometheus can't connect to cAdvisor
Ensure all services are on the `monitoring` network and the container names match in `prometheus/prometheus.yml`.

### Grafana dashboard shows no data
- Wait 1-2 minutes for Prometheus to collect initial metrics
- Check Prometheus targets at `http://localhost:9090/targets`
- Verify cAdvisor is healthy at `http://localhost:8080`

## Performance Tips

- Increase Minecraft server JVM memory: Add `MEMORY=2G` to the `mc` service environment
- Adjust Prometheus scrape interval in `prometheus/prometheus.yml` for less frequent collection (trades real-time data for reduced overhead)
- Monitor dashboard regularly to identify resource bottlenecks

## License

MIT
