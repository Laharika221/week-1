# Day 2 – RTL Workshop  

## 📌 Overview  
Day 2 focuses on building fundamental *RTL design skills* using Verilog.  
We will explore:  
- Writing simple Verilog modules  
- Testbench creation and simulation  
- Understanding simulation outputs (waveforms)  

This day builds on Day 1 basics and prepares us for more complex designs in upcoming sessions.  

---

## 🛠 Topics Covered  
1. *Combinational Logic in Verilog*  
   - Basic gate-level modeling  
   - Example: AND, OR, NOT implementation  

2. *Sequential Logic*  
   - D flip-flop with reset  
   - Testbench for D flip-flop  

3. *Simulation Flow*  
   - Compile → Run → View Waveforms  
   - Tools: iverilog, vvp, gtkwave  

---

## ✍ Example 1 – AND Gate  

### Verilog Code (and_gate.v)  
```verilog
module and_gate (
    input a, b,
    output y
);
  assign y = a & b;
endmodule
```
Testbench (and_gate_tb.v)
```
module and_gate_tb;
  reg a, b;
  wire y;

  and_gate uut (.a(a), .b(b), .y(y));

  initial begin
    $dumpfile("and_gate.vcd");
    $dumpvars(0, and_gate_tb);

    a = 0; b = 0; #10;
    a = 0; b = 1; #10;
    a = 1; b = 0; #10;
    a = 1; b = 1; #10;

    $finish;
  end
endmodule
```
### Simulation Steps
- Copy code
- Bash
- iverilog -o and_gate_tb.vvp and_gate.v and_gate_tb.v
- vvp and_gate_tb.vvp
- gtkwave and_gate.vcd
- Expected Waveform
- Output y is 1 only when both a and b are 1.
### ✍ Example 2 – D Flip-Flop with Asynchronous Reset
Verilog Code (d_ff_async.v)
```
module d_ff_async (
    input d, clk, reset,
    output reg q
);
  always @(posedge clk or posedge reset) begin
    if (reset)
      q <= 0;
    else
      q <= d;
  end
endmodule
```
Testbench (d_ff_async_tb.v)
```
module d_ff_async_tb;
  reg d, clk, reset;
  wire q;

  d_ff_async uut (.d(d), .clk(clk), .reset(reset), .q(q));

  initial begin
    $dumpfile("d_ff_async.vcd");
    $dumpvars(0, d_ff_async_tb);

    clk = 0;
    forever #5 clk = ~clk;  // clock with 10 time units period
  end

  initial begin
    reset = 1; d = 0; #12;   // apply reset
    reset = 0; d = 1; #10;   // store d=1
    d = 0; #10;              // store d=0
    reset = 1; #10;          // reset clears output
    reset = 0; d = 1; #10;   // store d=1 again
    $finish;
  end
endmodule
```
### Simulation Steps
iverilog -o d_ff_async_tb.vvp d_ff_async.v d_ff_async_tb.v
vvp d_ff_async_tb.vvp
gtkwave d_ff_async.vcd
Expected Waveform
When reset = 1, output q = 0 immediately (async reset).
On posedge clk, q follows input d.
✅ Learning Outcomes
- Understood basic combinational & sequential RTL design.
- Gained hands-on practice with simulation flow.
- Verified functionality using waveforms.
