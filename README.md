# TeslaMate Grafana

<div align="center">

<img src="logo.svg" alt="TeslaMate Logo" width="200">

**Pre-configured Grafana Docker image for TeslaMate with 49+ dashboards**

[![Build Status](https://github.com/crstian19/teslamate-grafana/actions/workflows/build-image.yml/badge.svg)](https://github.com/crstian19/teslamate-grafana/actions/workflows/build-image.yml)
[![Grafana Version](https://img.shields.io/badge/Grafana-12.1.1-orange)](https://grafana.com/)
[![Docker Pulls](https://img.shields.io/badge/docker-pull-blue)](https://github.com/crstian19/teslamate-grafana/pkgs/container/teslamate-grafana)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

[Quick Start](#-quick-start) • [Dashboards](#-dashboards) • [Configuration](#%EF%B8%8F-configuration) • [Docker Compose](#-docker-compose-example)

</div>

---

## 📋 Overview

A production-ready Grafana Docker image specifically designed for [TeslaMate](https://github.com/teslamate-org/teslamate), featuring:

- ✅ **49 pre-installed dashboards** (19 official + 30 community-contributed)
- 🔄 **Automatic weekly sync** with TeslaMate official dashboards
- 🤖 **Automated Grafana updates** via Renovate bot
- 🌐 **Multi-architecture support** (amd64, arm64, arm/v7)
- 🎨 **TeslaMate branding** (logo, favicon, icons)
- 📦 **Zero configuration required** - works out of the box

### Why this image?

The official TeslaMate repository often ships with outdated Grafana versions. This image ensures you always have:
- The latest stable Grafana release
- All official dashboards synchronized automatically
- Additional community dashboards for enhanced monitoring
- Multi-architecture support for all platforms

---

## 🚀 Quick Start

### Pull the image

```bash
docker pull ghcr.io/crstian19/teslamate-grafana:latest
```

### Access Grafana

Open http://localhost:3000 in your browser.

**Default credentials:**
- Username: `admin`
- Password: `admin`

---

## 📊 Dashboards

This image includes **49 dashboards** organized in 5 categories:

### 🔧 Official TeslaMate Dashboards (19)
Synchronized weekly from [teslamate-org/teslamate](https://github.com/teslamate-org/teslamate):

<table>
<tr>
<td width="50%">

- Battery Health
- Charge Level
- Charges
- Charging Stats
- Database Information
- Drives
- Drive Stats
- Efficiency
- Locations
- Mileage

</td>
<td width="50%">

- Overview
- Projected Range
- States
- Statistics
- Timeline
- Trip
- Updates
- Vampire Drain
- Visited

</td>
</tr>
</table>

### 🏠 Internal Dashboards (3)
- Charge Details
- Drive Details
- **Home** (default dashboard)

### 📈 Reports (1)
- Drives - Dutch Tax

### ⚡ CarlosCuezva Collection (12)
Enhanced versions of official dashboards:

<table>
<tr>
<td width="50%">

- Battery Health v2
- Charges v2
- Charging Costs Stats
- Charging Curves
- Charging Tops
- Current Charge View

</td>
<td width="50%">

- Drives v2
- Drive Tops
- Locations v2
- Overview v2
- States v2
- Tire Pressure

</td>
</tr>
</table>

### 🔬 Jheredia Collection (14)
Advanced analytics and specialized dashboards:

<table>
<tr>
<td width="50%">

- Amortization Tracker
- Charging Costs Stats
- Charging Curve Stats
- Continuous Trips
- Current Charge View
- Current Drive View
- Current State

</td>
<td width="50%">

- DC Charging Curves by Carrier
- Incomplete Data
- Mileage Stats
- Range Degradation
- Speed Rates
- Speed & Temperature
- Tracking Drives

</td>
</tr>
</table>

---

## ⚙️ Configuration

### Required Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `DATABASE_HOST` | PostgreSQL hostname | - |
| `DATABASE_USER` | Database username | - |
| `DATABASE_PASS` | Database password | - |
| `DATABASE_NAME` | Database name | - |

### Optional Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `DATABASE_PORT` | PostgreSQL port | `5432` |
| `DATABASE_SSL_MODE` | SSL mode for PostgreSQL | `disable` |
| `GF_SECURITY_ADMIN_USER` | Grafana admin username | `admin` |
| `GF_SECURITY_ADMIN_PASSWORD` | Grafana admin password | `admin` |

### Pre-configured Settings

This image comes with optimized Grafana settings:

- 🔒 Anonymous access disabled
- 🌍 Browser locale detection enabled
- 🎨 TeslaMate branding applied
- 📊 Default home dashboard set to Internal/Home
- 🚫 User sign-up disabled
- 🔕 Unified alerting disabled
- 📈 Analytics reporting disabled


---

## 🔄 Automatic Updates

### Dashboard Synchronization

A [GitHub Action](.github/workflows/sync-dashboards.yml) runs every **Monday at 2 AM UTC** to:

1. Check for new dashboard updates from TeslaMate official repository
2. Automatically create a Pull Request if changes are detected
3. Preserve custom dashboards (CarlosCuezva and Jheredia collections)

### Grafana Version Updates

[Renovate bot](renovate.json) monitors Grafana releases and automatically:

1. Creates PRs when new Grafana versions are available
2. Separates updates by type (major, minor, patch)
3. Includes release notes and testing commands
4. Requires manual review before merging

---


## 🛠️ Dashboard Management

### Backup Dashboards

Use the included [dashboards.sh](dashboards.sh) script:

```bash
export GRAFANA_API_TOKEN="your-api-token"
export URL="http://localhost:3000"
./dashboards.sh backup
```

### Restore Dashboards

```bash
export GRAFANA_API_TOKEN="your-api-token"
export URL="http://localhost:3000"
./dashboards.sh restore
```

---

## 📦 Available Tags

| Tag | Description |
|-----|-------------|
| `latest` | Latest stable build from main branch |
| `12.1.1` | Specific Grafana version |
| `main-<sha>` | Commit-specific build |

Example:
```bash
docker pull ghcr.io/crstian19/teslamate-grafana:latest
docker pull ghcr.io/crstian19/teslamate-grafana:12.1.1
```

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### Adding Custom Dashboards

1. Fork this repository
2. Add your dashboard JSON to `dashboards/` or create a new subfolder
3. Update `dashboards.yml` to include your dashboard folder
4. Update this README to list your dashboard
5. Submit a Pull Request

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Credits

### Official TeslaMate Project
- [TeslaMate](https://github.com/teslamate-org/teslamate) - The amazing TeslaMate project and official dashboards
- [TeslaMate Documentation](https://docs.teslamate.org/) - Comprehensive TeslaMate documentation

### Community Dashboard Contributors
- **CarlosCuezva** - Enhanced dashboard collection with improved visualizations
- **Jheredia** - Advanced analytics and specialized monitoring dashboards

### Technologies
- [Grafana](https://grafana.com/) - Open-source analytics and monitoring platform
- [PostgreSQL](https://www.postgresql.org/) - Powerful open-source database
- [Docker](https://www.docker.com/) - Containerization platform
- [GitHub Actions](https://github.com/features/actions) - CI/CD automation

---

## 📞 Support

- **TeslaMate Issues**: [teslamate-org/teslamate/issues](https://github.com/teslamate-org/teslamate/issues)
- **This Image Issues**: [crstian19/teslamate-grafana/issues](https://github.com/crstian19/teslamate-grafana/issues)
- **TeslaMate Community**: [TeslaMate Forum](https://community.teslamate.org/)

---

<div align="center">

**⭐ If this project helped you, consider giving it a star!**

Made with ❤️ for the TeslaMate community

</div>
