# Day 1: Introduction to Verilog RTL Design & Synthesis

Welcome to *Day 1 of the RTL Workshop!*  
In this session, I will begin with digital design journey by exploring:

- Verilog basics  
- Simulation using *Icarus Verilog (iverilog)*  
- Logic synthesis with *Yosys*  

By the end of today, i understand how to describe, simulate, and synthesize simple digital designs.

---

## 📑 Table of Contents
1. [Simulator, Design, and Testbench](#1-simulator-design-and-testbench)  
2. [Getting Started with iverilog](#2-getting-started-with-iverilog)  
3. [Lab: Simulating a 2-to-1 Multiplexer](#3-lab-simulating-a-2-to-1-multiplexer)  
4. [Verilog Code Walkthrough](#4-verilog-code-walkthrough)  
5. [Introduction to Yosys & Gate Libraries](#5-introduction-to-yosys--gate-libraries)  
6. [Lab: Synthesis with Yosys](#6-lab-synthesis-with-yosys)  
7. [Summary](#7-summary)  

---

## 1. Simulator, Design, and Testbench

- *Simulator* → A tool that verifies digital circuits by applying test inputs and observing outputs. Helps catch mistakes before hardware implementation.  
- *Design* → The actual Verilog code that defines logic behavior.  
- *Testbench* → A simulation environment that feeds inputs into your design and checks outputs.  

👉 Think of the *design* as the circuit and the *testbench* as the “tester” that checks if it works.

---

## 2.Getting Started with iverilog

*iverilog* is an open-source Verilog simulator.  

*Basic flow:*
1. Provide *design* and *testbench* files as inputs.  
2. Simulator runs and generates a .vcd file.  
3. Open .vcd in *GTKWave* to see waveforms.  

---

## 3.Lab: Simulating a 2-to-1 Multiplexer

### Step 1: Clone the workshop repo
```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
```

### Step 2: Install tools
```bash
  sudo apt install iverilog
  sudo apt install gtkwave
```

### Step 3: Run Simulation
```bash
iverilog mux2_1 test_mux2_1
gtkwave test_mux2_1
```

### Verilog code walkthrough
mux2_1.v
module mux2_1(
    input a,
    input b,
    input sel,
    output y
);
    assign y = sel ? b : a;  // if sel=1 choose b, else choose a
endmodule
Explanation:

Inputs: i0, i1 → data lines; sel → select signal

Output: y → chosen input

Logic: If sel=1, output y=i1; else y=i0.

---

### 4.Introduction to Yosys & Gate Libraries

Yosys converts Verilog to a gate-level netlist.

Features:
Synthesis, optimization, mapping, and visualization
Gate libraries contain multiple flavors to optimize:
Performance
Power
Area
Drive strength
---

### 6.Lab: Synthesis with Yosys

yosys
- read_liberty -lib /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
- read_verilog mux2_1.v
- synth -top mux2_1
- abc -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
show


---

7. Summary

- ✅ Learned simulators, designs, and testbenches
- ✅ Simulated a 2:1 multiplexer
- ✅ Analyzed Verilog code
- ✅ Explored Yosys synthesis and libraries

---

### 2️⃣ mux_2_1v content

```verilog
module mux2_1 (input i0, input i1, input sel, output reg y);
  always @(*) begin
    if (sel)
      y <= i1;
    else
      y <= i0;
  end
endmodule
```

---

3️   test_mux2_1.v content (example testbench)

module test_mux2_1;
  reg i0, i1, sel;
  wire y;

  good_mux uut (.i0(i0), .i1(i1), .sel(sel), .y(y));

  initial begin
    $dumpfile("tb_good_mux.vcd");
    $dumpvars(0, tb_good_mux);

    // Test cases
    i0 = 0; i1 = 0; sel = 0; #10;
    i0 = 0; i1 = 1; sel = 0; #10;
    i0 = 1; i1 = 0; sel = 1; #10;
    i0 = 1; i1 = 1; sel = 1; #10;

    $finish;
  end
endmodule


---
