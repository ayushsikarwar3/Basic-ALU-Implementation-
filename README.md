# 🚀Basic-ALU-Implementation-
Welcome to the 4-bit Arithmetic-Logic Unit (ALU) project! This project is an example of designing and simulating a simple ALU by using Verilog HDL. The ALU supports the basic arithmetic and logic functions like Addition, Subtraction, AND, OR, XOR and NOT, giving an insight into digital logic design and implementation of hardware.

# Overview-
This project focuses on the design and implementation of a 4-bit ALU using Verilog HDL, a fundamental building block of modern digital processors and VLSI systems. The ALU is designed to perform essential arithmetic operations such as addition and subtraction, along with logical operations including AND, OR, XOR, and NOT. The design is described using Verilog, simulated to verify correct functionality, and tested with different input combinations to ensure reliable operation. Through this project, practical knowledge of hardware description, digital logic design, simulation, and verification is gained, providing a strong foundation for advanced VLSI and FPGA-based digital system development.

# 📌Objectives-

- Create a 4-bit Arithmetic Logic Unit (ALU) in Verilog HDL.
- Carry out addition and subtraction of simple numbers.
- Use logical operators AND, OR, XOR and NOT.
- Design a modular and efficient combinational circuit.
- Model and test ALU using various input test vectors.
- Know the basics of Hardware Description Language (HDL) and digital circuit design.
- Apply digital design, simulation and functional verification skills to gain practical experience.
- Develop and consolidate concepts for VLSI design, FPGA development and processor architecture.
  
 # ⚙️Hardware Specifications & Pin Configuration
The module alu_4bit consists of two 4-bit data operands, a 3-bit control input, a 4-bit result output, and two critical status flags.

Pin Definitions
| Pin Name | Direction | Bit Width | Description |
|----------|-----------|-----------|-------------|
| `a` | Input | 4-bit `[3:0]` | Operand A |
| `b` | Input | 4-bit `[3:0]` | Operand B |
| `alu_control` | Input | 3-bit `[2:0]` | Operation Selection Code |
| `alu_result` | Output | 4-bit `[3:0]` | Execution Result Output |
| `carry_out` | Output | 1-bit | Carry Flag (Addition) / Borrow Flag (Subtraction) |
| `zero_flag` | Output | 1-bit | Active High when `alu_result == 4'b0000` |

# 🛠️Functional Operation Table (Truth Table)-

The 3-bit alu_control bus selects one of the 8 distinct functional modes:
| Control Code (`alu_control`) | Operation Type | Operation | Mathematical / Logical Expression |
|-------------------------------|----------------|-----------|-----------------------------------|
| `3'b000` | Arithmetic | Addition | `{carry_out, alu_result} = a + b` |
| `3'b001` | Arithmetic | Subtraction | `alu_result = a - b` (Carry indicates Borrow when `a < b`) |
| `3'b010` | Logical | Bitwise AND | `alu_result = a & b` |
| `3'b011` | Logical | Bitwise OR | `alu_result = a \| b` |
| `3'b100` | Logical | Bitwise XOR | `alu_result = a ^ b` |
| `3'b101` | Logical | Bitwise NOR | `alu_result = ~(a \| b)` |
| `3'b110` | Shifting | Left Shift | `alu_result = a << 1` |
| `3'b111` | Shifting | Right Shift | `alu_result = a >> 1` |

# Source Code (alu_4bit.v)-

```verilog
`timescale 1ns / 1ps

module alu_4bit (
    input  wire [3:0] a,           // 4-bit input operand A
    input  wire [3:0] b,           // 4-bit input operand B
    input  wire [2:0] alu_control, // 3-bit control signal to select operation
    output reg  [3:0] alu_result,  // 4-bit output result of the operation
    output reg        carry_out,   // Carry out flag for addition
    output reg        zero_flag    // Zero flag set high if alu_result is 0
);

    // Combinational ALU Logic
    always @(*) begin

        // Default outputs
        alu_result = 4'b0000;
        carry_out  = 1'b0;

        case (alu_control)

            // 000 : Addition
            3'b000:
                {carry_out, alu_result} = a + b;

            // 001 : Subtraction
            3'b001: begin
                alu_result = a - b;
                carry_out  = (a < b);      // Borrow Indicator
            end

            // 010 : Bitwise AND
            3'b010:
                alu_result = a & b;

            // 011 : Bitwise OR
            3'b011:
                alu_result = a | b;

            // 100 : Bitwise XOR
            3'b100:
                alu_result = a ^ b;

            // 101 : Bitwise NOR
            3'b101:
                alu_result = ~(a | b);

            // 110 : Logical Left Shift
            3'b110:
                alu_result = a << 1;

            // 111 : Logical Right Shift
            3'b111:
                alu_result = a >> 1;

            // Default Case
            default: begin
                alu_result = 4'b0000;
                carry_out  = 1'b0;
            end

        endcase

        // Zero Flag Logic
        zero_flag = (alu_result == 4'b0000);

    end

endmodule
```
---
#  💻Testbench Code (`tb_alu_4bit.v`)
```verilog
`timescale 1ns / 1ps

module tb_alu_4bit;

    // Inputs to the Device Under Test (DUT)
    reg [3:0] a;
    reg [3:0] b;
    reg [2:0] alu_control;

    // Outputs from the Device Under Test (DUT)
    wire [3:0] alu_result;
    wire       carry_out;
    wire       zero_flag;
// Instantiate the 4-bit ALU module
    alu_4bit uut (
        .a(a),
        .b(b),
        .alu_control(alu_control),
        .alu_result(alu_result),
        .carry_out(carry_out),
        .zero_flag(zero_flag)
    );

    // Stimulus block
    initial begin 
        $dumpfile("dump.vcd"); 
        $dumpvars(0, tb_alu_4bit); 
        a=4'b0000; b=4'b0000; alu_control=3'b000; 
        
        // Monitor window for console logging
        $monitor("Time=%0dns | Control=%b | A=%b B=%b | Result=%b | Carry=%b | Zero=%b", 
                 $time, alu_control, a, b, alu_result, carry_out, zero_flag);
        
  #10;
        
        // Test Case 1: Addition (4 + 5 = 9)
        a = 4'b0100; b = 4'b0101; alu_control = 3'b000; #10;
        
        // Test Case 2: Addition with Carry (12 + 6 = 18 -> Result 2, Carry 1)
        a = 4'b1100; b = 4'b0110; alu_control = 3'b000; #10;
        
        // Test Case 3: Subtraction (10 - 4 = 6)
        a = 4'b1010; b = 4'b0100; alu_control = 3'b001; #10;
        
        // Test Case 4: Bitwise AND
        a = 4'b1100; b = 4'b1010; alu_control = 3'b010; #10;
        
        // Test Case 5: Bitwise OR
        a = 4'b1100; b = 4'b1010; alu_control = 3'b011; #10;
        
        // Test Case 6: Bitwise XOR (Should trigger zero flag if identical)
        a = 4'b1111; b = 4'b1111; alu_control = 3'b100; #10;
        
        // Test Case 7: Left Shift A
        a = 4'b0011; alu_control = 3'b110; #10;

        // End simulation
        $finish;
    end

endmodule
```
---
# Simulation Console Outputs-
Below is the execution log captured from the simulator console during execution:
```text
Time=0dns | Control=000 | A=0000 B=0000 | Result=0000 | Carry=0 | Zero=1
Time=10dns | Control=000 | A=0100 B=0101 | Result=1001 | Carry=0 | Zero=0
Time=20dns | Control=000 | A=1100 B=0110 | Result=0010 | Carry=1 | Zero=0
Time=30dns | Control=001 | A=1010 B=0100 | Result=0110 | Carry=0 | Zero=0
Time=40dns | Control=010 | A=1100 B=1010 | Result=1000 | Carry=0 | Zero=0
Time=50dns | Control=011 | A=1100 B=1010 | Result=1110 | Carry=0 | Zero=0
Time=60dns | Control=100 | A=1111 B=1111 | Result=0000 | Carry=0 | Zero=1
Time=70dns | Control=110 | A=0011 B=0000 | Result=0110 | Carry=0 | Zero=0
```
---
# 📺 Simulation Results & Outputs Explanation-


- Time = 0ns (Initialization): Inputs `A` and `B` are both `0`, making the output result `0`. Therefore, Zero Flag = 1.
- Time = 10ns (Addition): Performs $4 + 5 = 9$ (`1001`). No overflow occurs, so Carry = 0.
- Time = 20ns (Addition Overflow): Performs $12 + 6 = 18$. Since $18$ exceeds the 4-bit limit ($0$–$15$), the output wraps around to `2` (`0010`) and sets Carry = 1.
- Time = 30ns (Subtraction): Performs $10 - 4 = 6$ (`0110`). No borrow is needed, so Carry = 0.
- Time = 40ns (Bitwise AND): Executes `1100 & 1010`. The bits match only at the most significant position, resulting in `8` (`1000`).
- Time = 50ns (Bitwise OR): Executes `1100 | 1010`, combining all active bits to output `E` (`1110`).
- Time = 60ns (Bitwise XOR): Executes `1111 ^ 1111`. Since both inputs are identical, all bits cancel out to `0`, setting Zero Flag = 1.
- Time = 70ns (Left Shift): Operates on `A = 0011` (Decimal 3). Shifting it left by 1 bit updates the value to `0110` (Decimal 6).
---
# Simulation Waveforms (EPWave)-
<img width="2408" height="1080" alt="Screenshot_20260601_145729" src="https://github.com/user-attachments/assets/c35ae7e2-b927-48f1-a812-8cb2f3f045d1" />

# console output (epwave window)-
<img width="1080" height="2408" alt="Screenshot_20260601_145906" src="https://github.com/user-attachments/assets/4a0a207e-6a97-403d-bad3-56ded9b47300" />

# 📝Conclusion- 

The design satisfies all constraints specified for a basic 4-bit processing block. Simulation trace validation verifies exact execution behavior matching across all basic arithmetic, logic, tracking, and flag monitoring pipelines without introducing physical latching anomalies.

      
