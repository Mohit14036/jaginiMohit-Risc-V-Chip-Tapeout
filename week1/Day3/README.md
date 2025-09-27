# RTL Workshop – Day 3

**Theme of the Day:** *Optimization in Digital Circuits*

Designs aren’t just about functionality — they must also be **fast, compact, and power-efficient**. Today’s session explores practical optimization strategies for both **combinational** and **sequential** logic, followed by hands-on labs to see these ideas in action.

---

## 🧩 Key Optimization Techniques

### 1. Constant Propagation

* Replace signals driven by constants with the constant value itself.
* Shrinks logic cones and eliminates unnecessary gates.
* **Outcome:** Less logic → smaller area, faster timing.

📌 *Example:* If `y = a ? b : 0;`, synthesis tools reduce it to `y = a & b`.

---

### 2. State Optimization

* Applied to **Finite State Machines (FSMs)**.
* Methods:

  * Remove equivalent/redundant states.
  * Choose efficient binary or one-hot encoding.
  * Simplify transition logic using Boolean reduction.
* **Impact:** Fewer registers and simpler decoding → lower power and area.

---

### 3. Cloning

* Duplicate a logic element to split heavy fan-out loads.
* Shortens long interconnects and improves timing closure.
* **Trade-off:** Uses more cells, but often reduces delay and congestion.

---

### 4. Retiming

* Move flip-flops across combinational logic without altering functionality.
* Goal: balance path delays, reduce the maximum clock period.
* Think of it as “redistributing registers” to meet timing constraints.

---

## 🛠️ Optimization Labs

Each lab highlights a specific synthesis optimization.

### Lab 1 – Conditional Reduction

```verilog
module opt_check (input a, input b, output y);
  assign y = a ? b : 0;
endmodule
```

➡️ Simplifies to `y = a & b`.

---

### Lab 2 – Multiplexer with Constant

```verilog
module opt_check2 (input a, input b, output y);
  assign y = a ? 1 : b;
endmodule
```

➡️ Equivalent to `y = a | b`.

---

### Lab 3 – Alternate Multiplexer Form

```verilog
module opt_check3 (input a, input b, output y);
  assign y = a ? 1 : b;
endmodule
```

➡️ Another mux example reducing to OR logic.

---

### Lab 4 – Nested Ternary Simplification

```verilog
module opt_check4 (input a, input b, input c, output y);
  assign y = a ? (b ? (a & c) : c) : (!c);
endmodule
```

➡️ Simplifies to `y = a ? c : ~c`.

---

### Lab 5 – Constant Flip-Flop

```verilog
module dff_const1(input clk, input reset, output reg q);
  always @(posedge clk, posedge reset)
    if (reset) q <= 1'b0;
    else       q <= 1'b1;
endmodule
```

➡️ After synthesis, `q` is permanently tied high once reset is released.

---

### Lab 6 – Redundant Flip-Flop

```verilog
module dff_const2(input clk, input reset, output reg q);
  always @(posedge clk, posedge reset)
    q <= 1'b1;
endmodule
```

➡️ `q` is always `1`, so the flip-flop can be removed.

---

## 📌 Lessons Learned

* **Constant Propagation** simplifies logic by folding constants.
* **FSM Optimization** reduces hardware overhead through better state representation.
* **Cloning** and **Retiming** improve timing closure in real-world designs.
* Many sequential designs collapse into constant outputs if not coded carefully — labs 5 and 6 prove this.
* Optimization is not just about *removing gates*, it’s about finding the right balance between **area, power, and performance**.

---
