# XIAO-seeed-C3-with-LD2410-sensor

```
substitutions:
  device_name: "seeed-c3-motion-sensor"       # or what ever you like
  friendly_name: "Seeed C3 Motion Sensor"     # or what ever you like
  area: "my room"                             # or what ever you like

esphome:
  name: $device_name
  friendly_name: $friendly_name
esp32:
  board: esp32-c3-devkitm-1
  framework:
    type: esp-idf
logger:
api:
  encryption:
    key: !secret api_encryption
ota:
  - platform: esphome
    password: !secret ota_password             # assuming you have set OTA PWD for ESPhome
wifi:
  ssid: !secret wifi_ssid                      # assuming you have set WIFI SSID for ESPhome
  password: !secret wifi_password              # assuming you have set WIFI PWD for ESPhome
  min_auth_mode: WPA2
  ap:
    ssid: $device_name
    password: !secret wifi_fallback_password   # assuming you have set AP mode fallback PWD for ESPhome
#bluetooth_proxy:
#  active: true
web_server:
  auth:
    password: !secret wifi_fallback_password    # assuming you have set WEB PWD for ESPhome
    username: admin
uart:
  - id: uart_ld2410
    tx_pin: GPIO4 
    rx_pin: GPIO3
    baud_rate: 115200
    parity: NONE
    stop_bits: 1
ld2410:
  uart_id: uart_ld2410
binary_sensor:
  - platform: ld2410
    has_target:
      name: "Presence"
      id: presence
      filters:
        - settle: 1000ms
    has_moving_target:
      name: "Moving Target"
      filters:
        - settle: 1000ms
    has_still_target:
      name: "Still Target"
      filters:
        - settle: 1000ms
    out_pin_presence_status:
      name: Out Pin Presence Status      
      filters:
        - settle: 1000ms
  - platform: gpio
    pin:
      number: GPIO10
      mode: INPUT
    name: "LD2410 OUT Pin"
    device_class: motion
sensor:
  - platform: wifi_signal
    id: "wifi_signal_db"
    name: "Wifi Signal dB"
    update_interval: 60s
    icon: mdi:wifi
    entity_category: "diagnostic"
  - platform: copy
    source_id: "wifi_signal_db"
    filters:
      - lambda: return min(max(2 * (x + 100.0), 0.0), 100.0);
    unit_of_measurement: "%"
    entity_category: "diagnostic"
    name: "Wifi Signal Percent"
  - platform: uptime
    name: "Uptime"
    update_interval: 10s
    icon: mdi:clock-outline
  - platform: ld2410
    light:
      name: Light
      filters:
      - throttle: 1000ms
    moving_distance:
      name: "Moving Distance"
      filters:
      - throttle: 1000ms
    still_distance:
      name: "Still Distance"
      filters:
      - throttle: 1000ms
    moving_energy:
      name: "Moving Energy"
      filters:
      - throttle: 1000ms
    still_energy:
      name: "Still Energy"
      filters:
      - throttle: 1000ms
    detection_distance:
      name: "Detection Distance"
      id: distance
      filters:
      - throttle: 1000ms
    g0:
      move_energy:
        name: g0 move energy
      still_energy:
        name: g0 still energy
        filters:
        - throttle: 1000ms
    g1:
      move_energy:
        name: g1 move energy
        filters:
        - throttle: 1000ms
      still_energy:
        filters:
        - throttle: 1000ms
        name: g1 still energy
    g2:
      move_energy:
        name: g2 move energy
        filters:
        - throttle: 1000ms
      still_energy:
        name: g2 still energy
        filters:
        - throttle: 1000ms
    g3:
      move_energy:
        name: g3 move energy
        filters:
        - throttle: 1000ms
      still_energy:
        name: g3 still energy
        filters:
        - throttle: 1000ms
    g4:
      move_energy:
        name: g4 move energy
        filters:
        - throttle: 1000ms
      still_energy:
        name: g4 still energy
        filters:
        - throttle: 1000ms
    g5:
      move_energy:
        name: g5 move energy
        filters:
        - throttle: 1000ms
      still_energy:
        name: g5 still energy
        filters:
        - throttle: 1000ms
    g6:
      move_energy:
        name: g6 move energy
        filters:
        - throttle: 1000ms
      still_energy:
        name: g6 still energy
        filters:
        - throttle: 1000ms
    g7:
      move_energy:
        name: g7 move energy
        filters:
        - throttle: 1000ms
      still_energy:
        name: g7 still energy
        filters:
        - throttle: 1000ms
    g8:
      move_energy:
        name: g8 move energy
        filters:
        - throttle: 1000ms
      still_energy:
        name: g8 still energy
        filters:
        - throttle: 1000ms
text_sensor:
  - platform: version
    name: "Version"
    icon: mdi:cube-outline
  - platform: wifi_info
    ip_address:
      name: "IP"
      id: myip
    ssid:
      name: "SSID"
    bssid:
      name: "BSSID"
    mac_address:
      name: "Mac Address"
    scan_results:
      name: "Latest Scan Results"
    dns_address:
      name: "DNS Address"
    power_save_mode:
      name: "Wifi Power Save Mode"
switch:      
  - platform: restart
    name: $friendly_name restart
  - platform: ld2410
    engineering_mode:
      name: "Engineering Mode"
    bluetooth:
      name: "LD2410 Bluetooth"
number:
  - platform: ld2410
    timeout:
      name: Timeout
    light_threshold:
      name: Light Threshold
    max_move_distance_gate:
      name: Max Move Distance Gate
    max_still_distance_gate:
      name: Max Still Distance Gate
    g0:
      move_threshold:
        name: g0 move threshold
      still_threshold:
        name: g0 still threshold
    g1:
      move_threshold:
        name: g1 move threshold
      still_threshold:
        name: g1 still threshold
    g2:
      move_threshold:
        name: g2 move threshold
      still_threshold:
        name: g2 still threshold
    g3:
      move_threshold:
        name: g3 move threshold
      still_threshold:
        name: g3 still threshold
    g4:
      move_threshold:
        name: g4 move threshold
      still_threshold:
        name: g4 still threshold
    g5:
      move_threshold:
        name: g5 move threshold
      still_threshold:
        name: g5 still threshold
    g6:
      move_threshold:
        name: g6 move threshold
      still_threshold:
        name: g6 still threshold
    g7:
      move_threshold:
        name: g7 move threshold
      still_threshold:
        name: g7 still threshold
    g8:
      move_threshold:
        name: g8 move threshold
      still_threshold:
        name: g8 still threshold
button:
  - platform: ld2410
    factory_reset:
      name: "LD2410 Factory Reset"
    restart:
      name: "LD2410 Reboot"
    query_params:
      name: "LD2410 Query Params"
  - platform: restart
    icon: mdi:power-cycle
    name: "ESP Reboot"
select:
  - platform: ld2410
    distance_resolution:
      name: "Distance Resolution"
    baud_rate:
      name: "Baud Rate"
    light_function:
      name: Light Function
    out_pin_level:
      name: Out Pin Level

esp32_ble_tracker:

```
