# Marstek BLE Gateway

> ## 🚫 **PROJECT DEPRECATED**
>
> **This project is no longer maintained.** Please use the superior approach below instead.
>
> ### Why This Project Is Abandoned
>
> This ESPHome gateway package required a dedicated ESP32 device running custom firmware to bridge BLE communication from Marstek batteries to Home Assistant. While functional, this approach had several limitations:
>
> - **Single battery limitation**: Each gateway typically supported one battery
> - **Complex setup**: Required custom ESPHome configuration and package management
> - **Limited auto-discovery**: Manual configuration was needed for battery pairing
> - **Dedicated hardware**: Required a separate ESP32 just for this integration
>
> ### ✅ Recommended Alternative
>
> **Use [ha-marstek-ble](https://github.com/jaapp/ha-marstek-ble)** - A native Home Assistant integration that connects directly via BLE.
>
> **Key advantages:**
> - **Multiple batteries**: Connect multiple Marstek batteries through a single ESP32 Bluetooth Proxy
> - **Better auto-discovery**: Batteries are automatically discovered and added to Home Assistant
> - **Simpler setup**: Native Home Assistant integration with standard BLE proxy
> - **Reusable hardware**: Your ESP32 can serve as a general-purpose [ESPHome Bluetooth Proxy](https://esphome.io/components/bluetooth_proxy) for all BLE devices
> - **More reliable**: Direct BLE integration without custom gateway firmware
>
> ### Migration Steps
>
> 1. **Set up an ESPHome Bluetooth Proxy** on your ESP32:
>    - Follow the [ESPHome Bluetooth Proxy guide](https://esphome.io/components/bluetooth_proxy)
>    - Any ESP32 device can serve as a proxy (no custom firmware needed)
>
> 2. **Install the ha-marstek-ble integration**:
>    - Install from [GitHub](https://github.com/jaapp/ha-marstek-ble)
>    - Batteries will auto-discover through your BLE proxy
>
> 3. **Remove this gateway**: The dedicated ESPHome gateway is no longer needed
>
> ---
>
> ## ⚠️ Legacy Documentation Below
>
> The documentation below is preserved for historical reference only.
>
> ---

> ⚠️ **EXPERIMENTAL** - Use at your own risk

ESPHome package for Marstek Venus E energy storage systems. Integrates with Home Assistant without cloud connectivity.

## Usage as Package

Add to your ESPHome device configuration:

```yaml
# your-device.yaml
esphome:
  name: my-marstek-gateway
  friendly_name: My Marstek Gateway

esp32:
  board: esp32-s3-devkitc-1
  variant: esp32s3
  framework:
    type: esp-idf

logger:
  level: INFO

api:
  encryption:
    key: "YOUR_32_BYTE_BASE64_ENCRYPTION_KEY_HERE"

ota:
  - platform: esphome
    password: "YOUR_OTA_PASSWORD_HERE"

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

# Time component - REQUIRED
time:
  - platform: homeassistant
    id: ha_time

# Import the Marstek BLE Gateway package
packages:
  marstek_ble: github://jaapp/marstek-ble-gateway/marstek-ble-gateway.yaml@main
```

### Setup Steps

1. **Create secrets.yaml**:
   ```yaml
   wifi_ssid: "YourWiFiName"
   wifi_password: "YourWiFiPassword"
   ```

2. **Configure your device**:
   - Replace `YOUR_32_BYTE_BASE64_ENCRYPTION_KEY_HERE` with a random 32-byte base64 key
   - Replace `YOUR_OTA_PASSWORD_HERE` with a secure password
   - Generate encryption key with: `openssl rand -base64 32`

3. **Flash to ESP32**:
   ```bash
   esphome run your-device.yaml
   ```

4. **Add to Home Assistant**:
   - Go to Settings → Devices & Services
   - Add ESPHome integration
   - Device will auto-discover nearby Marstek devices starting with "MST"

## Standalone Usage

See `example.yaml` for a complete working configuration.

### 3. Features

- **Real-time monitoring**: Battery voltage, current, SOC, SOH, temperature
- **Power control**: Set power limits, EPS mode, output control
- **Energy tracking**: Daily energy in/out, integration with HA Energy Dashboard
- **Device info**: Firmware version, MAC address, network status
- **Auto-discovery**: Automatically finds Marstek devices via BLE scanning
- **Local operation**: No cloud connectivity required

### 4. Troubleshooting

- **Device not found**: Ensure Marstek device BLE is enabled and within range
- **Connection issues**: Check ESP32 is close enough to Marstek device (< 10m)
- **WiFi problems**: Verify credentials in secrets.yaml

## Attribution

Based on excellent reverse engineering work from:
- [esphome-b2500](https://github.com/tomquist/esphome-b2500) by @tomquist
- [hm2500pub](https://github.com/noone2k/hm2500pub) by @noone2k  
- [marstek-venus-monitor](https://github.com/rweijnen/marstek-venus-monitor) by @rweijnen

## Disclaimer

This is experimental software created through reverse engineering. Not affiliated with Marstek Energy. Use at your own risk. The authors are not responsible for any damage to your equipment.