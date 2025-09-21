# Chapter 1: The Design File - A Multiplexer (Mux) Example

## 1.1 Module Overview
- The design file contains a Verilog module named `good_mux`.
- This module is presented as an illustrative example; it is noted that there are multiple ways to code a multiplexer in Verilog.

## 1.2 Module Interface (Inputs & Outputs)
- **Inputs**: The module has three inputs:
  - I0: Input 0  
  - I1: Input 1  
  - select: The selection line that determines which input is passed to the output.  

- **Output**: The module has a single output:  
  - Y: The module's output signal.  

## 1.3 Functional Behaviour
The core function of the `good_mux` module is to select an input based on the `select` signal.

- If `select` is HIGH (1): The output `Y` will take the value of input `I1`.  
- If `select` is LOW (0): The output `Y` will take the value of input `I0`.  

---

# Chapter 2: The Test Bench File

## 2.1 Core Purpose and Characteristics
- A test bench is a Verilog module used to test another module (the design).  
- A key feature is that the test bench instantiates the design it is intended to test.  
- Crucially, a test bench does not have any primary inputs or outputs of its own.  

## 2.2 Design Instantiation (UUT/DUT)
- The process of creating an instance of the design module inside the test bench is called instantiation.  
- The instance is commonly given a standard name:  
  - UUT: Stands for Unit Under Test.  
  - DUT: Stands for Design Under Test.  
- These terms are described as "very, very famous terminology" used in design verification.  

## 2.3 Stimulus Generation
- The stimulus generator is the section of the test bench code responsible for creating and applying input signals to the design under test.  

- **Initial Conditions**:  
  An initial block sets the starting values for the signals. In this example, `select`, `I0`, and `I1` are all set to 0 at the beginning of the simulation.  

- **Applying Stimulus Over Time**:
  - `always` blocks are used to change the input signals at specific time intervals.  
  - In the example, the `select` signal is toggled every 75 nanoseconds (`select = ~select`).  
  - The inputs `I0` and `I1` are also toggled after specific time durations to test the design's response.  

## 2.4 Simulation Control
- The duration of the simulation can be explicitly set.  
- The command `$finish` is used to signal the end of the simulation.  
- To change the simulation time, you can modify the value in the test bench (e.g., from 300 nanoseconds to 500 or 100).  

## 2.5 Output Observation
- This specific test bench does not contain a stimulus observer (code within the test bench to automatically check for correct outputs).  
- Instead, it uses a different method for verification:  
  1. **Dump VCD File**: The test bench is configured to dump all signal activities into a VCD (Value Change Dump) file.  
  2. **Visual Inspection**: The VCD file is then opened with a waveform viewer tool, GTKWave, allowing the user to visually observe the output waveforms and verify the design's behaviour manually.  
