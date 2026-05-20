
https://wokwi.com/projects/438812540487441409

# ESP32 Smart Temperature Control System using MQTT

## Overview

This project is an IoT-based temperature monitoring and control system using an ESP32, a temperature sensor (NTC thermistor module), and a relay module.

The ESP32:

* Reads temperature from the sensor
* Publishes temperature data using MQTT
* Controls a relay remotely
* Supports:

  * **Manual Mode**
  * **Automatic Mode**

The project uses the public MQTT broker:
`broker.hivemq.com`

---

# Hardware Components

* ESP32 Development Board
* Relay Module
* NTC Temperature Sensor Module
* LED (used as load simulation)
* Jumper Wires
* Breadboard

---

# Circuit Diagram

![Image](https://images.openai.com/static-rsc-4/zBUx-momYRJr6MTV5zreQb5OzUrGk06dfwWK5r7sICGJZBED4AuhD-OKX6UY8_M0VSbdBt23Tn11tEuqoJNlFmwhDYjPB5_2i_c6KEEQ0lFCYqH9R260AY4Yt8Qjno8yLWj5C3y9uMzv8fUHcOAJFajTnNvbw5SPHIvIXPlbzbz7UUT_esDTZN2bws437_yw?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/wS4zC3fTCTcjRWCXByBBvaHNW4_KIJ82Y6m538XOTS6957eXJK_vSUVqsHzmOYF8pejb0gFhgRgejJbSEvWJ5zIzH57sDNvOa_9_h7LZ82qrnNvrqi8Fmdov3z0ubKAee-oOCEs5QCV2DzLS8WY_gfR-xuprc2LsD0EI_8BtXhwzhsvArtCeTddwlz2qU7GH?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/b6BvJLCF09vOVakB6uOfadypHHmNHh1mkexB8TZfwQLB7CgtP1y6CCtWh7dak4ByJtDEDK-rUEM-D5X9srBYqneq9gaDMUPW_t1I_3jMUgFiH-knppRfFHs2HbfhFIn_6C2TmG4l-S57GI49BrVojt3o1l1apzVlaRC6s7aRMS3fnqB-MVaDvBH2DG3H0o-D?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/9vRSjOO-YyU-YX33N4wjMfJFFg4SxhRFk4uzY1PmdJ520bnt60UkcLBS5h2c8TAWpmy27NQApocWoulTOC7mwLQuW41P5RvgwGE0C9h73i5Bed0nAa_KgIsf6DTRHKaccz21kpA_Ex99NTRU3u1aq-35SPFTFTiVPn4WF9i24HmKYzOAOLZxECtcwoRBdSqi?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/kAjYcPLfQ35t0euCbGXiHlhFljon90YXfXlInwUktm3HYoEmRZo1rtS8PDuAfECoONwlwQ7U0Z8vBnSWVclIX3M0sAKw7KWED-snUs2pLb7vVQx3MircAfwYDcdMgq35eOyDtvhIiv0Nds_3E2182u3ZEvdYqryqlavV4XbGee9Gg9PCo57iMbGAHK5M80eu?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/xoLhVXLkJKgx6MIsQWwovY6yYvtL660o2IptiIFfr07QjK651Ehpv2yGtQxAhwymemKYZPz6S0qgbkLzMz85yJrEx272Oh29w-OyXhSZhkZS5ioKZmM8DFg_plHgToyJdgFpTFWBUpUAa01WJ1p8oi76lkCl1lmTXYbdJKzCCCqVJaWXKZIK-P30Lw6o9_XL?purpose=fullsize)

### Pin Connections

| Component              | ESP32 Pin |
| ---------------------- | --------- |
| Relay IN               | GPIO 32   |
| Temperature Sensor OUT | GPIO 33   |
| VCC                    | 3.3V / 5V |
| GND                    | GND       |

---

# Features

## 1. Temperature Monitoring

The ESP32 continuously reads the temperature from the thermistor sensor.

## 2. MQTT Communication

The device publishes temperature readings to MQTT topics.

## 3. Manual Relay Control

You can manually turn the relay:

* ON
* OFF
* STOP

## 4. Automatic Temperature Control

The relay turns ON/OFF automatically depending on a temperature threshold.

---

# MQTT Topics

| Topic     | Purpose                    |
| --------- | -------------------------- |
| `measure` | Publish temperature values |
| `mode1`   | Select operation mode      |
| `manual`  | Manual relay control       |
| `edge`    | Automatic mode threshold   |

---

# Modes of Operation

## Manual Mode

Publish:

```text
m
```

to topic:

```text
mode1
```

Then send commands to:

```text
manual
```

### Commands

| Command | Action           |
| ------- | ---------------- |
| `on`    | Turn relay ON    |
| `off`   | Turn relay OFF   |
| `stop`  | Stop manual mode |

---

## Automatic Mode

Publish:

```text
a
```

to topic:

```text
mode1
```

Then publish a temperature threshold to:

```text
edge
```

Example:

```text
30
```

Behavior:

* If current temperature > threshold → Relay ON
* If current temperature < threshold → Relay OFF

To stop automatic mode:

```text
99
```

---

# MQTT Workflow

```text
User/App
   ↓
MQTT Broker
   ↓
ESP32 subscribes to commands
   ↓
Relay Control
```

---

# How It Works

## Temperature Calculation

The thermistor temperature is calculated using:

T=\frac{1}{\frac{\ln\left(\frac{1}{\frac{4095}{ADC}-1}\right)}{B}+\frac{1}{298.15}}-273.15

Where:

* `ADC` = analog sensor reading
* `B` = thermistor beta coefficient

---

# Software Requirements

* MicroPython
* ESP32 firmware
* MQTT Library:

  * `umqtt.simple`

---

# Installation

## 1. Flash MicroPython to ESP32

Install MicroPython firmware on your ESP32 board.

## 2. Upload the Code

Upload the Python script to the ESP32 using:

* Thonny
* uPyCraft
* ampy

## 3. Install MQTT Library

Ensure:

```python
from umqtt.simple import MQTTClient
```

works correctly.

---

# Running the Project

1. Connect ESP32 to WiFi
2. ESP32 connects to MQTT broker
3. Select:

   * Manual mode
   * Automatic mode
4. Monitor temperature values from MQTT topic `measure`

---

# Example MQTT Commands

## Enable Manual Mode

Topic:

```text
mode1
```

Message:

```text
m
```

---

## Turn Relay ON

Topic:

```text
manual
```

Message:

```text
on
```

---

## Enable Automatic Mode

Topic:

```text
mode1
```

Message:

```text
a
```

---

## Set Temperature Threshold

Topic:

```text
edge
```

Message:

```text
28
```

---

# Future Improvements

* Add OLED/LCD display
* Add mobile application
* Store data in cloud database
* Add email/SMS alerts
* Add web dashboard
* Use secure MQTT with authentication

---

# Author

**Abdullah Said**

Engineering Student — Communications & Computer Engineering

GitHub Project: `bank_clients`

---

# License

This project is open-source and available for educational purposes.
