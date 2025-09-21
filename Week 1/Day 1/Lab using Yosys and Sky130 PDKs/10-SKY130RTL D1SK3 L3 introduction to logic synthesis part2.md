# 📘 Chapter 1: Analysis of a Synthesised Two-Input Multiplexer (Mux)

> **Note:** The source material provided did not include timestamps.  
> The notes are structured based on the logical flow of the explanation.

---

## 1.1 Introduction: Circuit Overview
The primary goal of the analysis is to verify that a given logic circuit, composed of several standard cells, correctly implements the functionality of a **two-input multiplexer (MUX)**.  

- **Inputs:** `I0`, `I1`, and a **select line** (`select`, also referred to as `cell`)  
- **Output:** `Y`

---

## 1.2 Identification of Logic Gates (Standard Cells)
The circuit is constructed from various **logic gates**, each with a specific function.  
The library name prefix **CHI 130 FD SCHD** is common to all cells and can be ignored for logical analysis.

- **NAND2 Gate** → A standard two-input NAND gate.  
- **Inverter (Clock Inverter)** → Inverts the input signal.  
  - Example: `I0 → Inverter → I0'` (I0 bar)  
- **O2AI Gate (OR-AND-Invert)**  
  - This gate performs an OR on two inputs (`A1` and `A2`),  
    then AND with a third input (`B1`),  
    and finally inverts the result.  
  - **Boolean Expression:**  
    ```
    Output = ((A1 + A2) * B1)'
    ```

<img width="1920" height="1080" alt="Screenshot (310)" src="https://github.com/user-attachments/assets/04e9a30e-3706-4a8f-9ecb-444df464e1c3" />

---

## 1.3 Derivation of the Circuit's Boolean Expression
We trace the signals step by step from inputs to the final output.

### 1. Inputs
- `I0`  
- `I1`  
- `select` (a.k.a. `cell`)  

### 2. Intermediate Signal 1 (Output of O2AI gate)
- Inputs:  
  - `A1 = select`  
  - `A2 = I0'` (inverted I0)  
- Expression:  
(I0' + select)'


### 3. Intermediate Signal 2 (Output of NAND2 gate)
- Inputs: `I1` and `select`  
- Expression:  
(I1 . select)'


### 4. Final Output Y (Final NAND gate output)
- Inputs: Intermediate Signal 1 and Intermediate Signal 2  
- Expression:  

Y = [ (I1 . select)' . (I0' + select)' ]'


---

## 1.4 Simplification Using De Morgan's Theorem
To verify the circuit’s function, simplify using **Boolean algebra** and **De Morgan’s Theorem**.

### Step 1: Apply De Morgan’s Theorem to outer bar
Y = ((I1 . select)')' + ((I0' + select)')'


### Step 2: Remove double negations

Y = (I1 . select) + (I0' + select)'


### Step 3: Apply De Morgan’s Theorem to second term

(I0' + select)' = (I0')' . select' = I0 . select'


### Step 4: Substitute back

Y = (I1 . select) + (I0 . select')

<img width="1920" height="1080" alt="Screenshot (311)" src="https://github.com/user-attachments/assets/11964929-5413-4d69-99e7-bb6e266f1877" />

---

## 1.5 Conclusion: Verification of the MUX Function
- **Final Simplified Equation:**

Y = (I1 . select) + (I0 . select')


- This is the **standard Boolean equation for a 2-to-1 Multiplexer**:
- **Logic:**  
  `(Input1 AND Select) OR (Input0 AND NOT Select)`

- ✅ Verification:  
> *“This is exactly the equation of a two-input MUX.  
This is what we wanted to create and this is what we created here.”*

---

### ✅ Summary
The analysis proves that the arrangement of **NAND**, **Inverter**, and **O2AI** gates **correctly realises a two-input multiplexer**.

MUX Equation: Y = I1 . select + I0 . select'
