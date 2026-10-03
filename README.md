# MVcontroller

**Work in progress:** This README and the project documentation are being established. Hardware specifications, wiring details, and installation instructions are incomplete and marked TBD where they are not yet verified.

The **MVcontroller** is a Raspberry Pi-based controller for connecting a mechanical ventilation system to Home Assistant. Its design has evolved from a humidity-controlled fan using a custom REST/GPIO backend to a reusable MQTT device architecture based on [HMD-DGB](https://github.com/jvanoosterhout/HMD-DGB).

**Status:** HMD-DGB beta testing is in progress. The controller hardware and historical software designs are still being documented; do not treat TBD wiring or configuration as build instructions.

## Project Map

- **Project overview and development history:** [Overview](#overview) and [History & Design Evolution](#history--design-evolution)
- **Hardware and build:** [hardware/README.md](hardware/README.md)
- **Wiring sources and diagram output:** [hardware/wiring/README.md](hardware/wiring/README.md)
- **Bill of materials:** [hardware/bom.csv](hardware/bom.csv)
- **Software and Home Assistant notes:** [software/README.md](software/README.md) and [docs/README.md](docs/README.md)
- **Project documentation:** [docs/](docs/)
- **Changelog:** [CHANGELOG.md](CHANGELOG.md)

## Overview

MV stands for mechanical ventilation. The project connects ventilation equipment and its controls to Home Assistant. The initial goal was to control a mechanical-ventilation fan in four states based on humidity. Later work explored controlling ventilation valves with common valves and Belimo actuators after the existing CO2-controlled demand valve failed.

The system is intended to separate responsibilities: Home Assistant handles higher-level automations, while the Raspberry Pi service exposes the controller as a device and manages hardware-specific pin bindings. The current software direction uses MQTT and HMD-DGB; earlier iterations used a custom REST API and GPIO backends.

### History & Design Evolution

#### December 2021: System goals

The wider home-automation system was planned to be affordable, understandable, reliable, and maintainable over many years. Home Assistant was selected as the central control platform because it is open, widely supported, and integrates with existing devices. The early MV plan was a four-state fan controlled according to humidity. Raspberry Pi 4, Python, relays, and a simple REST API were explored as an accessible and flexible alternative to expensive proprietary controllers.

#### January 2023: First MV controller

The MV controller used Raspberry Pi GPIO through a custom REST API. The API was intended to provide a simple `get_state` / `set_state` interface, while a custom backend controlled the GPIO pins.

#### 2023: Unifying GPIO control

The controller was extended for humidity-based MV control. Experience with a separate bathroom controller showed that maintaining a different custom backend for each use case would become difficult. The design goal changed to one GPIO backend that could support different pin types and configurations. A Raspberry Pi Zero 2 W and SSH-based maintenance were explored to reduce the size and power consumption compared with a Pi 4.

#### 2024: Pin model and hardware redesign

The software generalized pin control behind the REST API and introduced a pin-configuration model. Home Assistant took over higher-level behavior such as humidity-based fan control. Operations sent the full pin configuration so the controller could resume correctly after a restart or power outage.

The MV hardware was redesigned around a Raspberry Pi Zero 2 W in a compact enclosure, powered from the ventilation unit's 24 V supply through a 24 V-to-5 V buck converter. A GX12-6 connector was used for the connection to the ventilation equipment. Later in 2024, work on the ventilation system revealed that its CO2-controlled demand valve had failed and that the existing CO2 sensors and replacement valve were costly. A further hardware redesign was planned to use common valves and Belimo actuators, requiring more relays and a larger enclosure.

#### 2025: Reusable GPIO service and MQTT exploration

In Q1 2025, GPIO control was separated into the reusable GPIOapi package, and MV control logic was moved into Home Assistant automations. This reduced duplicated controller code, but configuration and entities still required significant manual management in Home Assistant.

In Q4 2025, MQTT discovery was explored with ebusd. Seeing related entities grouped under one Home Assistant device helped motivate a broader design change: Raspberry Pi controllers should act as service hubs, with device configuration and pin bindings managed by the Pi and meaningful devices presented to Home Assistant.

#### 2026: HMD-DGB and beta testing

In Q1-Q2 2026, the REST API was rewritten towards MQTT devices, alongside expanded documentation and testing of HMD-DGB. By Q3 2026, the project was being used for HMD-DGB beta testing. MQTT lifecycle behavior, including disconnect handling and an offline topic, was identified for follow-up work.

## Requirements

### Intended Functionality

- Control a mechanical-ventilation fan, originally planned with four humidity-dependent states.
- Expose ventilation equipment to Home Assistant through MQTT devices.
- Support hardware-specific pin binding on the Raspberry Pi rather than exposing raw pins as Home Assistant entities.
- Explore controlling ventilation valves using common valves and Belimo actuators.
- Recover controller operations after a restart or power outage using centrally managed configuration.

### Design Goals

- Prefer affordable, available hardware over expensive proprietary controllers.
- Keep Home Assistant automations understandable and keep device implementation details in the controller service.
- Reuse established packages and open-source practices where practical.
- Make setup, updates, and long-term maintenance clear and reproducible.

## Hardware Design

The hardware design is being documented. Confirmed design details and gaps are listed in [hardware/README.md](hardware/README.md). The parts list is maintained in [hardware/bom.csv](hardware/bom.csv).

## Pinout & Wiring

Verified GPIO assignments and wiring diagrams have not yet been added. See [hardware/wiring/README.md](hardware/wiring/README.md); do not infer pin assignments from the historical design notes.

## Software & Home Assistant

The software architecture and known setup gaps are summarized in [software/README.md](software/README.md). Home Assistant configuration examples belong in `software/` once their current, tested form is available. Historical use of REST or GPIOapi should not be assumed to describe the current MQTT interface.

## Documentation Status

This repository currently captures the project history and intended architecture. The exact controller model/revision, final valve and actuator selection, relay count, GPIO map, wiring diagrams, installation commands, and tested Home Assistant configuration still need verification.

## Related Projects

- [HMD-DGB](https://github.com/jvanoosterhout/HMD-DGB) - MQTT-based home automation device platform
- [Gcontroller](https://github.com/jvanoosterhout/Gcontroller) - Garage-door and garden-irrigation controller
- [MKcontroller](https://github.com/jvanoosterhout/MKcontroller) - Utility-cupboard controller

## License & Disclaimer

This is a personal home-automation project shared as-is. Electrical work involving mains voltage, relays, or ventilation equipment must be designed and verified safely. The documentation in this repository is incomplete; do not use TBD entries as construction instructions.