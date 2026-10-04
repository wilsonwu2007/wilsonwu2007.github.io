---
title: "Bruin Supermileage: Hydrogen Fuel Cell Safety Board"
summary: "Designed a two-layer safety PCB integrating two hydrogen sensors, ignition and emergency-stop interlocks, and CAN communication; verified four fault conditions and estimated a 1 s faster shutdown than the previous year's board."
date: 2025-10-01
org: "UCLA Bruin Racing, Bruin Supermileage"
tags: ["PCB Design", "KiCad", "LTspice", "CAN", "Embedded Systems", "STM32"]
featured: true
order: 1
image: "/images/projects/bruin-supermileage-safety-board/pcb-render.png"
---

![Bruin Supermileage safety board PCB render](/images/projects/bruin-supermileage-safety-board/pcb-render.png)

## Project Overview

**Role:** Low Voltage Engineer · October 2025–June 2026

My teammate and I designed and tested a two-layer safety PCB for UCLA's Shell Eco-marathon hydrogen fuel-cell vehicle. The board integrates two hydrogen sensors, an ignition interlock, emergency-stop inputs, and CAN communication. Its hardware interlock combines these safety signals to interrupt vehicle power when a fault is detected.

**My contribution:** high-level block diagram, safety architecture, schematic design, PCB design, and testing.

## Technical Specifications

- PCB: 2-layer, designed in KiCad
- Input: 12 V
- MCU: STM32
- Sensors: two H₂ sensors
- Power rails: 5 V / 3.3 V
- Safety logic: hardware interlock + relay shutdown
- Feedback: optocoupler isolation
- Communication: CAN

## Design Decisions

- Initially planned to use a **NOR gate and DC-DC converter**, but switched to an **AND gate** for more flexibility in isolating individual failure modes.
- Considered adding a **contactor control system** for high-power switching, but ultimately used a **load switch** for simplicity, cost, and current protection.

![Hand-drawn block diagram of the safety board](/images/projects/bruin-supermileage-safety-board/block-diagram.jpg)

## Process & Challenges

The block diagram was one of the most time-consuming parts of developing the board. As one of my first electrical projects, I spent significant time researching what each component did, why it was needed, and how the system should connect. A teammate later suggested adding a contactor control system, which led to several days of discussion and revisions before we decided against it.

The biggest lesson I took away was to **finalize the architecture before starting the schematic and PCB layout.** Changes at the block diagram stage are relatively easy to make, while changes during PCB layout require much more time and rework.

## Schematic Implementation

**Dual H₂ Sensors**

- Initially selected the **TGS6812-D00 hydrogen sensor**, but its millivolt-range output required an **instrumentation amplifier** to produce a usable signal.
- Tested a second hydrogen sensor available in the workspace and confirmed its output was suitable for hydrogen detection, integrating it as a backup sensor.

![Dual H2 sensor schematic](/images/projects/bruin-supermileage-safety-board/schematic-h2-sensors.png)

**H₂ Sensor Comparator Logic**

- Used an **LMV331 comparator** with a resistor divider to set a detection threshold and convert the sensor output into a digital signal for the AND gate.
- Added a **potentiometer** to adjust the detection threshold without redesigning the circuit.

![H2 sensor comparator logic schematic](/images/projects/bruin-supermileage-safety-board/schematic-comparator.png)

**AND Gate**

- Used an **SN74HCS21 AND gate** so that any safety input going LOW drives the shutdown path LOW, turning off the MOSFETs and opening the safety relays.
- A limitation of the design is that the shutdown path still relies on **MOSFETs**, which can fail. A future revision could use a more mechanically isolated switching path for the final shutdown stage.

![AND gate schematic](/images/projects/bruin-supermileage-safety-board/schematic-and-gate.png)

**Safety Relays**

- Chose **safety relays** for final power switching because of their high current handling and electrical isolation.
- Wired the **E-stop buttons in series with the relay coils**, allowing an E-stop to physically open the coil circuit and cut power without relying on a MOSFET.
- Added **optocoupler feedback** to inform the STM32 of the E-stop state and validated the feedback circuit in LTspice before hardware testing.

During physical testing, MOSFETs were a recurring source of issues. In a future revision, I would explore using mechanically isolated switching for more of the shutdown path.

![Safety relay schematic](/images/projects/bruin-supermileage-safety-board/schematic-safety-relays.png)

## PCB Implementation

**Design Decisions**

- Designed a **2-layer board** to minimize cost, with connectors placed along the edges for easier integration.
- Kept signal traces short to reduce noise and improve signal integrity.
- Added **test points and 0 Ω jumpers** to make testing and troubleshooting easier.

Fitting passive components around the large relay footprints was harder than expected. Resistors and capacitors ended up wedged between the relays, making soldering and rework difficult. Keeping the board small was a priority, but this taught me that **assembly and debugging access need to be considered alongside size and cost during PCB layout.**

![PCB layout render](/images/projects/bruin-supermileage-safety-board/pcb-layout.png)

## Testing & Integration

**What We Tested**

- Added **test points** throughout the board to isolate potential failure points during testing.
- Verified **CAN communication and relay response across four fault conditions** through bench and in-vehicle testing, including hydrogen-sensor faults, E-stop activation, and ignition control.
- Estimated an **approximately 1 s reduction in shutdown response time** compared with the previous year's board using video timing.
- Integrated the safety board with the vehicle and verified that the safety functions operated as intended.

![Board under bench testing with H2 tank](/images/projects/bruin-supermileage-safety-board/testing-photo.jpg)

**Challenges and Limitations**

The INA and H₂ sensor were placed too far apart on the PCB, making the sensor output unreliable. The **0 Ω debug jumpers** allowed us to reroute the connection without redesigning the PCB. Much of the AND-gate logic was verified through **LTspice** rather than systematic physical bench testing. Given the timeline, we prioritized the most critical safety functions, but more comprehensive bench testing could have caught issues earlier. We also estimated relay switching time from video rather than measuring it directly. A future revision would use an **oscilloscope** to accurately measure switching time.
