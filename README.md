# DS3231 Real Time Clock (RTC) Component for ESPHome

This repository provides a custom ESPHome component for the DS3231 Real Time Clock (RTC) module, including support for its integrated temperature sensor. The DS3231 is a highly accurate I2C RTC with an integrated temperature-compensated crystal oscillator (TCXO) and crystal. It is widely used in embedded and IoT projects for precise timekeeping, even during power outages (with a backup battery).

## Features

- Accurate real-time clock (RTC) integration for ESPHome
- I2C communication
- Automatic time synchronization with ESPHome
- Exposes the DS3231's built-in temperature sensor as a standard ESPHome sensor
- Actions to manually read from or write to the RTC via ESPHome automations

## What is the DS3231?

The DS3231 is a low-cost, extremely accurate I2C real-time clock (RTC) with an integrated temperature-compensated crystal oscillator (TCXO) and crystal. The device incorporates a battery input and maintains accurate timekeeping when main power to the device is interrupted. It also features a built-in digital temperature sensor, which can be read out via I2C.

## Installation

1. **Copy the `ds3231` directory** into your ESPHome project's custom components folder (usually inside `esphome/` or your project root).
2. Make sure your ESPHome device is connected to the DS3231 module via I2C (SDA/SCL).
3. Add the custom component to your ESPHome YAML configuration as shown below.

## Example ESPHome Configuration

```yaml
esphome:
    name: my_ds3231_clock
    includes:
        - ds3231/ds3231.h
        - ds3231/ds3231.cpp

external_components:
    - source: github://<your-github-username>/DS3231-RTC-component-for-ESPHome
        components: [ ds3231 ]

# Enable I2C bus
i2c:
    sda: D2
    scl: D1
    scan: true

# Time component using DS3231
time:
    - platform: ds3231
        id: ds3231_time
	        temperature:
            name: "DS3231 Temperature"

# Optional: Expose actions for automations
# Example: Write current time to RTC
# automation:
#   - trigger:
#       ...
#     action:
#       - ds3231.write_time: ds3231_time
#
#   - trigger:
#       ...
#     action:
#       - ds3231.read_time: ds3231_time
```

## Usage

- The component will automatically keep the RTC in sync with the ESP's system time.
- The temperature sensor will be available as a standard ESPHome sensor entity.
- You can use the provided actions (`ds3231.write_time`, `ds3231.read_time`) in automations to manually synchronize the RTC or read the time from the module.

## Notes

- The default I2C address for the DS3231 is `0x68`. If your module uses a different address, adjust the configuration accordingly.
- Make sure to connect a backup battery to the DS3231 to retain time during power loss.
- The temperature sensor is primarily intended for internal compensation and may not reflect ambient temperature accurately, but it can be useful for monitoring the module's environment.

## Troubleshooting

- If the component fails to communicate, check your I2C wiring and address.
- Use the ESPHome logs to see detailed error messages from the component.

## License

This project is licensed under the GNU General Public License v3.0. See [LICENSE](LICENSE) for details.

## Credits

- Based on the ESPHome external component system.
- DS3231 datasheet: https://datasheets.maximintegrated.com/en/ds/DS3231.pdf
