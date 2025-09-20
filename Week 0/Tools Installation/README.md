
# 🖥️ RISC-V Reference SoC Tapeout Program – VSD 

  



## Tools Installation

#### All the instructions for installation of required tools can be found here:

### **System Requirements**
- 6 GB RAM
- 50 GB HDD
- Ubuntu 20.04 or higher
- 4 vCPU

### **Resizing the Ubuntu window to fit the screen**
```bash
$ sudo apt update
$ sudo apt install build-essential dkms linux-headers-$(uname -r)
$ cd /media/spatha/VBox_GAs_7.1.8/
$ ./autorun.sh
```

### **TOOL CHECK**

#### **Yosys**
```bash
$ sudo apt-get update
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys
$ sudo apt install make               # If make is not installed
$ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make config-gcc
# Yosys build depends on a Git submodule called abc, which hasn't been initialized yet. You need to run the following command before running make
$ git submodule update --init --recursive
$ make 
$ sudo make install
```
![Screenshot]("https://github.com/user-attachments/assets/29150df3-5e00-437d-bbfa-1fb51b9b80f0")

#### **Iverilog**
```bash
$ sudo apt-get update
$ sudo apt-get install iverilog
```
![Screenshot](<img width="1920" height="1080" alt="Screenshot (271)" src="https://github.com/user-attachments/assets/471c1d69-737b-4687-98e5-4320f85dc2ea" />
)

#### **gtkwave**
```bash
$ sudo apt-get update
$ sudo apt install gtkwave
```
![Screenshot]<img width="1920" height="1080" alt="Screenshot (272)" src="https://github.com/user-attachments/assets/d40900d2-d66e-478a-83ff-10e1876432ac" />
