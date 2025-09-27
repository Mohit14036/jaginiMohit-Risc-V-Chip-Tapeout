# RTL Workshop – Day 2

**Topics for Today:**

* Timing libraries and how to read them
* Comparing hierarchical and flat synthesis flows
* Writing efficient flip-flop RTL code

---

## 1. Timing Libraries

Digital designs are mapped to technology-specific gates using **timing libraries (.lib files)**. These describe the performance, power, and characteristics of each cell in a process.

### SKY130 in Context

The **SkyWater SKY130** PDK (130nm CMOS) is open-source and widely used in the VLSI community. Its libraries provide cell-level data needed by synthesis tools like Yosys.

### Decoding the Library Name

Example: `sky130_fd_sc_hd__tt_025C_1v80.lib`

* **tt** → Typical process corner
* **025C** → Temperature of 25°C
* **1v80** → Supply voltage of 1.8 V

Together, this defines the environment for which the timing data applies.

### How to Explore the File

Since `.lib` is text-based, you can open it directly:

```bash
gedit sky130_fd_sc_hd__tt_025C_1v80.lib
```

Inside, you’ll find definitions for cells, pins, timing arcs, power data, and drive strengths.

---

## 2. Hierarchical vs. Flat Synthesis

Synthesis tools can either **preserve hierarchy** or **flatten everything** into a single netlist.

### Hierarchical Flow

* Each RTL module is synthesized independently.
* Easier debugging since RTL hierarchy is visible in reports.
* Faster turnaround on large designs.
* Limitation: cannot optimize across module boundaries.

### Flattened Flow

* Collapses all modules into one design.
* Enables global optimization across boundaries.
* Can improve performance/area but results in a larger, harder-to-debug netlist.
* Longer runtime and higher memory usage.

**Quick Comparison:**

| Feature      | Hierarchical           | Flattened           |
| ------------ | ---------------------- | ------------------- |
| Debugging    | Easier                 | Harder              |
| Optimization | Local/module level     | Whole design        |
| Runtime      | Faster for big designs | Slower for big ones |
| Output       | Structured netlist     | One big netlist     |

---

## 3. Flip-Flop RTL Coding Styles

Flip-flops are the backbone of sequential logic. Efficient and clear coding avoids unintended latches or synthesis mismatches.

### Asynchronous Reset DFF

```verilog
always @(posedge clk, posedge async_reset)
  if (async_reset) q <= 1'b0;
  else q <= d;
```

Resets immediately when `async_reset` is asserted.

### Asynchronous Set DFF

```verilog
always @(posedge clk, posedge async_set)
  if (async_set) q <= 1'b1;
  else q <= d;
```

Sets output high instantly, independent of the clock.

### Synchronous Reset DFF

```verilog
always @(posedge clk)
  if (sync_reset) q <= 1'b0;
  else q <= d;
```

Resets only on a clock edge.

---

## 4. Simulation and Synthesis Flow

### Simulation with Icarus Verilog

```bash
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd
```

### Synthesis with Yosys

```bash
yosys
read_liberty -lib /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

This flow binds RTL flip-flops to real library cells and produces a gate-level view.

---

## 🔑 Key Takeaways

* `.lib` files define timing, power, and cell details for a given PVT corner.
* **Hierarchical synthesis** is modular and debuggable, while **flat synthesis** is optimized but complex.
* Writing **clear flip-flop RTL** (async/sync reset, async set) ensures predictable synthesis.
* You can simulate with **Icarus Verilog** and synthesize with **Yosys** using open-source SKY130 libraries.

---
