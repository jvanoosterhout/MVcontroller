# Hardware

This folder will hold the MVcontroller bill of materials, wiring sources, and generated diagrams.

## Known design history

The documented design history includes a Raspberry Pi Zero 2 W in a compact enclosure, powered from the ventilation unit's 24 V supply through a 24 V-to-5 V buck converter and connected through a GX12-6 connector. A later redesign was planned to use common valves and Belimo actuators in place of a failed CO2-controlled demand valve. This redesign required additional relays and a larger enclosure.

These notes describe design decisions, not a complete verified build specification. The exact current revision, enclosure, power ratings, relay count, valve models, actuator models, protection, and terminal connections must be confirmed before assembly.

## Bill of materials

The initial list is in [bom.csv](bom.csv). Replace TBD entries with verified part numbers, ratings, quantities, and links as the design is finalized.

## Wiring and diagrams

- Wiring source files belong in [wiring/](wiring/).
- Generated diagrams belong in [wiring/generated/](wiring/generated/).
- See [wiring/README.md](wiring/README.md) for the current status and documentation requirements.

## Safety

Document the electrical interface between the controller, ventilation equipment, valves, and actuators before construction. Verify isolation, current and voltage ratings, failure states, and safe behavior on loss of controller power. Do not infer wiring from GPIO labels or historical notes.
