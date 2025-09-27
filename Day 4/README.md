# RTL Workshop — Day 4

Day 4 of the RTL workshop focuses on *sequential logic* and the important concepts of **blocking (=) vs non-blocking (<=) assignments** in Verilog.  
We also run simulations and observe how different coding styles affect results.

---

## Topics Covered

1. *Blocking vs Non-Blocking Assignments*
   - = (blocking) executes statements sequentially, one after another.
   - <= (non-blocking) schedules updates in parallel at the end of the current simulation time step.
   - *Rule of thumb:*
     - Use **non-blocking (<=)** inside always @(posedge clk) for sequential logic.
     - Use **blocking (=)** for combinational logic or temporary variables inside procedural blocks.

2. *Sequential Logic — Register Example*
   - A simple *D-Flip-Flop / Register* with synchronous reset.
   - Captures input d on the rising edge of the clock.
   - Clears to 0 when reset is high.

3. *Simulation & Waveform Observation*
   - We simulate using *Icarus Verilog*.
   - Results dumped to register.vcd and viewed in *GTKWave*.
   - We observe:
     - Output q follows input d at each rising clock edge.
     - When reset=1, output q is forced to 0.
     - Difference in waveform when using = vs <=.

---

## Design Files

### register.v
verilog
// register.v
// Simple D register with synchronous reset
```
module register (
    input  wire clk,
    input  wire reset,  // synchronous reset
    input  wire d,
    output reg  q
);
    always @(posedge clk) begin
        if (reset) begin
            q <= 1'b0;      // non-blocking assignment
        end else begin
            q <= d;         // capture data on clock edge
        end
    end
endmodule
```
---

register_tb.v

// register_tb.v
```
`timescale 1ns/1ps
module register_tb;
    reg clk;
    reg reset;
    reg d;
    wire q;

    // instantiate DUT
    register dut (
        .clk(clk),
        .reset(reset),
        .d(d),
        .q(q)
    );

    // clock generation
    initial clk = 0;
    always #5 clk = ~clk;  // 10 ns clock period

    initial begin
        $dumpfile("register.vcd");
        $dumpvars(0, register_tb);

        // Apply stimulus
        reset = 1; d = 0; #10;   // reset active
        reset = 0; d = 1; #10;   // capture 1
        d = 0; #10;              // capture 0
        d = 1; #10;              // capture 1 again

        reset = 1; #10;          // force reset mid-cycle
        reset = 0; d = 0; #10;   // resume normal operation

        #20;
        $display("Simulation finished");
        $finish;
    end
endmodule
```
---

Key Takeaways

Always use non-blocking (<=) assignments in sequential (clocked) always blocks.

Blocking (=) is safe for temporary combinational code.

Simulation waveforms are critical to validate correct register behavior.



---
