# Combinational Circuits

* **Combinational circuit**
  * A combinational circuit consists of logic gates whose outputs at present time are determined from only the present combination of inputs.
* **Design Procedure**
  * The design of combinational circuits starts from the specification of the design objective and culminates in a logic circuit diagram or a set of Boolean functions from which the logic diagram can be obtained. The procedure involves the following steps:
    1. From the specifications of the circuit. determine the required number of inputs and outputs and assign a symbol to each.
    2. Derive the truth table that defines the required relationship between inputs and outputs
    3. Obtain the simplified Boolean functions foreach output as a function of the input variables.
    4. Draw the logic diagram and verify the correctness of the design (manually or by simulation).
* **Adders**
  * **Half adder** - A combinational circuit that performs the addition of two bits.
  * **Full adder** - One that performs the addition of three bits (two significant bits and a previous carry)
  * **Ripple Carry Adder** - n-bit full adder
  * **Carry Look Ahead adder**
    * To reduce the delay in carry propagation we use two funtions known as
    * **Carry generate (G)** - and it produces a carry of 1 when both A(i) and B(i), are 1 regardless of the input carry.

      |G(i)=A(i)B(i)|
      |--|
    * **Carry propagate (P)** - because it determines whether a carry in stage i will propagate into stage i+1.

      |P(i)=A(i) xor B(i)|
      |--|
* **Subtractor**
  * **Half Subtractor** - A combinational circuit that performs the subtraction of two bits.

      |Diff=A xor B|Borrow=A'B|
      |--|--|
  * **Full Subtractor** - A combinational circuit that performs the subtraction of two bits.

      |Diff=A xor B xor Bin|Borrow=A'B+Bin(A xnor B)|
      |--|--|
  * **Overflow**
    * The extra bit that occured during the addition of the two data.
    * This occurs only when the addition of two numbers if they are both positive or negative.
* **BCD Adder**
* **Binary Multiplication**
* **Comparator**

    |xi = AiBi + Ai'Bi'; for i = 0,1,2,3|( A = B) = x3x2x1x0|
    |--|--|

    |(A > B) = A3B3' + x3A2B2' + x3x2A1B1' + x3x2x1A0B0'
    |--|

    |( A < B) = A3'B3 + x3A2'B2 + x3x2A1'B1 + x3x2x1A0'B0
    |--|
* **Decoder** is a combinational circuit that converts binary information from n inputs  to a maximum of 2^n unique output lines.
  * A decoder with enable input can function as a demultiplexer - a circuit that receives information from a single line and directs it to one of 2^n possible output lines.
* **Encoder**
  * Priority encoder
* **Mutliplexer**
  * A multiplexer is a combinational circuit that selects binary information from one of many input lines and directs it to a single output line.
  * The selection of a particular input line is controlled by a set of selection lines.
* **Three state gates**(Tristate buffer)
  * Digital circuit that exhibit three states.
  * Two states are equivalent to logic 1 and 0.
  * Third states is high impedance staes in which
    * the logic behaves like an open circuit.
    * the circuithas no logic significance.
    * the circuitconnected to the output of the three state gate is not affected by the inputs of the gate.
