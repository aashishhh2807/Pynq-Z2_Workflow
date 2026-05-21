
# PYNQ-Z2 Software-Hardware Co-Design Workflow

The workflow focuses on integrating custom FPGA hardware with software running on the ARM processor inside the Zynq SoC.

---

# Two Ways to Use PYNQ-Z2

## 1. Normal FPGA Workflow
- Write Verilog/VHDL code
- Generate `.bit` file
- Program FPGA directly using Vivado Hardware Manager

## 2. Software-Hardware Co-Design
- Generate `.bit` and `.hwh` files
- Use Python to communicate with FPGA hardware through AXI interfaces

---

# Workflow Architecture

```text
Verilog Design
      ↓
Simulation
      ↓
Create Custom IP
      ↓
Block Design Integration
      ↓
AXI GPIO Connection
      ↓
Generate Bitstream
      ↓
Upload .bit and .hwh to PYNQ
      ↓
Control Hardware Using Python
```

---

# Vivado Workflow

## 1. Design and Simulation
- Open Vivado
- Write Verilog code
- Simulate the design
- Save the project

---

## 2. Create and Package Custom IP

```text
Tools → Create and Package New IP
```

Steps:
- Select **Package your current project**
- Enter IP name in the Identification section
- Click **Review and Package**
- Click **Package IP**

---

## 3. Create Block Design

```text
Flow Navigator → Create Block Design
```

Add:
- Custom IP
- Zynq Processing System IP
- AXI GPIO IPs

Then run:
- Block Automation
- Connection Automation

---

## 4. FPGA Input/Output Interfacing

To connect FPGA switches, buttons, or LEDs:

```text
Right Click Port → Make External
```

Then:
- Rename the external port
- Add pin mappings in the `.xdc` constraints file

---

## Important Note About Multiple Drivers

A signal should not be driven by multiple sources simultaneously.

Example:

```text
Python GPIO → signal_a
Switch      → signal_a
```

This creates a multiple driver conflict.

To avoid this, use a **MUX (Multiplexer)**:

```verilog
assign a = sel ? python_input : switch_input;
```

---

## 5. Validate Design

```text
Tools → Validate Design
```

There should be:
- No errors
- No critical warnings

---

## 6. Create HDL Wrapper

```text
Sources → Right Click .bd File → Create HDL Wrapper
```

Select:

```text
Let Vivado Manage Wrapper
```

---

## 7. Generate Bitstream

```text
Flow Navigator → Generate Bitstream
```

---

## 8. Generated Files

| File | Purpose |
|---|---|
| `.bit` | FPGA configuration bitstream |
| `.hwh` | Hardware handoff file used by PYNQ |

Typical locations:

### `.bit` File
```text
D:\Vivado\testing\testing.runs\impl_1
```

### `.hwh` File
```text
D:\Vivado\testing\testing.gen\sources_1\bd\and_gate1\hw_handoff
```

---

# PYNQ Workflow

## 1. Install PYNQ OS
- Download the PYNQ image
- Flash it to the SD card
- Complete the initial board setup

---

## 2. Connect the Board
- Connect the Ethernet cable between the laptop and PYNQ-Z2 board

---

## 3. Open Jupyter Notebook

Default PYNQ address:

```text
192.168.2.99:9090
```

---

## 4. Power the Board
- Turn ON the board
- Wait until the DONE LED turns ON

---

## 5. Access Jupyter Notebook
Open the browser and navigate to:

```text
192.168.2.99:9090
```

The Jupyter Notebook runs on the ARM processor inside the Zynq SoC.

---

## 6. Upload Hardware Files
Inside Jupyter:
- Create a project folder
- Upload:
  - `.bit`
  - `.hwh`

Both files must:
- Be in the same folder
- Have the same filename

Example:

```text
and_gate.bit
and_gate.hwh
```

---

## 7. Program FPGA Using Python

```python
from pynq import Overlay

Overlay("and_gate.bit")

print("FPGA Programmed Successfully")
```

---

# Contributors
- Sree Balaji P
- Sriram J
