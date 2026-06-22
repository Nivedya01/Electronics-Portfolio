# High-Speed Low-Side Gate Driver Board for eGaN® FETs

## 📌 Project Overview
This project presents a high-performance, compact hardware platform optimized for driving Enhancement-mode Gallium Nitride (eGaN) transistors. Due to their ultra-fast switching transitions ($dv/dt$ and $di/dt$), GaN FETs are highly susceptible to parasitic loop inductance, which induces gate ringing and overvoltage spikes. 

This design implements a highly optimized, low-inductance coplanar PCB layout using the **Texas Instruments LMG1020** high-speed gate driver (WLCSP package) to drive an **EPC2019 GaN FET** ($200\text{V}$, $8.5\text{A}$ power stage).

---

##  Schematic Circuit Analysis

### KiCad Schematic Diagram
![](https://github.com)

### Circuit Implementations & Design Choices:
* **Asymmetrical Drive Topology:** The `OUTH` (Pin A2) and `OUTL` (Pin B2) of the LMG1020 are routed independently through individual $2\,\Omega$ resistors (`R1` and `R2`). This configuration enables distinct tuning of the turn-on and turn-off paths, suppressing $dv/dt$ induced turn-on problems.
* **Ultra-Local High-Frequency Decoupling:** A $100\text{ nF}$ ceramic bypass capacitor (`C1`) is placed directly across `VDD` (Pin A1) and `GND` (Pin B1). This maintains a tight loop to provide instant transient charging current to the GaN gate.
* **Signal and Power Routing Interfaces:** Dual 2-pin connectors (`Conn_01x02_Pin`) are utilized. `J1` delivers the high-speed PWM input signals (`IN+` and `IN-`), while `J2` interfaces directly with the drain and local source return path of the **EPC2019** (`Q1`).

---

## 📐 PCB Layout Optimization & Parasitic Mitigation

### 🖼️ KiCad 2-Layer PCB Component Layout
![](https://github.com)

### Layout Engineering Strategies Implemented:
* **In-Line Symmetrical Floorplan:** As visible in the layout, the driver (`U1`), split gate resistors (`R1` / `R2`), and the GaN FET (`Q1`) are arranged linearly. This keeps the gate loop paths parallel and of identical length, reducing structural loop inductance.
* **Kelvin Source Return Path:** The ground pads of the drive loop are star-connected directly back to the physical source pad (Pin 3) of the EPC2019. This isolates the gate return signal from heavy power-stage ground noise.
* **Minimizing Trace Inductance ($L_{\text{trace}}$):** Traces connecting `OUTH`/`OUTL` through the $1\,\Omega$ SMD components to the gate (Pin 1) are kept short and thick, directly mitigating inductive $V_{GS}$ voltage overshoots during sub-nanosecond switching.
* **Thermal and Ground Coplanar Pour:** High-density copper fills tie the surrounding ground networks cleanly together, acting as a low-impedance path to shield sensitive signaling nodes from EMI.
