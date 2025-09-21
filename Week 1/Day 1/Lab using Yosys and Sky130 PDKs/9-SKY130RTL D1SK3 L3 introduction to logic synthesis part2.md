# Logic Synthesis with YoSYS - Lab 3

## Chapter 1: Introduction to Logic Synthesis with YoSYS

### 1.1 Overview of the Synthesis Process

Logic synthesis is the process of converting a high-level description of a design, such as Verilog RTL (Register-Transfer Level), into an optimized gate-level netlist. This netlist is implemented using standard cells from a specific technology library.

- **Synthesizer Tool**: The tool used in this lab is YoSYS
- **Core Workflow**: The fundamental steps in YoSyS are:
  1. Read the Verilog design files
  2. Read the technology library files (.lib)
  3. Generate an output gate-level netlist
<img width="1920" height="1080" alt="Screenshot (306)" src="https://github.com/user-attachments/assets/b9344c35-2879-4c1d-bc6a-36477d7501f7" />


### 1.2 Setting Up the Environment

- **Invoking YoSYS**: The synthesizer is started from the command line by typing the command `yosys`
  - This assumes YoSYS has been installed, for example, as par<img width="1920" height="1080" alt="Screenshot (307)" src="https://github.com/user-attachments/assets/fd20b63f-346a-46d8-a3c1-ed8ce043abfc" />
t of the VSD (VLSI System Design) flow
- **Working Directory**: The commands are executed from the Verilog files folder, which is located within the main project directory cloned from GitHub
- **File Structure**:
  - **Verilog Files**: Contains the design files (e.g., `good_underscore_mux.v`)
  - **mylib**: Contains the technology library files needed for synthesis
<img width="1920" height="1080" alt="Screenshot (307)" src="https://github.com/user-attachments/assets/c369f53e-d95c-47e9-926e-c1ce48fba596" />

## Chapter 2: The YoSYS Synthesis Command Flow

This section details the sequence of commands used to perform synthesis on a Verilog design. The example used is a combinational multiplexer named `good_mux`.

### 2.1 Step 1: Reading the Technology Library

The first step is to load the standard cell library, which contains the definitions of the logic gates (standard cells) that the design will be built with.

- **Command**: `read_liberty -lib <path_to_library_file>`
- **Purpose**: This command reads the specified .lib file into YoSYS
- **Path**: The path to the library can be absolute or relative
- **Example**:
  ```
  read_liberty -lib ../mylib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
  ```
  > **Note**: The specific naming convention of the library file (e.g., FD, SC, HD, TT) corresponds to different process, voltage, and temperature (PVT) corners and will be explained in later sessions.

### 2.2 Step 2: Reading the Verilog Design

Next, the RTL design file(s) must be read into the tool.

- **Command**: `read_verilog <verilog_file_name>`
- **Example**: `read_verilog good_underscore_mux.v`
- **Verification**: A successful read is confirmed by the message: "successfully finished Verilog frontend"
- **Multiple Files**: If a design is spread across more than one Verilog file, all files must be read

### 2.3 Step 3: Specifying the Top Module

This step instructs YoSYS which module within the loaded design files is the top-level entity to be synthesized.

- **Command**: `synth -top <module_name>`
- **Purpose**: Sets the target module for the synthesis process
- **Example**:
  ```
  synth -top good_mux
  ```
  > The module `good_mux` is a purely combinational logic circuit, meaning it does not contain any sequential elements like flip-flops. If flops were present, an additional command would be required.

### 2.4 Step 4: Generating the Gate-Level Netlist

This is the core synthesis step where the RTL is mapped to the standard cells from the library.

- **Command**: `abc -liberty <path_to_library_file>`
- **Purpose**: The abc command takes the high-level design and realizes its logic using the standard cells defined in the provided liberty (.lib) file
- **Output**: This process generates a gate-level netlist and prints a summary report to the console
<img width="1920" height="1080" alt="Screenshot (308)" src="https://github.com/user-attachments/assets/66b03ede-9d88-4db3-9bec-de6ca0d601da" />

## Chapter 3: Analysing and Visualising the Synthesis Results

After synthesis, it is crucial to review the output to ensure the result is as expected.

### 3.1 The Synthesis Report

The `abc` command provides a report detailing how the design was synthesized. This is important for verification.

**Key Information in the Report**:

- **Inferred Signals**: It lists the number of inputs, outputs, and internal signals (wires) it identified
  - For the `good_mux` example, the tool correctly inferred:
    - 3 input signals (i0, i1, select)
    - 1 output signal
    - 0 internal signals, which matches the simple Verilog code

- **Standard Cells Used**: It provides a count of each type of standard cell used to implement the logic
  - For `good_mux`, the implementation used:
    - 1 clock inverter
    - 1 NAND2 (2-input NAND) gate
    - 1 O2AI (AND-OR-Invert) cell

### 3.2 Visualising the Synthesized Logic

YoSYS can generate a graphical representation of the gate-level netlist, which is useful for understanding the final circuit structure.

- **Command**: `show`
- **Purpose**: Displays a visual diagram of the synthesized logic
- **Result**: For the `good_mux` example, this command would show a schematic where the multiplexer's logic is built entirely from the Sky 130 library cells mentioned in the report (inverter, NAND2, O2AI)
<img width="1920" height="1080" alt="Screenshot (309)" src="https://github.com/user-attachments/assets/d950833a-6529-47b8-b702-eb555946195b" />

---

## Quick Reference Commands

| Step | Command | Purpose |
|------|---------|---------|
| 1 | `read_liberty -lib <path_to_lib>` | Load technology library |
| 2 | `read_verilog <verilog_file>` | Load Verilog design |
| 3 | `synth -top <module_name>` | Specify top module |
| 4 | `abc -liberty <path_to_lib>` | Generate gate-level netlist |
| 5 | `show` | Visualize synthesized logic |
