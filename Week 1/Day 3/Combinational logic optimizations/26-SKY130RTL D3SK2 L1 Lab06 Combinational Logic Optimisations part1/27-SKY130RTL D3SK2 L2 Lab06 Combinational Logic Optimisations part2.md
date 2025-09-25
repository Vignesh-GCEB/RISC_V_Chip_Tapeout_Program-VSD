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
- ✅ Verified synthesis.
<img width="1920" height="1080" alt="Screenshot (360)" src="https://github.com/user-attachments/assets/73a8a76e-d769-415f-b369-fa2919b1ae58" />


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
<img width="1920" height="1080" alt="Screenshot (361)" src="https://github.com/user-attachments/assets/ccd898c2-2751-4598-aba3-9dbbf3c50c06" />

**Optimised Circuit**  
- ✅ Synthesis simplifies to a **3-input AND gate**.  
- Quote: *“bingo we have a 3 input and gate”*.  

---<img width="1920" height="1080" alt="Screenshot (362)" src="https://github.com/user-attachments/assets/f5a83255-07f9-4ba6-afc9-d3931def253f" />


## 🛠️ Chapter 2: Synthesis Workflow and Commands

Standard synthesis flow for `opt_check_3`:

