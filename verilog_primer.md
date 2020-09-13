# Verilog HDL Primer by J B

* [Definitions](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/verilog_primer.md#definitions)
* [Chapter 1](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/verilog_primer.md#chapter-1) - Introduction
* [Chapter 2](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/verilog_primer.md#chapter-2) - Syntax

## Definitions

**What is HDL?**\
HDL is a specialized computer language used to describe the structure and behavior of electronic circuits, and most commonly, digital logic circuits.

**Why HDL?**

1. Concurrent execution of statements.
2. The language that converts the program(textual description) to a Netlist(Gate level netlist)
3. Some characteristics like propagation delay, interconnection of parts can’t be captured with traditional languages.
    **Why need of concurrent processng?**
      1. Due to the complexity of digital circuits, we need to process all the instructions at the same time.
      2. To reduce the delay in output.

## Chapter 1

**What is Verilog HDL?**\
Verilog HDL is a hardware description language that can be used to model a digital systema many levels of abstraction ranging from the algorithmic level to the gate-level to the switch-level.\
(The Verilog HDL language was first developed by Gateway Design Automation(acquired by Cadence Design Systems) in 1983).

**Capabilities:**

1. Primitive logic gates, such as and, or and nand, are built-in into the language.
2. Flexibility of creating a user-defined primitive (UDP). Such as primitive could either be a combinational logic primitive or a sequential logic primitive.
3. Switch-level modeling primitive gates, such as pmos and nmos, are also built-in into the language.
4. Hierarchical designs can be described, up to any level, using the module instantiation construct.
5. Notion of concurrency and time can be explicitly modeled.
6. The language is non-deterministic under certain situations,that is, a model may produce different resultson different simulators; for example, the ordering of events on an event queue is not defined by the standard.
7. Explicit language constructs are provided for specifyingpin-topin delays, path delays and timing checks of a design.
8. The capabilities of the Verilog HDL language can be further extended by using the programming language interface (PLI) mechanism. **PLI is a collection of routines that allow foreign functions to access information within a Verilog module and allows for designer interaction with the simulator.**
9. A design can be modeled in three different styles or in a mixed style. These styles are:
   1. Behavioral style - modeled using procedural constructs.
   2. Dataflow style - modeled using continuous assignments.
   3. Structural style - modeled using gate and module instantiations.

## Chapter 2(Suntax)