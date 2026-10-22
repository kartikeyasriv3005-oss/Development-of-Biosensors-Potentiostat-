# Development-of-Biosensors-Potentiostat-
# NanoStat: ESP32-Based Wireless Potentiostat for Electrochemical Gas Sensing

An open-source, low-cost potentiostat platform integrating an **ESP32 microcontroller** and the **LMP91000 electrochemical analog front end** for amperometric gas sensing, sensor characterization, and wireless measurement visualization.

## Overview

NanoStat is an embedded electrochemical measurement system designed for amperometric gas sensors, with a focus on nitrogen dioxide (NO₂) detection. The system interfaces with an electrochemical sensor, conditions its low-level output current using the LMP91000, and uses the ESP32 for measurement acquisition, processing, and wireless communication.

The project combines analog front-end design, embedded firmware, electrochemical sensor interfacing, PCB development, and electrode optimization through COMSOL Multiphysics simulations.

## Key Features

- **Electrochemical signal conditioning:** LMP91000 analog front end for potentiostatic sensor interfacing and current-to-voltage conversion.
- **Embedded control:** ESP32-based configuration and measurement processing.
- **I²C communication:** Register-level configuration of the LMP91000, including supported bias and transimpedance amplifier settings.
- **Wireless connectivity:** Wi-Fi-based communication for remote measurement access and visualization.
- **Amperometric gas sensing:** Designed around electrochemical NO₂ sensor characterization.
- **Electrode design exploration:** COMSOL simulations to investigate alternative electrode geometries, including interdigitated and Hilbert-curve structures.
- **Hardware development:** Circuit optimization, PCB design, and preparation of fabrication files.

## System Architecture

```text
       Electrochemical Gas Sensor
                 |
          Sensor Current
                 |
                 v
        +------------------+
        |     LMP91000     |
        | Electrochemical  |
        | Analog Front End |
        |                  |
        | Potentiostat     |
        | Control + TIA    |
        +------------------+
                 |
          Analog Voltage
                 |
                 v
        +------------------+
        |      ESP32       |
        |                  |
        | ADC Acquisition  |
        | Signal Processing|
        | Sensor Data      |
        +------------------+
                 |
          Wi-Fi Communication
                 |
                 v
        Measurement Visualization
```

The ESP32 communicates with the LMP91000 over I²C to configure the analog front end. The LMP91000 conditions the sensor current into an analog voltage, which is sampled by the ESP32's ADC and processed for measurement and visualization.

## Hardware Components

| Component | Role |
|---|---|
| ESP32 | Embedded controller, ADC acquisition, data processing, and Wi-Fi connectivity |
| LMP91000 | Electrochemical analog front end, potentiostat control, and programmable transimpedance amplification |
| Electrochemical gas sensor | Sensing element for amperometric NO₂ measurements |
| Working, reference, and counter electrodes | Electrochemical interface, depending on the sensor configuration |
| PCB and supporting components | Signal routing, power distribution, and hardware integration |

*The final component values, power configuration, and sensor connections depend on the implemented circuit and the selected electrochemical sensor.*

## Measurement and Signal Processing

### 1. Sensor interfacing

The electrochemical sensor produces a current in response to the target gas and the sensor's operating conditions. This current can be very small, making appropriate analog signal conditioning essential.

### 2. Potentiostat control and current-to-voltage conversion

The LMP91000 provides the analog interface for controlling the sensor's electrochemical operating conditions and converting the sensor current into a measurable voltage through its transimpedance amplifier (TIA).

For an ideal transimpedance stage, the current magnitude can be estimated from:

\[
|I_{\text{sensor}}| \approx
\frac{|\Delta V_{\text{out}}|}{R_{\text{TIA}}}
\]

where:

- \(I_{\text{sensor}}\) is the sensor current.
- \(\Delta V_{\text{out}}\) is the output-voltage change relative to the appropriate baseline.
- \(R_{\text{TIA}}\) is the configured transimpedance gain expressed as resistance.

The actual conversion must account for the circuit's polarity, reference voltage, selected gain, and calibration.

### 3. ESP32 acquisition

The ESP32 samples the analog output using its ADC. Firmware can convert the ADC reading into voltage and then into an estimated sensor current using the configured gain and calibration parameters.

ADC resolution alone does not determine measurement accuracy; noise, nonlinearity, grounding, and the analog signal range also matter.

### 4. Wireless monitoring

The ESP32 provides Wi-Fi connectivity for transmitting measurement data to a remote interface. A web-based interface can be used to display measurements and support remote access to the sensing system.

## Embedded Firmware

The firmware is responsible for coordinating the analog front end, acquiring sensor measurements, processing the data, and enabling wireless communication.

Core firmware responsibilities include:

- Initializing the ESP32 and I²C interface.
- Configuring supported LMP91000 registers.
- Setting the appropriate sensor bias and TIA gain.
- Acquiring analog voltage measurements.
- Converting voltage measurements into estimated sensor current.
- Handling measurement data and wireless transmission.
- Supporting measurement visualization and debugging.

## Electrode Design and COMSOL Simulation

In addition to the electronics, the project explores electrode geometries for electrochemical sensing using COMSOL Multiphysics.

The design study considers conventional interdigitated electrodes and alternative geometries, including Hilbert-curve structures, to investigate their electric-field distributions and potential implications for sensor performance.

The simulation workflow includes:

- Building electrode geometries and defining material properties.
- Setting up the relevant physics and boundary conditions.
- Generating a suitable mesh with local refinement near electrode features.
- Comparing electric-field distributions and localized field concentrations.
- Evaluating geometry-dependent behavior to inform electrode design decisions.

Simulation results are intended to guide design optimization. Improvements in actual sensor sensitivity or response must be established through appropriate electrochemical experiments.

## PCB Design and Hardware Integration

The hardware development workflow includes circuit design, analog front-end optimization, PCB layout, and fabrication preparation.

**Tools:** EasyEDA, LTspice, COMSOL Multiphysics, and Gerber fabrication files.

Key engineering considerations include:

- Maintaining a stable sensor bias.
- Selecting an appropriate TIA gain for the expected current range.
- Minimizing leakage currents and unwanted noise.
- Designing suitable analog and digital grounding.
- Verifying the analog output range against the ESP32 ADC input range.
- Calibrating the current measurement chain.
- Validating the integrated hardware against known electrical inputs and sensor measurements.

## Technology Stack

| Category | Technologies |
|---|---|
| Microcontroller | ESP32 |
| Analog front end | Texas Instruments LMP91000 |
| Communication | I²C, Wi-Fi |
| Embedded development | ESP32 firmware development |
| Interface development | HTML, CSS, JavaScript, as applicable to the implemented interface |
| Circuit simulation | LTspice |
| Electrode simulation | COMSOL Multiphysics |
| PCB design | EasyEDA |
| Fabrication outputs | Gerber files |

## Potential Extensions

The following are possible extensions rather than claims of completed functionality:

- Live current-versus-time plotting and measurement logging.
- Calibration routines and baseline correction.
- Sensor response and recovery-time characterization.
- Gas concentration estimation using experimentally established calibration curves.
- Support for additional compatible electrochemical sensors.
- Implementation of controlled electrochemical measurement waveforms where the hardware architecture supports them.
- Experimental comparison of simulated electrode geometries.

Techniques such as cyclic voltammetry (CV), linear sweep voltammetry (LSV), chronoamperometry (CA), and differential pulse voltammetry (DPV) require suitable waveform generation, acquisition, and control capabilities. They should be listed as implemented features only after the corresponding functionality has been verified.

## Project Status

**Project focus:** Electrochemical sensor interfacing, ESP32–LMP91000 integration, wireless data acquisition, electrode simulation, and hardware development.

The final status of the PCB, firmware, user interface, and experimental validation should be updated to reflect the current repository and tested hardware.

## Future Work

- Complete PCB fabrication and hardware bring-up.
- Verify LMP91000 configuration and sensor bias experimentally.
- Characterize the ADC and calibrate the current measurement chain.
- Test repeatability, baseline stability, noise, and sensor response.
- Integrate a measurement dashboard and data-logging workflow.
- Validate the performance of selected electrode geometries experimentally.

## References

- [Texas Instruments LMP91000 — Configurable Analog Front End for Chemical Sensors](https://www.ti.com/product/LMP91000)
- [Espressif ESP32 Documentation](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/)
- [COMSOL Multiphysics](https://www.comsol.com/)

---

**NanoStat** — Combining electrochemical sensing, embedded electronics, wireless instrumentation, and electrode design into a compact measurement platform.
