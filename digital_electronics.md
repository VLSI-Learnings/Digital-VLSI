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
  * ex: Binary, Decimal, BCD.
* **Unweighted Number System** - There is no particular value for the positions.
  * ex: Gray code, Excess-3 code.
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

#### Boolean Algebra

* The common postulates that formulates the algebric structures.
  * Closure
  * Associative
  * Commutative
  * Identity
  * Inverse
  * Distributive
  * De-morgan's law

    |(x + y)' = x' . y'|(x.y)' = x' + y' |
    |--|--|
* Huntington postulates:
  1. The structure is closed with respect to the operator '**+**' and '**.**'.
  2. (a) The element 0 is an identity element with respect to '+' that is

      |x + 0 = 0 + x = x|
      |--|

     (b) The element I is an identity element w ith respect to '.' that is

      |x · 1 = 1 · x = x|
      |--|

  3. (a) The structure is commutative with respect to '+' that is

      |x.y=y.x|
      |--|

     (b) The structure is commutative with respect to '.' that is

      |x.y=y.x|
      |--|

  4. (a) The operator . is distributive over '+' that is

      |x · (y+z) = (x . y) + ( x . z)|
      |--|

     (b) The operator + is distributive over '.' that is

      |x + (y . z) = (x + y) . ( x + z)|
      |--|
  5. For every element x in B there exists an element x' in B (called the complement of x) such that

      |x + x' = 1|x . x' = 0|
      |--|--|
  6. There exist at least two elements x, y in B such that

      |x != y|
      |--|
* ***Literal*** - The single variable in the term of the boolean expression.
* ***Binary logic*** - The variables with logic operations that has two discrete values.
* ***Truth table*** - The represents the information of output for all the combinations of inputs.
* ***Logic gate*** - A physical device that performs logical on one or more binary inputs and produces a single output.
  * **Basic gates: (AND, OR, NOT)**
    * Any digital circuit can be implemented using these gates.
  * **Universal gates:(NAND, NOR)**
    * Any logic can be implemented using only these gates.
  * **Arthimetic gates:(XOR, XNOR)**
    * Used in arthimetic operations
* **Duality**
  * The expressions that are deduciable from the postulates of the boolean algebra are remain valid if the operands and identity elements are interchanged.
    * **Self dual** - The same expression is obtained when the operands are interchaged two times.
* **Complement** - Negation of the function.
  * This can be obtained by taking negation for the whole function
  * First take the dual form and complement each literal
  * ex:
    f=x'y'z
    dual form f=x'+y'+z
    f'=x+y+z'
* **Operator precedence** - The operator precedence for evaluating Boolean expressions is (1) parenthesis, (2) NOT, (3) AND and (4) OR.
* **Minterm**(Standard product) - The term of literals in their normal or complemented form and their product is 1.
* **Maxterm**(Standard sum) - The term of literals in their normal or complemented form and their sum is 0.
* **Canonical form** - Boolean functions expressed as a sum of minterms or product of maxterms.
* **Standard form**
  * **SOP**(Sum of Products) - Boolean function expressed as ORing of product terms with single or multiple literals.
  * **POS**(Products of Sum) - Boolean function expressed as ANDing of sum terms with single or multiple literals.
* **Factors** considered for building of logic gates
  1. Feasibility and economy of producing the gate with physical components.
  2. The possibility of extending the gate to two or more than two inputs.
  3. The basic properties of the binary operator, such as commutativity and associativity, etc
  4. The ability of the gate to implement Boolean functions alone or in conjunction with other gates.
* **Positive logic**
  * "1" level vaule of the variable > "0" level value
* **Negative logic**
  * "1" level value of the variable < "0" level value
* Levels of Integration
  * Digital circuits are often categorized according to the complexity of their circuits as measured by the number of logic gates in a single package .
  * Small-scale integration (SSI)
  * Medium-scale integration (MSI)
  * Large-scale integration (LSI)
  * Very large-scale integration (VLSI)
* **Digital Logic Family**(More in [chapter 10](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/digital_electronics.md#chapter-10))
  * Each logic family has its own basic electronic circuit upon which more complex digital circuits and components are developed.
  * TIL - transistor-transistor logic
  * ECL - emitter-coupled logic
    * systems requiring high speed operation
  * MOS - metal-oxide semiconductor
    * circuit that needs high component density
  * CMOS - complementary metal-oxide semiconductor.
    * systems that need low power comsumption
* **Fan-out** - The no.of standard loads that the output of the gate can drive without impairing its operation.
* **Fan-in** - The no.of inputs available in a gate.
* **Power disspation** - The power consumed by the gate available from the power supply.
* **Propagation Delay** - The average transition delay for the signal to propagate from input to output.
* **Noise Margin** - The maximum external noise voltage added to the signal that does not cause an undesirable state to the output.

### Chapter 3

### Chapter 5

* **Binary cell** - A electronic physical device that has two stable states and capable of storing one bit.
* **Register** - A group of binary cells
* A ***register transfer*** operation is a basic operation that consists of a transfer of binary information from one set of registers into another set of registers.
