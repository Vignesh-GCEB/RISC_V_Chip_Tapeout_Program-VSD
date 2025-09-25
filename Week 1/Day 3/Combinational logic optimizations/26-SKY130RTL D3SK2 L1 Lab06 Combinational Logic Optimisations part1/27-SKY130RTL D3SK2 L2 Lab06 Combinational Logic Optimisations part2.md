# 🔧 Combinational Logic Optimisations (Part 2)

This lab demonstrates **combinational logic simplification** using HDL designs and synthesiser optimisations (likely with Yosys).  
Through case studies, we explore how synthesis tools apply **Boolean algebra** and **logic transformations** to generate more efficient circuits.

---

## 📖 Chapter 1: Combinational Logic Optimisations

### 1.1 Introduction to Logic Simplification
- Purpose: Show how synthesis tools simplify circuits described in Verilog.  
- Optimisations are done automatically using rules like **De Morgan’s Theorem**, constant propagation, and Boolean simplification.  

---

### 1.2 Example 1: Inverted-Input NAND Gate
**Initial Circuit Description**  
- Two inverters feed a NAND gate:  
\[
Y = (A') \; \text{NAND} \; (B')
\]

**Applying De Morgan’s Theorem**  
\[
A \cdot B = (A' + B')'
\]  
- Thus NAND(A’, B’) = A + B → equivalent to **OR gate**.  

**Optimisation Process**  
- Inputs were doubly inverted: (A')' = A, (B')' = B.  
- Back-to-back inversions cancel each other.  

**Final Result**  
- Circuit reduces to a single **OR gate**.  
- ✅ Verified in synthesis.  
<img width="1920" height="1080" alt="Screenshot (360)" src="https://github.com/user-attachments/assets/18b5a60b-5bcc-46ed-950a-64bd668483c6" />

---

### 1.3 Example 2: Multiplexer to 3-Input AND Gate (`opt_check_3`)

**HDL Description**  
- Output `y` depends on inputs `a`, `b`, `c`:  
  - If `a=0` → y=0  
  - If `a=1` & c=1 → y=b  
  - Else → y=0  

**Boolean Expression**  
\[
y = (a' \cdot 0) + a \cdot ((c' \cdot 0) + (c \cdot b))
\]

**Simplification**  
1. Inner: \((c' \cdot 0) + (c \cdot b) = b \cdot c\)  
2. Outer: \((a' \cdot 0) + (a \cdot (b \cdot c))\)  
3. Final:  
\[
y = a \cdot b \cdot c
\]

**Optimised Circuit**  
- ✅ Synthesis simplifies to a **3-input AND gate**.  
- Quote: *“bingo we have a 3 input and gate”*.  
<img width="1920" height="1080" alt="Screenshot (361)" src="https://github.com/user-attachments/assets/3120c6fc-e128-4cdf-b560-214eea534fb5" />

---

## 🛠️ Chapter 2: Synthesis Workflow and Commands

Standard synthesis flow for `opt_check_3`:
Step 1: Read Verilog design
read_verilog opt_check_3.v

Step 2: Set top module
synth -top opt_check_3

Step 3: Optimise
opt_clean -purge

Step 4: Map to library cells
abc -liberty ./mylib_sky130.lib

Step 5: Visualise schematic
show

---

## 🧪 Chapter 3: Further Lab Exercises

### 3.1 Single Module Optimisation (`opt_check_4`)
- Students should synthesise the module with the standard workflow.  
- Observe simplifications from **Verilog statements → gate-level netlist**.  

### 3.2 Multi-Module Optimisation
- **Objective**: Optimise hierarchical Verilog design across modules.  

**Workflow**:
1. **Flatten the hierarchy**:  
2. Optimise design:  
opt_clean -purge
3. Write final netlist.  

write_verilog final_netlist.v

- **Goal<img width="1920" height="1080" alt="Screenshot (362)" src="https://github.com/user-attachments/assets/39f86dc0-dcd3-460b-ac3d-58d9b5c387b4" />
**: Study the impact of optimisation **across module boundaries**.  

---

## ✅ Key Takeaways
- **De Morgan’s Theorem** & inversion cancellation simplify NAND-based logic into OR.  
- **Multiplexer conditions** simplified to a clean 3-input AND.  
- **Synthesis tools** recognise redundant constructs and rewire for efficiency.  
- **Multi-module designs** require flattening before global optimisation.  
- Commands such as `opt_clean -purge` and `abc` ensure minimal gate-level mapping.  

