# RTL Workshop — Day 5

Day 5 introduces *counters* and highlights best practices in RTL coding.  
We study how to design a synchronous up-counter, simulate it, and view waveforms.

---

## Learning Objectives

- Understand *counters* as sequential logic building blocks.
- Explore *reset behavior* and *enable control*.
- Strengthen the rule:  
  - Sequential logic → use **non-blocking (<=).  
  - Combinational logic → use **blocking (=).
- Observe simulation waveforms and verify counter operation.

---

## Design Description

for the *4-bit up-counter* with:

- Clock input (clk)  
- Active-high synchronous reset (reset)  
- Enable signal (en)  
- Output count (count[3:0])  

The counter increments only when en = 1.  

---

## Files

### counter.v
```verilog
// counter.v
// 4-bit synchronous up-counter
module counter (
    input  wire clk,
    input  wire reset,   // synchronous reset
    input  wire en,      // enable
    output reg [3:0] count
);
    always @(posedge clk) begin
        if (reset) begin
            count <= 4'b0000;    // reset to 0
        end else if (en) begin
            count <= count + 1;  // increment when enabled
        end
    end
endmodule
```
---

counter_tb.v

// counter_tb.v
```
`timescale 1ns/1ps
module counter_tb;
    reg clk, reset, en;
    wire [3:0] count;

    // instantiate DUT
    counter dut (
        .clk(clk),
        .reset(reset),
        .en(en),
        .count(count)
    );

    // generate clock
    initial clk = 0;
    always #5 clk = ~clk;   // 10ns clock

    initial begin
        $dumpfile("counter.vcd");
        $dumpvars(0, counter_tb);

        // initial conditions
        reset = 1; en = 0; #10;   // reset active
        reset = 0; en = 1; #50;   // enable counter
        en = 0; #20;              // hold count (pause)
        en = 1; #40;              // resume counting
        reset = 1; #10;           // reset again
        reset = 0; en = 1; #30;   // final run

        $display("Counter test completed");
        $finish;
    end
endmodule
````


---
