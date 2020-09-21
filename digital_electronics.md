# Digitial Electronics(NPTEL & Morris Mano)

## All the topics of Digital Electronics are here

* [Chapter 1](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/digital_electronics.md#chapter-1) - Basics
* [Chapter 2](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/digital_electronics.md#chapter-2) - Boolean Algebra and logic gates

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
* **Arthimetic Operations**
  * Addition
  * Subraction
  * Multiplication
  * Division
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
* **Data represention**
  * **Magnitude**
    * Unsigned
    * Signed
      * MSB is used as sign bit.
      * Range -(2^(n-1)-1) to 2^(n-1)
  * **complement**
    * 1's complement
      * Range -(2^(n-1)-1) to 2^(n-1)-1
    * 2's complement
      * Range -2^(n-1) to 2^(n-1)-1
* **Binary subtraction in complements**
  * 1's Complement
      s=A-B
    * Final carry in s is 1 then result = s+1
    * Final carry in s is 0 then result = 1's complement of s.
  This end around carry is an disadvantage so we use 2's complement.
  * 2's complement
     s=A-B
    * final carry = 1 then result=s
    * final carry = 0 then result=2's complement of s
* **Positive logic**
  * "1" level vaule of the variable > "0" level value
* **Negative logic**
  * "1" level value of the variable < "0" level value
* **Codes**
  * Group of symbols mainly numbers or letters.
    * Weighted code
      * BCD, 8421, 2421
    * Unweighted code
      * excess 3, gray code
    * Reflective code
      * 2421
    * Sequential code
      * Difference between 2 consecutive numbers is 1.
    * Alphanumeric code
      * ASCII(American Standard Code Information Interchange)
    * Error detecting & Correcting code
      * Hamming code
* **BCD**
  * Packed BCD
    * Representation of decimal >9 in BCD
  * BCD is less eficient than binary since BCD requires more no.of bits
  * But the convertion are easy from decimal to BCD
  * **BCD Addition**
    * sum>=9 then add 6 to it.
* **Excess 3**
  * BCD + 3
* **Gray code**
  * Unit distance code and minimum error code
  * This is mainly used in switching operations
* **ASCII CODE**
  * **Control characters** are used for routing data and arranging the printed text into a prescribed format. There are three types of control characters:
    * Formal effectors
      * Characters that control the layout of printing. They include the familiar word processor and type writer controls such as backspace(BS), horizontal tabulation(HT), and carriage return (CR).
    * Information separators
      * Used to the data into divisions such as paragraphs and pages. They include characters such record separator(RS ) and file separator (FS ).
    * Communication control characters
      * are useful during the transmission of text between remote terminals.
      * Ex: STX(start of text) and ETX (end of text) which are used to frame a textmessage transmitted through telephone wires.
    * Printable characters
    * Non Printable characters
* **Error Detecting code**
  * Parity bit - The extra bit included in the bit stream.(This can detect only when odd no.of bits are changed)
    * Even parity - The bit should such that overall 1's be even.
    * Odd parity - The bit should such that overall 1's be odd.
* **Code convertions**

### Chapter 2



### Chapter 5

* **Binary cell** - A physical device that has two stable states and capable of storing one bit.
* **Register** - A group of binary cells
* A ***register transfer*** operation is a basic operation that consists of a transfer of binary information from one set of registers into another set of registers.
