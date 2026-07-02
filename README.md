# Basic-ALU-Implementation-
Welcome to the 4-bit Arithmetic-Logic Unit (ALU) project! This project is an example of designing and simulating a simple ALU by using Verilog HDL. The ALU supports the basic arithmetic and logic functions like Addition, Subtraction, AND, OR, XOR and NOT, giving an insight into digital logic design and implementation of hardware.

# Overview-
This project focuses on the design and implementation of a 4-bit ALU using Verilog HDL, a fundamental building block of modern digital processors and VLSI systems. The ALU is designed to perform essential arithmetic operations such as addition and subtraction, along with logical operations including AND, OR, XOR, and NOT. The design is described using Verilog, simulated to verify correct functionality, and tested with different input combinations to ensure reliable operation. Through this project, practical knowledge of hardware description, digital logic design, simulation, and verification is gained, providing a strong foundation for advanced VLSI and FPGA-based digital system development.

# Objectives-

- Create a 4-bit Arithmetic Logic Unit (ALU) in Verilog HDL.
- Carry out addition and subtraction of simple numbers.
- Use logical operators AND, OR, XOR and NOT.
- Design a modular and efficient combinational circuit.
- Model and test ALU using various input test vectors.
- Know the basics of Hardware Description Language (HDL) and digital circuit design.
- Apply digital design, simulation and functional verification skills to gain practical experience.
- Develop and consolidate concepts for VLSI design, FPGA development and processor architecture.
  
 # Hardware Specifications & Pin Configuration
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

# Functional Operation Table (Truth Table)-

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

`timescale 1ns / 1ps

module alu_4bit (
    input  wire [3:0] a,             // 4-bit Operand A
    input  wire [3:0] b,             // 4-bit Operand B
    input  wire [2:0] alu_control,   // Operation Select
    output reg  [3:0] alu_result,    // ALU Result
    output reg        carry_out,     // Carry/Borrow Flag
    output reg        zero_flag      // Zero Flag
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
      
       
# Result-

The design and simulation of 4-bit ALU with basic logic gates, full adders and multiplexers has been successfully performed without using Verilog. The circuit correctly added and subtracted numbers, as well as AND, OR and XOR logic. They were able to get the expected outputs for different combinations of input, thereby proving that the design is working as intended.

# Applications-

- Used in processors and microcontrollers for performing arithmetic and logical operations.
- Forms a basic building block of digital systems and embedded devices.
- Assists in learning digital electronics, computer architecture and VLSI design.
- Ideal for teaching projects, hardware circuit design.
  
# Skills Gained-

- Better knowledge of digital logic and combinational circuits.
- Know how to design circuits with logic gates, full adder and multiplexer.
- Acquired knowledge of circuit simulation and output verification.
- Enhanced problem solving and digital circuit design abilities.
# Future Improvements-

- Improve the design to 8-bit or 16-bit for better performance.
- Include additional operations like NAND, NOR, XNOR and bit shifting.
- Use status flags such as Zero, Overflow and Negative.
- Design the same ALU in Verilog and program it into an FPGA to test the logic in actual hardware.
# Author

- Ayush Sikarwar|btech electronics and communication
- linkedin-https://www.linkedin.com/in/ayush-sikarwar-a89878413?utm_source=share_via&utm_content=profile&utm_medium=member_android

# License

This project is licensed under the MIT License.

