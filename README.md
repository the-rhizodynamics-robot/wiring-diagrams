# wiring-diagrams

Wiring diagram for the **example Rhizodynamics / GROOT root-imaging robot** built as the
reference rig. It is the hardware companion to
[robot-control](https://github.com/the-rhizodynamics-robot/robot-control) — it shows how the
Arduino Mega 2560, stepper drivers, photointerrupters, camera trigger, and shelf-light
relays are connected, matching the pin map the firmware expects.

![Wiring diagram](wiring.png)

> ⚠️ **This documents one specific reference build.** Pin assignments are what the firmware
> drives; the parts and wire colors are what's on the example robot. Adapt for your own
> hardware. The authoritative pin map lives in robot-control's firmware.

## Source & rendering

The diagram is **generated from [`wiring.yml`](wiring.yml)** with
[WireViz](https://github.com/wireviz/WireViz) — a text source so the wiring diffs cleanly in
git, not a binary CAD file. Edit `wiring.yml`, then regenerate:

```bash
pip install wireviz          # needs Graphviz `dot` on PATH (apt install graphviz)
wireviz wiring.yml           # -> wiring.svg, wiring.png, wiring.bom.tsv (+ .html)
```

Committed outputs: [`wiring.svg`](wiring.svg) (vector), `wiring.png` (embedded above), and
the bill of materials [`wiring.bom.tsv`](wiring.bom.tsv). The `.html`/`.gv` intermediates
are git-ignored — regenerate them locally.

## Pin map (Mega 2560)

Encoded in `wiring.yml`; mirrors `robot-control` (the firmware is the source of truth — keep
this in sync with that repo). Stepper `enable` and the shelf-light relays are **active-LOW**.

| Function | Pin(s) | Notes |
|---|---|---|
| Horizontal motor (X) step / dir / enable | 5 / 7 / 6 | enable active-LOW |
| Vertical motor (Y) step / dir / enable | 8 / 10 / 9 | enable active-LOW |
| Horizontal photointerrupter | D3 | `INPUT_PULLUP`, reads LOW when triggered |
| Vertical photointerrupter | D2 | `INPUT_PULLUP`, reads LOW when triggered |
| Camera trigger | D12 | ~50 ms HIGH pulse |
| Shelf light relays | 23, 25, 27, 29, 31, 33 | index 0 = shelf 1, active-LOW |

Directions: right = LOW, left = HIGH, up = LOW, down = HIGH.

## Power wiring — TODO (build-specific)

`wiring.yml` covers the **Arduino control-signal** wiring from the firmware pin map. The
**power distribution is not yet documented** and is build-specific — fill it in for this rig:

- Motor PSU (+V, GND) → stepper driver `VMOT`/`GND` (driver-dependent, e.g. A4988/DRV8825).
- 5 V logic source for the Mega, sensors, and the relay board's logic side.
- Relay **coil supply (JD-VCC)** and its ground — if powered separately, drop the
  `VCC`-from-Mega link in `wiring.yml`.

## Related repos

- [robot-control](https://github.com/the-rhizodynamics-robot/robot-control) — firmware +
  Python host that drive this hardware.
- [file-sorting](https://github.com/the-rhizodynamics-robot/file-sorting) — processes the
  captured images.

## License

MIT — see [LICENSE](LICENSE).
