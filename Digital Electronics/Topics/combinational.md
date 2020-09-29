# Combinational Circuits

## All about the combinational circuits

* [Chapter 1](/Digital%20Electronics/Topics/combinational.md#chapter-1)

### Chapter 1

* **Combinational circuit**
  * A combinational circuit consists of logic gates whose outputs at present time are determined from only the present combination of inputs.
* **Design Procedure**
  * The design of combinational circuits starts from the specification of the design objective and culminates in a logic circuit diagram or a set of Boolean functions from which the logic diagram can be obtained. The procedure involves the following steps:
    1. From the specifications of the circuit. determine the required number of inputs and outputs and assign a symbol to each.
    1. Derive the truth table that defines the required relationship between inputs and outputs
    1. Obtain the simplified Boolean functions foreach output as a function of the input variables.
    1. Draw the logic diagram and verify the correctness of the design (manually or by simulation).
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
