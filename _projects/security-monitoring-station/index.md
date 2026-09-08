---
layout: post
title: Multi-Sensor Security Monitoring Station
description:  A dual-microcontroller monitoring station that detects motion, sweeps an ultrasonic sensor across a 180 degree field on a servo gimbal, and renders the results on a radar-style OLED display. An ESP32 handles sensor acquisition and system state; an STM32G474RE drives all display and indicator output. The two boards communicate over a 115200 baud UART link.
skills:
  - Embedded firmware
  - Multi-MCU architecture
  - UART communication
  - I2C peripheral interfacing
  - Sensor integration
  - Logic level shifting
  - Fusion 360 CAD
  - Design for 3D printing
  - Tolerance validation

main-image: /gimbal.jpg
---

## Overview

This project started as a single-board build and became a two-board system once I hit the limits of running sensing and display work on the same microcontroller. Splitting the responsibilities turned out to be the more interesting engineering problem.

The ESP32 handles everything on the sensing side: a PIR motion sensor, an HC-SR04 ultrasonic rangefinder mounted on a two-axis servo gimbal, an arm/disarm button, and the state machine that ties them together. The STM32G474RE handles everything on the output side: two 1.3 inch OLED displays on independent I2C buses and a set of status LEDs. The two boards talk over UART.

---

## System Architecture

Separating acquisition from display meant each microcontroller could be dedicated to one job. The ESP32 is never blocked waiting on a display refresh, and the STM32 is never blocked waiting on an ultrasonic echo.

**ESP32 (sensing node)**

| Function | Pin |
|----------|-----|
| PIR motion sensor | GPIO34 |
| HC-SR04 trigger | GPIO5 |
| HC-SR04 echo | GPIO18 (via divider) |
| Pan servo | GPIO25 |
| Tilt servo | GPIO26 |
| Arm/disarm button | GPIO32 |
| UART2 TX / RX | GPIO17 / GPIO16 |

**STM32G474RE (display node)**

| Function | Pin |
|----------|-----|
| USART1 TX / RX | PC4 / PC5 |
| I2C1 (radar OLED) | PA15 / PB7 |
| I2C2 (status OLED) | PA9 / PA8 |
| Status LEDs | PB0, PB1, PB2 |

---

## Electrical Design

The HC-SR04 outputs its echo pulse at 5V, but the ESP32 GPIO pins are 3.3V tolerant only. Feeding 5V directly into the input would risk damaging the pin, so I designed a resistive voltage divider using 1k and 2k resistors to bring the echo signal down to a safe level.

The divider output works out to roughly two thirds of the input, putting a 5V pulse at about 3.3V. Detection range with this setup is approximately 180 cm.

I also separated the power rails deliberately. The 5V rail feeds the PIR sensor, ultrasonic sensor, and servos, while a 3.3V rail feeds logic-level components. Servos draw current in spikes when they change direction, and keeping them off the logic rail avoids brownouts on the sensitive components.

---

## Mechanical Design

The ultrasonic sensor is mounted on a two-axis pan-tilt gimbal that I designed in Fusion 360 and printed on a Bambu Lab A1. The design delivers 180 degrees of pan and roughly 100 degrees of tilt.

The platform is a U-shaped bracket. Its flat base mounts to the pan servo horn, the space between the side walls holds the tilt servo, and the front face carries the sensor.

### Tolerance validation

Rather than print the full assembly and discover the fit was wrong, I designed a test coupon first. The coupon contained a ladder of fastener holes stepped in 0.2 mm increments and a pocket matched to the measured dimensions of the servo horn.

I measured the actual servo horn at 34.45 mm tip to tip with a 16.75 mm cross width, rather than trusting the datasheet, since clone servos vary noticeably from nominal specifications.

Printing the coupon first meant I could find the right press-fit dimension for a few grams of filament instead of reprinting the entire bracket. The final joint uses the press fit for rotational lock and a screw through a boss for axial retention.

---

## Firmware

Getting the UART link working end to end took the most debugging. The initial implementation polled for incoming bytes and dropped data intermittently. The fix was enabling the NVIC global interrupt for USART1 so the STM32 could receive asynchronously rather than only when it happened to be checking.

I also wrote a custom 5x7 font renderer for the OLED rather than pulling in a graphics library, which kept the display code small and gave me direct control over the radar rendering.

```c
// UART receive with interrupt enabled - the missing piece
HAL_UART_Receive_IT(&huart1, &rx_byte, 1);

void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart) {
    if (huart->Instance == USART1) {
        rx_buffer[rx_index++] = rx_byte;
        HAL_UART_Receive_IT(&huart1, &rx_byte, 1);
    }
}
```

---

## Debugging Notes

A few problems worth recording, since the fixes were not obvious:

**OLED not detected on I2C1.** Wiring was correct and I scanned the full address range with nothing responding. The cause was missing pull-up resistors. The STM32 HAL does not enable internal pull-ups automatically the way the ESP32 Wire library does, so they have to be turned on explicitly in CubeMX.

**Intermittent link failure.** With only one data-capable USB cable available, powering the ESP32 through a charge-only cable caused the UART link to fail while the STM32 worked fine on the same cable. The ESP32 draws more current with WiFi and sensors active, and a charge-only cable could not supply it reliably.

**Shared ground.** A loose ground wire between the two boards produced symptoms that looked like firmware bugs. Both boards need a common ground reference for UART to work at all.

---

## Current Status

The full sensing-to-display pipeline is validated: ultrasonic reading on the ESP32, transmission over UART, interrupt-driven receive on the STM32, and live distance rendered on the OLED. Remaining work is the radar sweep display, the second status OLED, and final integration of the printed gimbal.
