# Methodology (high level)

This describes the **approach** at a conceptual level, for other researchers. It
deliberately contains **no firmware internals** — no memory addresses, no keys, no
opcodes, no resource tables. The value of those details is real, and they stay private.

## Principles

1. **Own device.** All work is performed on a camera I own.
2. **Read before write.** The camera's own storage is read back over a standard USB link
   first; nothing is altered until the behavior is understood.
3. **Reversible first.** Behaviors are confirmed in volatile working memory — changes
   that vanish the moment the camera is power-cycled — before anything permanent is
   considered. This makes exploration safe and non-destructive.
4. **Verify in emulation.** The relevant logic is exercised in a CPU emulator so that
   outcomes and edge cases are checked off-camera before touching real hardware.
5. **Integrity-aware.** The build pipeline respects the camera's own validation so that
   a well-formed image is either accepted cleanly or rejected cleanly — never left
   half-written.

## Pipeline, conceptually

```
Own camera  ──(read-only, USB)──►  local backup of my own device
                                         │
                            understand behavior in RAM (reversible)
                                         │
                            verify logic in a CPU emulator
                                         │
                            build a well-formed image  ──►  flash my own device
                                         │
                            keep a stock restore image
```

## What is intentionally not published here

- Specific memory addresses, offsets, and register layouts (the "map").
- Any key, XOR table, or signature/integrity detail.
- Extracted strings, resource tables, or disassembly listings.
- The build/patch tooling itself.

These are the substance of the research and, for anyone doing serious interoperability
work, the thing worth collaborating on directly. See the contact note in the
[README](../README.md).

## Prior art and community

This follows the established camera reverse-engineering tradition — publish **method and
evidence**, keep the project **non-commercial and community-facing**, and never
redistribute the manufacturer's firmware. See the Nikon-Hacker community and
Simeon Pilgrim's long-running Nikon firmware tooling for the canonical example.
