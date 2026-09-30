# Bill's Mushroom Grow Project 🍄
> 🍄 One of several related projects. See the full list at **[billjuv.github.io](https://billjuv.github.io)**.
> 
I'm a hobbyist, not a professional developer, who enjoys tinkering with IoT, home automation, and environmental monitoring. When a friend started a mushroom growing operation in Nevada, I offered to handle the tech side.

I knew (and still know) next to nothing about growing mushrooms. But I do know how to get a sensor to talk to a dashboard. So the deal was simple: **he grows, I wire.**

The goal was to monitor and control everything from a dashboard on his cell phone.

---

## The Setup

The grow operation lives in **two insulated shipping containers** parked side by side, with a sliding door cut in to connect them:

- **Container 1:** Lab and incubation areas
- **Container 2:** Fruiting room, plus a pre-conditioning room with a mini-split for air conditioning

Shipping containers are great for mushrooms. For WiFi, they're basically a Faraday cage. More on that below.

## The Stack

- **Raspberry Pi 4** as the central server
- **Node-RED** with **FlowFuse Dashboard 2.0**
- **MQTT** (Mosquitto)
- **InfluxDB 1.x** + **Grafana**
- **ESPHome** on ESP32 boards
- **LoRa** hardware via **OpenMQTTGateway (OMG)**

For remote access, the dashboard is reached through **RemoteRED**. It works well and can also send notifications. I use **Tailscale** when I need to get in and do remote programming.

Nearly everything talks MQTT, so most of this could be ported to Home Assistant if you prefer that route.

### Why not Mycodo?

I started out with Mycodo as the control platform. It turned out to be overkill: this setup doesn't need PID control or other fancy functions, and the dashboards and controls didn't translate well to a phone screen. So I moved everything to Node-RED. Claude helped with much of the automation logic and the functions needed for the changeover.

---

## Getting Power and Signal Through Metal Boxes

**Power.** The Pi 4 sits in a weather-resistant box in the lab. It shares that box with a MeanWell 5V/5A power supply, which has cables run to the ESP32 sensor boxes throughout the containers. That means no USB power bricks sitting out in the humidity and spores. (Spores get *everywhere*.)

**WiFi.** Most of the sensors and smart plugs are on WiFi, so an Ethernet-connected **GL.iNet router** serves as an access point. The Pi is also wired to Ethernet.

Then my friend noticed that some devices in the fruiting container dropped off whenever he closed the sliding metal door. The fix was a second access point (**TP-Link RE705X**) in the pre-conditioning room. Lesson learned: metal doors are very good at their job.

**LoRa.** As an experiment for future expansion around the farm, I built LoRa-based sensor packs (SCD41 CO₂ / temperature / humidity). They report to a LoRa/WiFi receiver. So far, LoRa gets through the metal containers just fine.

---

## Projects

- [Node-RED Dashboard 2.0](https://github.com/billjuv/Mushroom-NodeRED-Dashboard) 🆕: The phone dashboard that ties it all together, including lights, humidity, fans, CO₂, temperatures, energy use, the heat pump, and alerts. Each card has a screenshot, notes, the devices used, and an importable Node-RED flow.
- [EZO/ESPHome CO2 and Humidity monitoring](https://github.com/billjuv/EZO_ESPHome) 🆕: ESPHome firmware for an ESP32 sensor box using Atlas Scientific EZO-CO2 and EZO-HUM sensors over I2C, with MQTT publishing and a remote command channel. Adaptable to other EZO circuits.
- [XIAO/LoRa CO2 monitoring](https://github.com/billjuv/XIAO-LoRa-CO2-monitoring): A wireless CO₂, temperature, and humidity monitoring system for mushroom grow operations. It uses LoRa radio to send sensor data to a central MQTT broker, so no WiFi is required on the sensor node.
- [EC Fan ESPHome](https://github.com/billjuv/EC_Fan_ESPHome): ESPHome configuration for EC fans, for use with MQTT, Node-RED dashboards, Home Assistant, or Mycodo. The control board is powered by the fan itself.
- [EC PWM Fan Control Boards](https://github.com/billjuv/EC_PWM_FanControlBoards): Stand-alone PWM fan control boards for EC fans that use USB-C connectors for PWM speed control. They're modifications of Kyle Gabriel's Mycodo fan control boards for TerraBloom EC fans, adapted to use USB-C connectors instead of audio connectors.
- [Govee BLE Sniffers with OpenMQTTGateway](https://github.com/billjuv/OMG-ESP32-BLE_govee_sniffer): ESP32 boards that listen to Govee temperature/humidity sensors over Bluetooth and forward the data to MQTT over WiFi.

## Related

- [ESP32-Renogy-LoRa](https://github.com/billjuv/ESP32-Renogy-LoRa): A standalone LoRa transmitter that draws its power from a Renogy solar charge controller and reads its data over RS232. It sends the solar data to a central MQTT broker. Built with an ESP32 DevKit-v1 and an Adafruit RFM95W LoRa transceiver.
- [ESP32-S3_XiAO-Renogy-LoRa](https://github.com/billjuv/ESP32-S3_XiAO-Renogy-LoRa): A smaller, cheaper version of ESP32-Renogy-LoRa, using a SeeedStudio ESP32-S3 XIAO/LoRa kit and a smaller, easier-to-wire RS232-to-TTL board.

## More Coming Soon

There's always another sensor to add. 🍄
