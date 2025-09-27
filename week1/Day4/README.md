# RTL Workshop – Day 4

**Focus Areas:**

* Gate-Level Simulation (GLS)
* Coding styles in Verilog: Blocking vs. Non-Blocking
* Avoiding Simulation vs. Synthesis mismatches

---

## 🚦 Gate-Level Simulation (GLS)

When RTL is synthesized, we get a **gate-level netlist**. GLS is the process of simulating this netlist to ensure it behaves as expected.

Why it matters:

* Confirms that synthesis didn’t change functionality.
* Lets you check **timing behavior** using real cell delays (via SDF).
* Verifies **DFT structures** like scan chains.

Kinds of GLS you’ll see:

* **Functional GLS**: Zero/unit delay, purely logic check.
* **Timing GLS**: Includes delay annotations to expose setup/hold problems.

---

## ⚠️ Simulation–Synthesis Mismatch

It’s common for RTL simulation to pass, but the synthesized version to fail. Some causes:

* Using **initial blocks** or delays (`#5`) in RTL.
* Incomplete sensitivity lists (`always @(sel)` instead of `always @(*)`).
* Incorrect use of blocking/non-blocking assignments.
* Tool-dependent interpretations of ambiguous RTL.

**Rule of thumb:** *If it cannot be synthesized, it should not be in your RTL.*

---

## 🔀 Blocking vs. Non-Blocking Assignments

Two assignment operators, two very different behaviors.

### Blocking (`=`)

* Executes instantly in the order written.
* Suited for combinational logic.

```verilog
always @(*) y = a & b;
```

### Non-Blocking (`<=`)

* Updates scheduled, all take effect at the end of the time step.
* Needed for sequential logic.

```verilog
always @(posedge clk) q <= d;
```

### Quick Guide

| Use Case         | Operator | Why?                             |
| ---------------- | -------- | -------------------------------- |
| Combinational    | `=`      | Reflects immediate evaluation    |
| Sequential (FFs) | `<=`     | Models parallel register updates |

---

## 🧪 Labs

### Lab 1 – Clean 2:1 MUX

```verilog
assign y = sel ? i1 : i0;
```

Synthesize and simulate → works fine.

---

### Lab 2 – Netlist Simulation

Run Yosys → produce netlist → simulate with Icarus + primitives.
Checks consistency between RTL and gates.

---

### Lab 3 – Faulty MUX Example

```verilog
always @(sel) begin
  if (sel) y <= i1;
  else     y <= i0;
end
```

Problems:

* Sensitivity list ignores `i0`/`i1`.
* Wrong operator (`<=` in combinational block).

Correct form:

```verilog
always @(*) begin
  if (sel) y = i1;
  else     y = i0;
end
```

---

### Lab 4 – Blocking Trap

```verilog
d = x & c;
x = a | b;
```

Here, `d` uses the **old** value of `x`. Reorder statements or use a temporary variable.

---

### Lab 5 – GLS of Faulty Code

Run GLS on the buggy mux and notice mismatches vs. RTL. This illustrates why style matters.

---

## 📝 Key Lessons

* Always run **GLS** before moving to layout.
* **Blocking** (`=`) → combinational; **Non-blocking** (`<=`) → sequential.
* Avoid mismatches
