# RTL Workshop – Day 1

Welcome to the first day of the **RTL Design and Synthesis Workshop**.
Today’s focus: writing Verilog, simulating designs with **Icarus Verilog**, and performing basic synthesis using **Yosys**.

---

## 🧭 Learning Roadmap

* Understand the role of a **design**, **testbench**, and **simulator**
* Run your **first Verilog simulation**
* Explore a **2:1 multiplexer design**
* Perform **RTL-to-gate synthesis** with Yosys
* Learn about **technology libraries**

---

## 1. Core Concepts

* **Design** → Your Verilog description of the digital circuit
* **Testbench** → Provides stimulus and checks behavior
* **Simulator** → Executes design + testbench to validate logic before hardware

Think of it as:
**Design = Circuit**, **Testbench = Environment**, **Simulator = Playground**

---

## 2. Tools Setup

Make sure you have the following installed:

```bash
sudo apt install iverilog
sudo apt install gtkwave
```

For synthesis:

```bash
sudo apt install yosys
```

---

## 3. Example: 2:1 Multiplexer

### Design (good_mux.v)

```verilog
module good_mux (input i0, input i1, input sel, output reg y);
always @(*) begin
    if(sel) y <= i1;
    else    y <= i0;
end
endmodule
```

### Testbench (tb_good_mux.v)

The testbench applies values to `i0`, `i1`, and `sel`, then monitors output `y`.

---

## 4. Simulation Flow

1. Clone workshop repo:

   ```bash
   git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
   cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
   ```

2. Compile design + testbench:

   ```bash
   iverilog good_mux.v tb_good_mux.v
   ```

3. Run:

   ```bash
   ./a.out
   ```

4. Open waveforms:

   ```bash
   gtkwave tb_good_mux.vcd
   ```

---

## 5. From RTL to Gates with Yosys

1. Launch yosys:

   ```bash
   yosys
   ```

2. Load standard cell library:

   ```bash
   read_liberty -lib /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```

3. Read design:

   ```bash
   read_verilog good_mux.v
   ```

4. Run synthesis:

   ```bash
   synth -top good_mux
   ```

5. Map logic to library cells:

   ```bash
   abc -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```

6. View schematic:

   ```bash
   show
   ```

---

## 6. Why Gate Libraries Matter

Standard cell libraries contain multiple versions of each gate (AND, OR, MUX, etc.) optimized for:

* **Speed** (fast paths)
* **Power** (low-energy cells)
* **Area** (smaller footprint)
* **Drive strength** (ability to fan out signals)

The synthesis tool automatically chooses the right version based on constraints.

---



