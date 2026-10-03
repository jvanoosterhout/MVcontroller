# Software

The MVcontroller software direction is a Raspberry Pi service using HMD-DGB over MQTT. The service is intended to own hardware-specific pin configuration and expose meaningful ventilation devices to Home Assistant. Home Assistant handles higher-level automations such as humidity-based control.

## Architecture history

- Earlier versions used a custom REST API with `get_state` / `set_state` operations and GPIO backends.
- GPIO control was later generalized and separated into the reusable GPIOapi package.
- In Q1-Q2 2026, work began replacing the REST interface with MQTT devices using HMD-DGB.

Historical REST and GPIOapi details are included for context only; they are not installation instructions for the current software.

## Setup and configuration

The tested operating-system image, Python version, HMD-DGB release, service name, installer, MQTT topics, configuration payload, Home Assistant automations, and update procedure have not yet been documented for this repository. Add tested, revisioned instructions here when available. Do not copy a configuration from another controller without checking its device model and pin mapping.

## Verification status

The project requirements include reusable code, clear documentation, CI checks, unit tests, integration tests, release history, and straightforward setup and maintenance. Which of these are currently implemented for the MVcontroller must be verified. See [design decisions](../docs/design-decisions.md).
