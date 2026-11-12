# acoustofluidic_ev_trapping.ipynb
# Acoustophoretic focusing of extracellular vesicles

Can a standard bulk acoustic wave microchannel focus extracellular vesicles (EVs) using the acoustic radiation force alone? This project answers that with a physics based simulation of particles moving in the channel cross section, sweeping particle size, frequency, acoustic power and particle material.

![Results](figures/acoustofluidic_ev_study.png)

## Why this question matters

EVs are small particles released by cells (roughly 30 nm to 1 µm) that carry markers useful for early cancer detection. Acoustofluidic devices can sort and enrich particles without labels or contact, but the forces involved depend strongly on particle size. This project estimates where direct acoustic focusing stops working and what that means for designing EV enrichment devices.

## Model

* Channel 375 µm wide and 150 µm high, filled with water, driven at its half wave resonance (2.0 MHz), so a pressure node forms along the centre line.
* **Acoustic radiation force** on small spheres in a standing wave (Gor'kov theory), which pushes particles toward the pressure node and scales with particle radius cubed.
* **Rayleigh streaming**, the swirling flow the sound creates near the channel walls (analytic parallel plate solution), which drags particles along with it.
* **Brownian motion**, the random thermal motion that matters for very small particles.
* Particle motion is integrated step by step with a Monte Carlo (Euler Maruyama) scheme: 300 particles per size, 30 s of simulated time, fixed random seed.
* Three materials, described by their acoustic contrast factor Φ: polystyrene bead (0.165), cell (0.059) and EV (0.068). The EV properties are an assumption (density 1.10 g/mL, compressibility close to cells).

## Results

| Quantity | Value |
|---|---|
| Critical diameter for focusing, EVs (simulation / formula) | 2.0 µm / 2.2 µm |
| Critical diameter, polystyrene (simulation / formula) | 1.3 µm / 1.4 µm |
| Focusing time by radiation force at 10 J/m³, 10 µm cell | 2.6 s |
| Focusing time by radiation force at 10 J/m³, 100 nm EV | about 6.3 h |
| Depth of the radiation trap for a 100 nm EV | 0.3 kT (10 J/m³), 2.6 kT (100 J/m³) |
| Frequency needed for 100 nm to be the critical size (EV) | about 950 MHz |

1. **Size decides the regime.** Particles larger than about 2 µm reach the centre within seconds. Smaller particles follow the streaming rolls (panel a, red paths) and never concentrate in the centre (panel b).
2. **Power does not move the limit.** Radiation force and streaming both grow with acoustic energy density, so more power makes everything faster but leaves the critical size unchanged (panel d). For a 100 nm EV the trap is also only a few kT deep, so thermal motion competes with it too.
3. **Frequency and material do move it.** The critical size shrinks with the square root of frequency, and materials with a lower contrast factor (cells, EVs) need larger sizes than polystyrene (panel c). Bringing the limit down to 100 nm would need close to 1 GHz, far outside normal bulk resonator designs.
4. **What it means for device design.** Direct focusing alone cannot enrich EVs. Enrichment needs mechanisms that do not depend on the force on the vesicle itself, such as trapping on larger seed particles through secondary radiation forces, high frequency surface acoustic waves, or channel designs that control streaming. Frequency, power, geometry and contrast factor are the parameters to test experimentally.

## Limitations

* Rayleigh's streaming solution is an approximation for a channel of this height (kh ≈ 1.3). A full finite element model would give the exact streaming field, including side wall effects.
* Wall corrections to drag, gravity, particle interactions and secondary radiation forces are not included.
* EV material properties are assumed and should be checked against measured values.

## Next steps

* Validate the model on the bench by tracking polystyrene beads of 0.5 to 5 µm across frequency and drive voltage.
* Replace the analytic streaming field with a finite element model of the real device.
* Add secondary radiation forces to model seed particle trapping of EVs.

