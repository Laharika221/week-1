# RTL Workshop — Day 3

This repository contains a simple RTL design example for Day 3 of the RTL Workshop.  
It's directly clone this repo, paste the files, and run the commands.

---


## Goal

- Learn how to write a basic combinational module (2:1 multiplexer).
- Write a testbench for it.
- Run simulation and check waveforms.

---

## Setup

Install tools:

- *Ubuntu/Debian*
  ```bash
  sudo apt update
  sudo apt install -y iverilog gtkwave

macOS (Homebrew)

brew install icarus-verilog gtkwave



---

Files

mux2.v
```
// mux2.v
// 2-to-1 multiplexer
```
module mux2 (
    input  wire a,
    input  wire b,
    input  wire sel,
    output wire y
);
    assign y = sel ? b : a;
endmodule
```
mux2_tb.v

// mux2_tb.v
```
`timescale 1ns/1ps

module mux2_tb;
    reg a, b, sel;
    wire y;

    mux2 dut (
        .a(a),
        .b(b),
        .sel(sel),
        .y(y)
    );

    initial begin
        $dumpfile("mux2.vcd");
        $dumpvars(0, mux2_tb);

        a = 0; b = 0; sel = 0; #10;
        a = 1; b = 0; sel = 0; #10;
        a = 0; b = 1; sel = 0; #10;
        a = 1; b = 1; sel = 0; #10;

        sel = 1; #5;
        a = 0; b = 0; #10;
        a = 1; b = 0; #10;
        a = 0; b = 1; #10;
        a = 1; b = 1; #10;

        #10;
        $display("Testbench finished");
        $finish;
    end
endmodule
```
```
Makefile

# Makefile
SIM = iverilog
SIMARGS = -g2012

all: sim

sim: mux2.v mux2_tb.v
	$(SIM) $(SIMARGS) -o simv mux2.v mux2_tb.v
	./simv

wave: sim
	gtkwave mux2.vcd &

clean:
	rm -f simv mux2.vcd

.PHONY: all sim wave clean
```

---


From the Day_3 directory:
Run simulation:
```
make sim
```
Open waveform:
```
make wave
```
Clean:
```
make clean
```


---

Expected Output
```
Testbench finished
```
---

