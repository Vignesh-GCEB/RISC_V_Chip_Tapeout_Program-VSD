# Chapter 1: Introduction to iVerilog, Simulation, and Test Benches

This chapter covers the fundamental concepts of digital design simulation, introducing the roles of a simulator, the design itself, and the test bench used for verification.

---

## 1.1 The Role of a Simulator
- **Definition:** A simulator is a tool used for checking and verifying a digital design.  
- **Purpose:** The primary goal is to ensure that the RTL (Register-Transfer Level) design adheres to its intended specifications (spec) by simulating its behaviour.  
- **Tool in Focus:** The course will use the iVerilog simulator.  

### 1.1.1 Core Working Principle
- Simulators are event-driven; they look for changes on the input signals of a design.  
- The output is only re-evaluated when there is a change in the input.  
- **Crucial Point:** If there are no changes to the inputs, the simulator will not re-evaluate or change the outputs.  

---

## 1.2 The Design
- **Definition:** The Design is the actual Verilog code (or set of codes) that implements the intended functionality to meet the required specifications.  
- **Structure:**  
  - A design can be spread across one or more files.  
  - It has a set of primary inputs (one or more) and primary outputs (one or more).  
  - **Example:** An inverter is a simple design with one primary input.  

---

## 1.3 The Test Bench (TB)
- **Definition:** A Test Bench (often abbreviated as TB) is the setup created to apply stimulus to a design and check its functionality against the specification.  

### 1.3.1 Purpose and Components
- **Functionality:** The test bench applies stimuli, also known as test vectors, to the design's primary inputs. It then observes the design's primary outputs and matches them to the expected results defined in the spec.  
- **Core Components:**  
  - **Stimulus Generator:** Creates and applies the input signals to the design.  
  - **Stimulus Observer:** Captures and checks the output signals from the design.  
- **Instantiation:** The design itself is instantiated within the test bench, which acts as a wrapper around it.  

### 1.3.2 Key Characteristics
- A critical distinction is that a test bench has no primary inputs or primary outputs of its own.  
- All inputs are generated internally, and all outputs are observed internally.  
- Only the design has external primary I/O.  

---

## 1.4 iVerilog-Based Simulation Flow
This section outlines the practical workflow for simulating a design using iVerilog.

- **Step 1: Inputs to the Simulator**  
  - Both the design file(s) and the test bench file are provided as input to the iVerilog simulator.  

- **Step 2: Simulation and Output**  
  - The simulator processes the inputs, evaluating outputs based on changes to the inputs.  
  - The output of the simulation is a **VCD (Value Change Dump)** file.  
  - The VCD format is named this way because the file only records the changes in the values of the signals over time, which is exactly what a simulator tracks.  

- **Step 3: Viewing the Results**  
  - The VCD file, which contains the simulation data, is viewed using a separate waveform viewer tool.  
  - The recommended tool for this is **GTKWave**.  
  - Using GTKWave, you can visually inspect the waveforms of the inputs and outputs to verify the functionality of the design.  
