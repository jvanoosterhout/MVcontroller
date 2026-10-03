# Design Decisions and Development Notes

This document preserves relevant design context that does not fit in the project overview. It distinguishes historical choices and goals from details that still need verification.

## System goals

The home-automation project started with these goals:

- Keep the system affordable; avoid expensive gateways where a practical alternative exists.
- Use Home Assistant as the central control platform, with an active community and broad device support.
- Prefer understandable, testable, and maintainable software and hardware.
- Use a small number of systems and keep them maintainable over many years.
- Prefer wired networking where practical for reliability. The original assumption that wired networking would always consume less power than Wi-Fi was later reconsidered.

## Hardware and operation

- The initial exploration used a Raspberry Pi 4 as a general-purpose service platform. Its measured/observed power use of roughly 6 W with PoE and physical size motivated trying a Pi Zero 2 W for space- and power-constrained controllers.
- The MV controller was redesigned around a Pi Zero 2 W in a small enclosure. It draws power from the ventilation equipment's 24 V supply through a 24 V-to-5 V buck converter and uses a GX12-6 connection. Verify these details against the current hardware before building.
- The move from TeamViewer/desktop maintenance to SSH and a laptop-based VS Code workflow reduced dependence on a desktop session. VS Code remote development directly on the Pi Zero was unreliable due to its RAM and CPU constraints; the Pi 4 remained useful as a development host.
- The earlier PoE-based network design was not retained for the MV hardware after redesigning its power connection. The current network interface and power budget still need to be documented.

## Software architecture changes

- **2021-2023:** A generic REST interface was explored, with `get_state` and `set_state` operations and controller-specific custom GPIO backend code.
- **2023-2024:** Maintaining separate GPIO backends was judged to be a dead end. The design moved toward one configurable backend and a pin model that could support input and output pin types. Higher-level behavior, including humidity-based fan automation, was moved to Home Assistant as experience with HA automations grew.
- **2024:** Full pin configuration was sent with operations so the service could restore the intended configuration after a restart or power outage. The exact persistence and recovery behavior of the current MQTT implementation must be documented separately.
- **Q1 2025:** GPIO control was extracted into a reusable GPIOapi package rather than copied between controller projects. MV control logic was separated into Home Assistant automations.
- **Q4 2025 onward:** MQTT discovery highlighted the value of grouping related entities under a device. The design goal became a Raspberry Pi service hub that owns hardware-specific configuration and exposes meaningful devices to Home Assistant, rather than exposing individual pins and spreading related configuration across HA.
- **Q1-Q2 2026:** Work began replacing the REST API interface with MQTT devices using HMD-DGB. The intended direction is to reuse established packages and follow common open-source practices.

## Project and package quality goals

These were stated as requirements for the software/package direction; their implementation in this repository has not yet been verified:

- A straightforward setup and maintenance workflow, ideally write an SD card, boot, configure the IP address, then manage configuration and updates from Home Assistant.
- Reusable software rather than copying code into a new folder for each use case.
- Clear documentation and configuration ownership.
- CI checks, unit tests, integration tests, and release/change logs.
- Rely on existing packages where appropriate to avoid unnecessary custom code.

## MQTT follow-up items recorded in Q3 2026

- Reimplement or verify disconnect handling (`on-disconnect`) for the MQTT device.
- Implement an offline topic for the MQTT device.
- Decide whether matching a nested JSON value in an incoming JSON payload is needed.

These are recorded as follow-up items, not as confirmed behavior or completed features.
