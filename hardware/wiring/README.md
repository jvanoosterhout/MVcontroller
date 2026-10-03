# Wiring Documentation

No verified MVcontroller wiring diagram or GPIO-to-connector map has been added yet. Do not use the historical GX12-6 reference as a pinout.

When documenting a hardware revision, add:

- A source diagram in this folder and its generated output in [generated/](generated/).
- Raspberry Pi GPIO numbers (BCM), signal direction, active level, and pin type.
- Connector names, pin numbers, wire colors, and destination terminals.
- Voltage/current ratings, isolation, protection, and expected safe state on controller or power failure.
- The exact valve, actuator, relay, and power supply models used by the revision.

Use revision-specific diagram names, for example `mv-controller-rev-a.yaml` and `generated/mv-controller-rev-a.svg`, once a verified diagram is available.
