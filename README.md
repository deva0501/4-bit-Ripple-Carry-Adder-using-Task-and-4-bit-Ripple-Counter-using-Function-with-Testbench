# 4-bit-Ripple-Carry-Adder-using-Task-and-4-bit-Ripple-Counter-using-Function-with-Testbench
Aim:
To design and simulate a 4-bit Ripple Carry Adder using Verilog HDL with a task to implement the full adder functionality and verify its output using a testbench.
To design and simulate a 4-bit Ripple Counter using Verilog HDL with a function to calculate the next state and verify its functionality using a testbench.

Apparatus Required:
Computer with Vivado or any Verilog simulation software.
Verilog HDL compiler.

 Verilog Code for ripple carry adder 4bit:
```verilog
module ripple_4bit_adder (
    input [3:0] A,
    input [3:0] B,
    input Cin,
    output [3:0] Sum,
    output Cout
);
    reg [3:0] sum_temp;
    reg carry;
    integer i;

    assign Sum = sum_temp;
    assign Cout = carry;

    // Task for Full Adder
    task full_adder;
        input a, b, cin;
        output sum, cout;
        begin
            sum = a ^ b ^ cin;
            cout = (a & b) | (b & cin) | (a & cin);
        end
    endtask

    always @(*) begin
        carry = Cin;
        for (i = 0; i < 4; i = i + 1) begin
            full_adder(A[i], B[i], carry, sum_temp[i], carry);
        end
    end

endmodule
```
simulated output:
            ![WhatsApp Image 2025-04-12 at 14 08 15_ed20298d](https://github.com/user-attachments/assets/1e696034-3ee2-4c49-928d-c75d1de2e33a)

 Test bench code for Ripple carry adder 4bit:
```verilog
module ripple_carry_adder_4bit_tb;

    reg [3:0] A, B;
    reg Cin;
    wire [3:0] Sum;
    wire Cout;

    // Instantiate the ripple carry adder
    ripple_carry_adder_4bit uut (
        .A(A),
        .B(B),
        .Cin(Cin),
        .Sum(Sum),
        .Cout(Cout)
    );

    initial begin
        // Test cases
        A = 4'b0001; B = 4'b0010; Cin = 0;
        #10;
        
        A = 4'b0110; B = 4'b0101; Cin = 0;
        #10;
        
        A = 4'b1111; B = 4'b0001; Cin = 0;
        #10;
        
        A = 4'b1010; B = 4'b1101; Cin = 1;
        #10;
        
        A = 4'b1111; B = 4'b1111; Cin = 1;
        #10;

        $stop;
    end

    initial begin
        $monitor("Time = %0t | A = %b | B = %b | Cin = %b | Sum = %b | Cout = %b", $time, A, B, Cin, Sum, Cout);
    end

endmodule
```
simulated output:
          ![WhatsApp Image 2025-04-29 at 12 45 51_28fef259](https://github.com/user-attachments/assets/e6b84e4c-edbf-4755-b003-96618fe2294e)

 Verilog Code ripple carry counter 4bit:
 ```verilog
module ripple_carry_counter_4bit_function(
 input clk,
 input reset,
  output reg [3:0] Q 
   );

 function [3:0] next_state; 
 input [3:0] curr_state;
  begin 
  next_state = curr_state + 1; 
  end endfunction

// Sequential logic for counter 
always @(posedge clk or posedge reset) 
begin 
if (reset)
 Q <= 4'b0000; 
 
  else 
  Q <= next_state(Q);
   
   end

endmodule
```
simulated output:
        ![WhatsApp Image 2025-04-12 at 14 44 53_c312ba78](https://github.com/user-attachments/assets/10a0812f-c190-418c-9bbd-63b7fcb6133b)

Test Bench code for the ripple counter 4bit:
```verilog
module ripple_counter_4bit_tb;

    reg clk;
    reg reset;
    wire [3:0] Q;

    // Instantiate the ripple counter
    ripple_counter_4bit uut (
        .clk(clk),
        .reset(reset),
        .Q(Q)
    );

    // Clock generation (10ns period)
    always #5 clk = ~clk;

    initial begin
        // Initialize inputs
        clk = 0;
        reset = 1;

        // Hold reset for 20ns
        #20 reset = 0;

        // Run simulation for 200ns
        #200 $stop;
    end

    initial begin
        $monitor("Time = %0t | Reset = %b | Q = %b", $time, reset, Q);
    end

endmodule
```
simulated output:
       ![WhatsApp Image 2025-04-29 at 12 45 51_f4af81dc](https://github.com/user-attachments/assets/79457db7-b9d3-4bf3-be70-f048971e978e)

Conclusion:
The 4-bit Ripple Carry Adder was successfully designed and implemented using Verilog HDL with the help of a task for the full adder logic. The testbench verified that the ripple carry adder correctly computes the 4-bit sum and carry-out for various input combinations. The simulation results matched the expected outputs.

The 4-bit Ripple Counter was successfully designed and implemented using Verilog HDL. A function was used to calculate the next state of the counter.

