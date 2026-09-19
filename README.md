# Analog Mixed-Signal IC Design: BGR & SSADC (2023 AIC Final Project)

## Overview
This repository contains the transistor-level design and SPICE simulation of a complete Analog-to-Digital Converter system, including a Bandgap Reference (BGR) circuit and a Single Slope ADC (SSADC). The project demonstrates low-power analog IC design techniques, achieving a stable reference voltage across process-voltage-temperature (PVT) variations and reliable analog-to-digital conversion using a custom-designed comparator. 

> **Note**: For a detailed explanation of the design methodology, parameter calculations, and complete simulation waveforms, please refer to the attached project report: [Report.pdf](./Report.pdf) (Written in Traditional Chinese).

## System Architecture & Circuit Explanations
* **Bandgap Reference (BGR)**: Designed with a constant-gm biasing core and a startup circuit. It generates a highly stable temperature-independent reference voltage ($V_{ref}$) of approximately 1.36V, which is then supplied to the ADC. To ensure high Power Supply Rejection (PSR) and loop stability, a two-stage operational amplifier was custom-designed and integrated into the feedback loop.
* **Single Slope ADC (SSADC)**: Utilizes the BGR's $V_{ref}$ as its power supply. The architecture consists of a Sample & Hold (S/H) circuit with a bootstrapped switch, a linear ramp generator, and a custom static comparator. The comparator continuously compares the sampled analog input with the rising ramp voltage. Once the ramp exceeds the input voltage, the comparator triggers a 6-bit digital latch to store the counter value, completing the analog-to-digital conversion.

## Detailed Specifications & Simulation Results

### 1. Bandgap Reference (BGR)
Simulated across TT, SS, and FF process corners with a supply voltage of 1.62V, 1.8V, and 1.98V.
* **Reference Voltage ($V_{ref}$)**: ~1.36V
* **Power Consumption**: 36.41 μW (Target: < 50 μW)
* **Temperature Coefficient (T.C.)** (from -40°C to 125°C):
  * TT Corner: 8.77 ppm/°C
  * SS Corner: 8.65 ppm/°C
  * FF Corner: 8.81 ppm/°C
* **Power Supply Rejection (PSR)**:
  * @ DC: -41.4 dB to -48.0 dB
  * @ 10kHz: -41.0 dB to -45.8 dB

### 2. Single Slope ADC (SSADC)
Evaluated at a 20 kHz sampling rate with a 6-bit digital counter.
* **Input Range**: 0.34V ~ 1.28V ($0.25 V_{ref} \sim 0.95 V_{ref}$)
* **Dynamic Performance** (at 20 kHz Sampling Rate):
  * **SNDR**: 31.944 dB
  * **SFDR**: 39.494 dB
  * **ENOB**: 5.014 bits (Stable across 1.62V, 1.8V, and 1.98V supply voltages)

## Repository Structure
The project includes SPICE netlists for transistor-level circuit simulation, MATLAB scripts for frequency-domain analysis, and the comprehensive final report:

* [Report.pdf](./Report.pdf): The complete project report detailing the design process, schematic analysis, and simulation results.
* `bgr.spi`: Transistor-level SPICE netlist for the Bandgap Reference circuit.
* `ssadc.spi`: Transistor-level SPICE netlist for the Single Slope ADC and custom comparator.
* `final_tb.sp`: The top-level testbench used to run the combined BGR and SSADC simulation across PVT corners.
* `FreqA.m` / `SSADC_ENOB_Analysis.mlx`: MATLAB scripts used to perform FFT on the simulation output to calculate SNDR, SFDR, and ENOB.
* `final_enob.csv`: Extracted digital output waveform data (b0~b5) used for MATLAB analysis.