# Environmental Box 2.0

A low-cost, research-grade environmental chamber for microscopic droplet experiments. Developed as a Mechanical Engineering capstone project at Northeastern University, Spring 2025.

![Full System Overview](images/full-system-overview.jpeg)

## Overview

Environmental Box 2.0 enables reproducible droplet evaporation and crystallization experiments by maintaining stable temperature (±2.5°C) and humidity (±5% RH) for over 30 minutes. Built for under $1,000, it delivers performance comparable to commercial chambers costing $18,000+.

Designed for Dr. Xiaoyu Tang's Multiphase Transport Research Lab at Northeastern University.

## The Problem

Droplet experiments are highly sensitive to environmental fluctuations—even minor changes in temperature or humidity compromise reproducibility. The previous Environmental Box 1.0 couldn't sustain stable conditions beyond 5 minutes due to sealing issues and incomplete control logic. Commercial environmental chambers are prohibitively expensive and lack features specific to micro-scale droplet research, such as optical access and low internal airflow.

## Specifications

| Parameter | Value |
|-----------|-------|
| Temperature range | 20–60°C |
| Temperature stability | ±2.5°C |
| Humidity range | 5–95% RH |
| Humidity stability | ±5% RH |
| Hold time | 30+ minutes |
| Chamber dimensions | 12 × 8 × 6 inches |
| Build cost | < $1,000 |

## System Architecture

```
┌──────────────────────────────────────────────────────────┐
│                     TEST CHAMBER                          │
│   ┌──────┐                                ┌──────┐       │
│   │Heater│       ┌──────────────┐         │Heater│       │
│   │  1   │       │ Test Platform│         │  2   │       │
│   └──────┘       └──────────────┘         └──────┘       │
│   ┌──────┐                                ┌──────┐       │
│   │Heater│          [SHT40]               │Heater│       │
│   │  3   │                                │  4   │       │
│   └──────┘                                └──────┘       │
└─────────┬──────────────────────────────────┬─────────────┘
          │ Inlet                     Outlet │
     ┌────▼────┐                       ┌─────▼───┐
     │  Gate   │                       │  Gate   │
     │  Valve  │                       │  Valve  │
     └────┬────┘                       └────┬────┘
          │                                 │
     ┌────▼────┐                       ┌────▼────┐
     │  3-Way  │                       │  3-Way  │
     │  Valve  │                       │  Valve  │
     └────┬────┘                       └────┬────┘
          │                                 │
  ┌───────┴───────┐                ┌────────┴───────┐
  │  HUMIDIFIER   │                │ DEHUMIDIFIER   │
  │  (Ultrasonic) │◄──Inline Fan──►│  (Desiccant)   │
  └───────────────┘                └────────────────┘
```

## Key Components

### Test Chamber
- 12×8×6" sealed acrylic enclosure
- Elevated central test platform for specimens
- Six clear walls for full optical access

### Heating System
- 4× PTC heaters (120W each, 24V DC) at corners
- Even thermal distribution with minimal gradients
- Low-speed integrated fans for gentle air mixing

### Humidity Control
- **Humidifier:** Piezoelectric ultrasonic transducer with baffled reservoir design for sealed operation
- **Dehumidifier:** Modular silica gel desiccant with hot-swap container design

### Airflow Control
- Custom 3D-printed 3-way valves route air through humidifier or dehumidifier
- Gate valves control air entry into test chamber
- Internal mechanical stops eliminate need for precision motor control

### Electronics & Control
- Arduino Uno microcontroller
- SHT40 sensor (±0.2°C temp, ±2.0% RH accuracy)
- Solid-state relays for 10ms switching
- PID temperature control with PWM output

## Results

- Reaches 60°C from ambient in < 3 minutes
- Maintains ±2.5°C temperature stability over 30+ minutes
- Achieves full 5–95% RH range with ±5% stability
- Major improvement over v1.0 (which held stable for only ~5 minutes)

## Repository Structure

```
environmental-box-2.0/
├── README.md
├── arduino/
│   └── pid_temperature_control.ino    # PID control with SHT40 sensor
├── cad/
│   ├── test-chamber/                  # Acrylic enclosure
│   │   ├── Prototype2_Assem.SLDASM    # Full assembly
│   │   ├── Prototype2_Acrylic_*.SLDPRT # Panel parts
│   │   ├── *.DXF                      # Laser cut files
│   │   └── Prototype2_Legv1.SLDPRT    # Support legs
│   ├── valves/                        # 3-way and gate valve STL files
│   │   ├── valve_body_A.STL
│   │   ├── valve_body_B.STL
│   │   ├── valve_turn.STL
│   │   ├── cylinder_valve_*.STL
│   │   └── gate_valve_turn.STL
│   ├── humidifier/
│   │   └── humdifier_prototype.SLDPRT
│   └── dehumidifier/
│       ├── Dehumidifier_Assembly.SLDASM
│       ├── Core_V4.SLDPRT
│       ├── Closed_Cap_V4.SLDPRT
│       ├── Dehumidifier_Base.SLDPRT
│       ├── Dehumidifier_Handle.SLDPRT
│       └── *.STL                      # Print-ready files
├── data/
│   └── dehumidifier_testing_10-31.xlsx
├── docs/
│   └── Enviromental_Box_2_0_Exec_Summary_Final_Version.pdf
└── images/
    ├── full-system-overview.jpeg
    ├── dehumidifier-hot-swap.jpeg
    ├── 3way-valve-assembled.jpeg
    ├── 3way-valve-components.jpeg
    └── arduino-wiring.jpeg
```

## Bill of Materials (Key Components)

| Component | Qty | Notes |
|-----------|-----|-------|
| PTC Heater 120W 24V | 4 | With integrated fans |
| SHT40 Sensor Module | 1 | I2C temperature/humidity |
| Arduino Uno | 1 | Main controller |
| Solid State Relay | 1 | High-current for heaters |
| Piezoelectric Transducer | 1 | Ultrasonic humidifier |
| Silica Gel Desiccant | ~500g | Dehumidifier medium |
| 24V Power Supply | 1 | 10A+ rated |
| 1/4" Acrylic Sheet | — | Chamber walls |
| 2" PVC Tubing | — | Airflow ducting |

## Getting Started

### Hardware Setup
1. 3D print valve components from `/cad/valves/` (use 100% infill for humidifier parts)
2. Laser cut acrylic chamber walls
3. Assemble chamber with acrylic cement
4. Mount PTC heaters at corners
5. Connect tubing between subsystems

### Software Setup
1. Install Arduino IDE
2. Install required libraries:
   - `Adafruit_SHT4x`
   - `Wire` (built-in)
3. Upload `arduino/pid_temperature_control.ino`
4. Connect SHT40: SDA→A4, SCL→A5
5. Connect SSR signal to pin 3

### Tuning
Adjust PID parameters in the code:
```cpp
double kp = 20;  // Proportional gain
double ki = 0;   // Integral gain
double kd = 5;   // Derivative gain
double setpoint = 35;  // Target temperature °C
```

## Photos

### Custom 3D-Printed Valves
| Assembled Valve | Valve Components |
|-----------------|------------------|
| ![3-Way Valve](images/3way-valve-assembled.jpeg) | ![Valve Parts](images/3way-valve-components.jpeg) |

### Modular Dehumidifier (Hot-Swap Design)
![Dehumidifier Hot-Swap](images/dehumidifier-hot-swap.jpeg)

### Arduino Control System
![Arduino Wiring](images/arduino-wiring.jpeg)

## Team

**Design Team**
- Ethan Cook
- Alwyn Henry
- Dillon O'Malley
- Zev Schwab
- Evita Shein

**Design Advisor**
- Prof. Xiaoyu Tang, Northeastern University

## Acknowledgments

- Kevin McCue for electronics guidance
- Multiphase Transport Research Lab for project support and testing facilities
- Environmental Box 1.0 team for foundational work

## License

This project is open-source for educational and research purposes. Feel free to adapt and improve upon this design.

## References

See `docs/Enviromental_Box_2_0_Exec_Summary_Final_Version.pdf` for complete technical documentation and citations.
