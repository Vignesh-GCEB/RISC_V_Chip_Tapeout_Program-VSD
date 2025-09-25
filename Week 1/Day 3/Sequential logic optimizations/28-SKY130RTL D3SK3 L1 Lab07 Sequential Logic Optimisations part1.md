# 🧪 Lab 07: Sequential Logic Optimisations - Part 1

This lab explores how synthesis tools identify and optimise **sequential circuits**, particularly focusing on **sequential constants**. We compare two designs: one that remains a **true sequential circuit** and another that simplifies to a **constant**.

---

## 📖 Chapter 1: Introduction to Sequential Constants

### 1.1 Objective
- Analyse how synthesis tools handle sequential designs.  
- Understand the concept of **sequential constants**.  
- Compare a non-optimisable DFF vs. a constant-output DFF.  

### 1.2 Lab Files
- **Design Files**:  
  - `dff_const1.v`  
  - `dff_const2.v`  
- **Test Benches (TBs)**:  
  - Each design has a matching TB, e.g. `TB_dff_const1.v`, `TB_dff_const2.v`.  

---

## 🔄 Chapter 2: Case Study — `dff_const1.v` (Non-Constant Sequential Circuit)

### 2.1 Verilog Code Analysis
- **Always block**:  
always @(posedge clk or posedge rst)
- Behaviour:  
- If `rst=1` → Q = 0  
- Else on rising `clk` edge → Q = 1  
- Inference: D input of DFF = constant `1`.  

### 2.2 Waveform Analysis
- **Expected**:  
- During reset → Q = 0  
- When reset deasserted → Q changes only at **next clock rising edge**.  
- ❌ Common Misconception: It is **not** `Q = ~reset`.  
- Output is **synchronous with clock**, not asynchronous with reset.  

- **Simulation Result** (with iVerilog + GTKwave):  
- Even if reset goes low mid-cycle, Q stays `0` until the next `clk ↑`.  

### 2.3 Synthesis Result
- Synthesis infers a **standard DFF cell**:  
- D connected to constant `1`.  
- Reset/clock pins wired correctly.  
- Q output preserved.  
- ✅ Conclusion: Flip-flop **cannot be removed** since output changes are **clock-dependent** → fundamentally sequential.  

---

## 🔁 Chapter 3: Case Study — `dff_const2.v` (True Sequential Constant)

### 3.1 Verilog Code Analysis
- Circuit infers a DFF with:  
- **Set input** driving Q = 1 when asserted.  
- D input tied to `1`.  

### 3.2 Waveform Analysis
- **Expected**:  
- During reset/set → Q = 1.  
- After reset released + clock ticks → Q remains 1 (since D=1).  
- ✅ Conclusion: Q is **always 1** → a true sequential constant.  

- **Simulation Result**:  
- Q output confirmed **high at all times** in waveform viewer.  

---

## ⚙️ Chapter 4: Synthesis Flow for Sequential Designs

### 4.1 Tools Used
- **Synthesis**: Yosys  
- **Simulation**: iVerilog  
- **Waveform Viewer**: GTKWave  

### 4.2 Key Synthesis Commands (Yosys)

Load standard cell library
read_liberty -lib <path_to_lib>

Read Verilog design
read_verilog dff_const1.v # or dff_const2.v

Perform synthesis (infer DFFs)
synth -top <module_name>

Map generic DFFs to technology library cells
dfflibmap -liberty <path_to_lib>

Technology mapping
abc -liberty <path_to_lib>

Optional: Visualise netlist
show

**Important Step — `dfflibmap`:**
- Ensures **generic DFFs** from synthesis are mapped to **physical flip-flop cells** in the library.  
- Required since libraries often have **separate sequential/combinational cells** or **specialised flops** (with set/reset pins).  

---

## ✅ Key Takeaways

- **dff_const1.v**: Sequential flip-flop cannot be optimised → output changes only on clock → real sequential circuit.  
- **dff_const2.v**: Output is **always constant (1)** → tool optimises away the DFF, replacing it with a constant connection.  
- **Synthesis with yosys** requires `dfflibmap` to properly map sequential cells.  
- Sequential constant optimisation highlights how tools eliminate **redundant flops and logic**.  
