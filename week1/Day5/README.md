# RTL Workshop – Day 5

**Topic:** Optimizing Synthesizable RTL

**Highlights:**

* Writing complete `if-else` and `case` statements
* Preventing inferred latches
* Using `for` loops and `generate` blocks for scalable hardware
* Understanding ripple carry adder (RCA) design

---

## 1️⃣ If-Else Statements

Used in procedural blocks (`always`, `initial`) to implement conditional behavior.

**Syntax Example:**

```verilog
if (cond) begin
    // execute if true
end else begin
    // execute if false
end
```

**Nested Conditional:**

```verilog
if (cond1)
    // action 1
else if (cond2)
    // action 2
else
    // default action
```

> Always include all paths to avoid unintended latches.

---

## 2️⃣ Inferred Latches

A latch is inferred when a variable isn’t assigned in every possible execution path inside combinational logic.

**Problematic Example:**

```verilog
always @(a,b,sel)
    if (sel)
        y = a; // else missing → latch inferred
```

**Fix:** Include default assignment or complete else block.

---

## 3️⃣ Labs on Conditional Statements

**Lab Examples:**

* **Lab 1:** Incomplete `if` → generates latch
* **Lab 2:** Synthesis shows latch inference
* **Lab 3:** Nested `if-else` → partial assignments
* **Lab 4:** Synthesis output confirms missing paths
* **Lab 5:** Complete `case` statement → proper assignments
* **Lab 6:** Synthesized case → no latch
* **Lab 7:** Incomplete case handling → caution with wildcards
* **Lab 8:** Partial assignments within a case → ensure all outputs assigned

> Labs demonstrate how missing assignments can create unintended storage elements.

---

## 4️⃣ For Loops

Used in procedural blocks for repetitive operations. Only synthesizable if iteration count is fixed at compile-time.

**Example – 4-to-1 MUX:**

```verilog
integer i;
always @(data, sel) begin
    y = 0;
    for (i=0; i<4; i=i+1)
        if (i == sel)
            y = data[i];
end
```

---

## 5️⃣ Generate Blocks

Generate blocks are used for parameterized, repetitive hardware structures at compile time.

**Example:**

```verilog
genvar i;
generate
  for (i=0; i<4; i=i+1) begin : gen_loop
      and_gate u_and (.a(in[i]), .b(in[i+1]), .y(out[i]));
  end
endgenerate
```

---

## 6️⃣ Ripple Carry Adder (RCA)

A chain of full adders used to sum binary numbers. Each stage propagates the carry to the next.

**Example – 8-bit RCA using generate:**

```verilog
genvar i;
generate
  for (i=1; i<8; i=i+1)
    fa u_fa (.a(num1[i]), .b(num2[i]), .c(carry[i-1]), .co(carry[i]), .sum(sum[i]));
endgenerate

fa u_fa0 (.a(num1[0]), .b(num2[0]), .c(1'b0), .co(carry[0]), .sum(sum[0]));
assign sum[8] = carry[7];
```

---

## 7️⃣ Labs on Loops & Generate Blocks

* **Lab 9:** 4-to-1 MUX with `for` loop
* **Lab 10:** 8-to-1 Demux using `case`
* **Lab 11:** 8-to-1 Demux using `for` loop
* **Lab 12:** 8-bit RCA using generate block

> Reinforces writing compact, scalable RTL code that synthesizes efficiently.

---

## ✅ Key Takeaways

* Ensure all outputs in combinational blocks are assigned in every path.
* Prefer complete `if-else` and `case` statements to avoid unintended latches.
* Use `for` loops and `generate` blocks for repeatable structures.
* RCA design can be efficiently implemented with generate loops.
* Labs demonstrate practical application and synthesis behavior for safe, optimized RTL.

---
