# Digitial Electronics(NPTEL & Morris Mano)

## All the topics of Digital Electronics are here

* [Chapter 1](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/digital_electronics.md#chapter-1) - Basics
* [Chapter 2](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/digital_electronics.md#chapter-2)

### Chapter 1

* Definitions
  * **Signal** - Information that represents the variation of physical quantity with respect to a parameter.
  * **Analog Signal** - The dependent parameter that can any value defined within the limits as a function of the independent parameters.
  * **Discrete Signal** - The dependent parameter is defined for particular values of the independent parameter.
  * **Digital Signal** - The signal which is discrete in both dependent parameter and independent parameter.
  * **Transducer** - The device which converts non electrical signal to electric signal.
  * **Digital System** - The system the manipulates the discrete elements of information represented in binary form.

* Y's
  * Noise immunity
    * Because of the levels, eventhough when the noise is added cancelled due to the levels.
  * Less use of bandwidth.
  * Efficiency of long distance transmission.
* Binary digit 0 or 1 is **BIT**

### Number System

"The set of values used to represent a quantity"

* **Base** - The no.of distinct in the particular number system.
* Pure Binary System - Non negative number representation.

  |Decimal|Binary|Octal|Hexadecimal|
  |:--:|:--:|:--:|:--:|
  |(base 10)|(base 2)|(base 8)|(base 16 )|
  |00|0000|00|0|
  |01|0001|01|1|
  |02|0010|02|2|
  |03|0011|03|3|
  |04|0100|04|4|
  |05|0101|05|5|
  |06|0110|06|6|
  |07|0111|07|7|
  |08|1000|10|8|
  |09|1001|11|9|
  |10|1010|12|A|
  |11|1011|13|B|
  |12|1100|14|C|
  |13|1101|15|D|
  |14|1110|16|E|
  |15|1111|17|F|

* **Weighted Number System** - Each position of the number is represents a particular value.
  ex: Binary, Decimal, BCD.
* **Unweighted Number System** - There is no particular value for the positions.
  ex: Gray code, Excess-3 code.

* **Number system conversions**

* **Complement**
  * Complements are used in digital computers to simplify the subtraction operation and for logical manipulation.
  * Simplifying operations leads to simpler and less expensive circuits to implement the operations.
  * There are two types of complements for each base-r system:
    * Radix complement - r's complement
      * **r's complement** = (r^n-N), n is no.of digits in N.
    * Diminished radix complement - (r - 1 )'s complement.
      * **(r-1)'s complement** = (r^n-N-1)
        |r's complement = (r-1)'s complement + 1|
        |---|
