# Lab 2 – iVerilog and GTKWave Guide

---

## 📘 Chapter 1: Introduction to Lab 2

### 1.1: Lab Objective  
- The primary goal of this lab is to understand how to work with the **iVerilog simulator** ⚡ and the **GTKWave waveform viewer** 📊.  

### 1.2: File Structure and Naming Conventions 📂  
- All design files are located in a folder named **Verilog files**.  
- This folder contains two main types of files:  
  - **Design Files:** These are the actual Verilog modules (e.g., `goodmux.v`).  
  - **Test Bench Files:** These files are used to test the design files.  

- There is a one-to-one correspondence between a design file and its test bench.  
  - Example: The test bench for `goodmux.v` is `tb_goodmux.v`.  
  - Example: The test bench for `badlatch.v` is `tb_badlatch.v`.  

---

## ⚙️ Chapter 2: The Simulation Workflow  

This chapter outlines the two-step process for simulating a Verilog design using the command line.  

### 2.1: Step 1 – Compilation with iVerilog  
- **Purpose:** Compile the Verilog design and its associated test bench into an executable file.  
- **Command:**  


- **Example:**  


- **Output:** Generates a default executable file named `a.out`.  

### 2.2: Step 2 – Execution and Waveform Generation  
- **Purpose:** Run the compiled simulation, generating a waveform data file.  
- **Command:**  


- **Output:** Produces a **VCD (Value Change Dump)** file containing all the signal data required for waveform viewing.  

---

## 📊 Chapter 3: Waveform Analysis with GTKWave  

This chapter covers how to view and analyse the VCD file generated during simulation.  

### 3.1: Launching the Waveform Viewer  
- **Purpose:** Open the VCD file in a graphical interface for analysis.  
- **Command:**  


### 3.2: The GTKWave Interface 🖥️  
- **Signal Hierarchy:**  
  - The top-level module is the Test Bench, displayed as **TB**.  
  - Inside the test bench is the design being tested, referred to as the **UUT (Unit Under Test)**.  

- **Viewing Signals:**  
  - Drag and drop signals from the signal list into the main waveform panel to view their waveforms.  

### 3.3: Essential Analysis Tools 🔍  
- **Zoom Fit:**  
  - Fits the entire simulation timeline into the viewing window.  
  - Especially useful for long simulations (e.g., 300 ns run with 1 ps timescale).  

- **Zooming:**  
  - Click on a specific region of the waveform to zoom in or zoom out.  

- **Tracing Transitions:**  
  - Helps navigate quickly to points where a signal changes value.  
  - Select a signal, then use the forward/backward arrow buttons to jump between transitions (e.g., low to high).  

---

## 🧪 Chapter 4: Case Study – Verifying a Multiplexer (`goodmux.v`)  

### 4.1: Mux Functionality  
The multiplexer’s output `Y` is controlled by a select line:  

- When `select = 0`, output `Y` follows input `I0`.  
- When `select = 1`, output `Y` follows input `I1`.  

### 4.2: Waveform Verification  
- **Scenario 1: `select = 0`**  
  - The waveform shows `Y` follows `I0`.  
  - Changes in `I1` do not affect `Y`.  

- **Scenario 2: `select = 1`**  
  - Output `Y` switches to follow `I1`.  
  - It immediately stops following `I0`.  

- **Scenario 3: `select = 0` again**  
  - Output `Y` returns to following `I0`.  
