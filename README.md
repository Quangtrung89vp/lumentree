# Lumentree Solar Inverter — Home Assistant Integration

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-orange.svg)](https://github.com/custom-components/hacs)
[![GitHub release](https://img.shields.io/github/release/ngoviet/lumentreeHA.svg)](https://github.com/ngoviet/lumentreeHA/releases)
[![GitHub stars](https://img.shields.io/github/stars/ngoviet/lumentreeHA.svg)](https://github.com/ngoviet/lumentreeHA/stargazers)
[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-ffdd00?style=flat&logo=buy-me-a-coffee&logoColor=black)](https://buymeacoffee.com/ngoviet)
[![GitHub Sponsors](https://img.shields.io/badge/GitHub%20Sponsors-ff69b4?style=flat&logo=githubsponsors&logoColor=white)](https://github.com/sponsors/ngoviet)

<a href="https://my.home-assistant.io/redirect/hacs_repository/?owner=ngoviet&repository=lumentreeHA&category=integration" class="my badge" target="_blank"><img src="https://my.home-assistant.io/badges/hacs_repository.svg" alt="Open this repository in HACS" width="200" height="36"></a>

>Chào mừng bạn đến với Bộ tích hợp dành cho Lumentree Của Quang Trung liên Hệ Zalo (0973941273)

---

High-performance **Home Assistant custom integration** for **Lumentree hybrid solar inverters** (SUNT series). Real-time monitoring via **MQTT (Modbus)**, historical energy statistics via **HTTP API**, and comprehensive battery management. Track solar PV generation, grid import/export, battery SOC, and calculate electricity cost savings in VND.

---

## Screenshots

### Sensors Overview
![Sensors Entity Overview](images/sensors_entity_overview.jpg)

### Dashboard — Energy Charts (24h)
![Dashboard Energy Charts 24h](images/dashboard_energy_charts_24h.jpg)

### Dashboard — Statistics Summary
![Dashboard Statistics Summary](images/dashboard_statistics_summary.jpg)

---

## Features

### Real-time Data (MQTT — 5s polling)
- **PV Power**: Solar generation (W) — PV1 + PV2
- **Battery Management**: Power, voltage, current, SOC (%), status (Charging/Discharging)
- **Grid Power**: Import/export power + status
- **Load Power**: Total consumption monitoring
- **AC Output**: Voltage, frequency, power, apparent power
- **AC Input**: Voltage, frequency, power
- **Device**: Temperature, online status, UPS mode, serial number
- **Battery Cells**: Individual cell voltage monitoring

### Daily Statistics (HTTP API — 5min refresh)
- PV Generation, Battery Charge/Discharge, Grid Import, Load Consumption
- Essential Load, Total Load, Energy Saved (kWh), Cost Savings (VND)
- **288-point 5-minute series** for detailed charts

### Monthly Statistics
- 9 metrics: PV, Charge, Discharge, Grid, Load, Essential, Total Load, Saved (kWh), Savings (VND)
- **Daily arrays** for month-view charts (1-31 days)

### Yearly Statistics
- 9 metrics with **monthly arrays** for year-view charts (1-12 months)
- Historical data across multiple years via disk cache

### Lifetime/Total Statistics
- Cumulative totals across all available years
- Auto-aggregated from yearly data

### Performance
- **40-50% faster** parsing with struct caching
- **3x faster** API calls with concurrent requests
- **20-30% lower** memory with `__slots__`

> 💡 **Người dùng báo cáo tiết kiệm 200,000-500,000 VND/tháng tiền điện** nhờ theo dõi chính xác sản lượng điện mặt trời và tiêu thụ. Nếu integration này giúp ích cho bạn, [ủng hộ tôi một cốc cà phê](https://buymeacoffee.com/ngoviet) ☕

---

## Requirements

- **Home Assistant**: 2023.1+
- **Python**: 3.9+
- **Dependencies**: aiohttp>=3.8.0, paho-mqtt>=1.6.0, crcmod>=1.7
- **Network**: Internet (API + MQTT to `lesvr.suntcn.com`)

---

## Installation

### HACS (Recommended)
1. Open **HACS** → **Integrations** → **Custom Repositories**
2. Add `https://github.com/ngoviet/lumentreeHA` (Integration type)
3. Install **Lumentree Inverter** → Restart HA

### Manual
1. Download [latest release](https://github.com/ngoviet/lumentreeHA/releases)
2. Extract to `custom_components/lumentree/` in your HA config
3. Restart Home Assistant

---

## Configuration

1. **Settings** → **Devices & Services** → **Add Integration**
2. Search **Lumentree Inverter**
3. Enter **Device ID** (format: `H240909079` — found on device label or Lumentree app)
4. Follow setup wizard

---

## Available Entities

### Real-time Sensors (26 entities)
| Category | Sensors |
|----------|---------|
| Power | PV1, PV2, PV Total, Battery, Grid, Load, AC Output, AC Input, Total Load |
| Voltage | Battery, PV1, PV2, Grid, AC Output, AC Input |
| Current | Battery |
| Frequency | AC Output, AC Input |
| Status | Battery SOC (%), Battery Status, Grid Status, Battery Type, UPS Mode, Master/Slave |
| Info | Device Temperature, MQTT Device SN, Battery Cell Info |

### Statistics Sensors (28 entities)
| Period | Metrics | Refresh |
|--------|---------|---------|
| Daily (7) | PV, Charge, Discharge, Grid, Load, Essential, Total Load | 5 min |
| Monthly (7) | Same + Saved (kWh) + Savings (VND) | 5 min |
| Yearly (7) | Same + monthly arrays for charting | 5 min |
| Total (7) | Lifetime cumulative sums | 5 min |

### Binary Sensors (2 entities)
- Online Status, UPS Mode

---

## Troubleshooting

### No entities after setup
- Restart Home Assistant
- Check logs: `custom_components.lumentree` for errors
- Verify network access to `lesvr.suntcn.com:1886` (MQTT) and `lesvr.suntcn.com:80` (HTTP)

### Sensors stuck / not updating
- MQTT sensors freeze → integration auto-reconnects within 2 minutes
- Stats sensors → check if daily coordinator is updating

### Enable debug logging
```yaml
logger:
  logs:
    custom_components.lumentree: debug
```

---



