# AC-DC Power System Simulation (Telecom Site)

A Python simulation of how a telecom site converts AC grid power to DC and keeps running through a grid outage. It combines the power electronics fundamentals (rectification and filtering) with a site-level view (-48V DC plant, battery backup and availability).

**Run it in Colab:** [AC-DC Power System Simulation](https://colab.research.google.com/drive/1UFqloEokMsd2gY81_lhEPyfji_ZUTVaJ#scrollTo=4koaG_PcvHxC)

## Why this project

Telecom towers must stay up even when the grid fails. Site uptime depends on three things working together: the rectifier, the battery and the load. This project models those three and shows how they behave during an outage.

## What it simulates

### Part 1: Rectifier and filter (AC to DC)
- Full-wave bridge rectifier with a capacitor filter
- Includes the diode voltage drop (two diodes in the conduction path)
- Calculates peak output, average DC voltage, ripple and ripple factor
- Compares the simulated ripple with the standard approximation `Vr ≈ Vp / (2 f R C)`

### Part 2: -48V DC plant with battery backup
- Rectifier supplies the load and charges the battery while the grid is available
- During an outage, the battery supplies the load at constant power
- Battery model: state of charge, open-circuit voltage, internal resistance and charge efficiency
- Low voltage disconnect (LVD) at 43.2 V
- Reports minimum state of charge, downtime in minutes, availability and grid energy used

## Key parameters

| Parameter | Default | Meaning |
|---|---|---|
| `f` | 50 Hz | Grid frequency |
| `C` | 4700 µF | Filter capacitor |
| `R` | 20 Ω | Load resistance |
| `P_load` | 1500 W | Site DC load |
| `batt_ah` | 400 Ah | Usable battery capacity |
| `v_float` | 54 V | Rectifier float voltage |
| `lvd` | 43.2 V | Low voltage disconnect |
| `out_start`, `out_end` | 60 to 300 min | Grid outage window |

## How to run

**Colab:** open the link above and run all cells.

**Locally:**
```
pip install numpy matplotlib
python acdc_power_sim.py
```

## Results

- Ripple (simulation): 14.29 V, ripple (formula Vp/2fRC):16.19 V
- Minimum battery state of charge during a 4 hour outage: 70.5 %
- Site downtime: 0 min, availability: 100 %

## What I learned

- How capacitor size affects ripple, and where the formula stops matching the simulation
- Why battery capacity must be sized with margin for depth of discharge and temperature
- How the LVD protects the battery and defines the site-down point

## Limitations

- The rectifier is modelled as an ideal bridge with a capacitor. Real telecom rectifiers are switch-mode (SMPS) units.
- The battery open-circuit voltage is a linear function of state of charge. It ignores Peukert's effect, temperature and aging.
- There is no DG set, so a long outage ends in downtime.

## Future work

- [ ] Add a DG set with start delay and failure probability
- [ ] Monte Carlo simulation of outage risk
- [ ] Rectifier sizing with N+1 redundancy
- [ ] Add temperature and battery aging
- [ ] Compare VRLA and lithium-ion

## Tech

Python, NumPy, Matplotlib, Google Colab

## Author

Shreya Yadav
[LinkedIn](https://www.linkedin.com/in/shreya-yadav-5ab196222/)
