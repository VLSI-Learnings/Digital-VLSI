# Data types

**Vaule Set:**

* The 4 basic values in Verilog HDL are
  * 0 :logic-0orfalse
  * 1: logic-1 or true
  * x: unknown
  * z: high-impedance
* x and z are case insensitive.
* Constants are 3 types - Integer, Real, String.
  * Integer - represented in two formats
    * Simple decimal
      * 30, -15
    * Base format
      * [size]'base value
        * size - specifies the size of the constant in no of bits.
        ex: 4'b0101
      * Not legal formats
        * Space between ' character and base
        * Size can't be expression - (2+1)'b00000
        * value can't be negative
  * Real
    * Decimal format
      * 1.5, 2,670
    * Scientific format
      * 14_3.1e2 (=14310)(underscore is ignored)
      * 4e-2 (0.04)
  * String - A sequence of characters within the double quotes.
    * A character is represented by an 8-bit ASCII value which is treated as an unsigned integer.
    * The backslash (\\) character can be used to escape certain special characters.
    * \n - newline
    * \t - tab
    * \\ - \ character

    ```verilog
    message = "Internal Error";
    reg [8*14:0] message;
    ```

* An underscore (_) character can be used in an integer or a real constant freely; they are ignored in the number itself. They can be used to improve readability; the only restriction is that the underscore character cannot be the first character.
* A number in base format notation is always an unsigned number.
* If the size specified is larger than the no of bits for the constant, the number is padded to the left with 0's except for the case where the leftmost bit is a x or a z, in which a x or a z respectively is used to pad to the left.
  * For example:
    1. 10'b10 padded with 0 to the left, 0000000010
    2. 10'bx0x1 Paddedwith x to the left, xxxxxxx0x1
* If the size specified is smaller, then the leftmost bits are appropriately truncated.

**Data Types:**

* Verilog HDL has two groups of data types.

* **Net**
  * Represents the physical connection between the elements.
  * Its value is determined from the value of its drivers such as a continuous assignment or a gate output.
  * If no driver is connected to net, default value is z.
  * Net default size is 1 bit.
  * wire, tri, wor, trior, wand, triand, trireg, tril, tri0, supply0, supply 1
    Syntax

    ```verilog
    net_type [msb:lsb] var
    ```

  * wire and tri are similar types but tri is used when multiple drivers drive a net.

    | wire(or tri) | 0 | 1 | x | z |
    | :-: | - | - | - | - |
    | 0 | 0 | x | x | 0 |
    | 1 | x | 1 | x | 1 |
    | x | x | x | x | x |
    | z | 0 | 1 | x | z |
  * wor and trior

    | wor(or trior) | 0 | 1 | x | z |
    | :-: | - | - | - | - |
    | 0 | 0 | 1 | x | 0 |
    | 1 | 1 | 1 | 1 | 1 |
    | x | x | 1 | x | x |
    | z | 0 | 1 | x | z |
  * wand and triand

    | wand(or triand) | 0 | 1 | x | z |
    | :-: | - | - | - | - |
    | 0 | 0 | 0 | 0 | 0 |
    | 1 | 0 | 1 | x | 1 |
    | x | 0 | x | x | x |
    | z | 0 | 1 | x | z |
  * Trireg net
    * This net stores value and is used to model a capacitive node.
    * Default value of trireg net is an x.
  * Tri0 & Tri1
    * Thses nets also model wired logic nets.
    * The particular characteristic of a tri0 (or 1) is, if no driver is driving this net, its value is 0(or 1).

    | tri0(or tri1) | 0 | 1 | x | z |
    | :-: | - | - | - | - |
    | 0 | 0 | x | x | 0 |
    | 1 | x | 1 | x | 1 |
    | x | x | x | x | x |
    | z | 0 | 1 | x | 0(1) |
  * Supply0 & Supply1
    * The supplyO net is used to model ground, that is, the value 0, and the supplyl net is used to model a power net, that is, the value 1.
  * If there are any undeclared nets that can set to default net type by using complier directive `default_nettype.

  ```verilog
    * `default_nettype wand
  ```

  * Vectored & Scalared nets
    * If a net is declared with the keyword vectored, then bit-selects and part-selects of this vector net are not allowed.
    * For scalar those are allowed.

* **Register** - represents an abstract data  storage element.
  * It is assigned values only within an always statement or an initial statement, and its value is saved from one assignment to the next.
  * A register type has a default value of x.
  * The value in a register is always interpreted as an unsigned number.
  * There are five different kinds of register types:
    * reg
      * Default size is 1 bit.
      * Memory is an array of registers.

        ```verilog
        reg [msb:lsb]mem1[upperlimit:lowerlimit]
        ```

        ```verilog
        parameter ADDR_SIZE =16, WORD_SIZE = 8;
        reg [1: WORD_SIZE] RamPar [ADDR_SIZE-1 : 0] , DataReg;
        ```

        * RamPar is a memory, an array of sixteen 8-bit registers, while DataRegis a 8-bit register.
      * A memory cannot be assigned a value in one assignment(means value should be assigned to each location), but a register can.
      * To assign values to memory is b using the system tasks:
        1. $readmemb (loads binary values)
        2. $readmemh (loads hexadecimalvalues)
      * Thesesystem tasks read and load data from a specifiedtext file into a memory.
      * The text file must contain the appropriate form of numbers, either binary or hexadecimal.

        ```verilog
        reg [4:0]mem[15:0];
        $readmemb("file.txt",mem)
        ```

    * integer
      * default size of the integer is 32bits.
      * An integer register holds signed quantities and arithmetic operations provide 2's complement arithmetic results.
      * converting an integer to a bit-vector can be accomplished by simply using an assignment.
      * Type conversionis automatic. No specific functions are necessary.
    * time
      * is used to store and manipulate time values.
      * default size is 64  bits
      * A tme register holds only an unsigned quantity.
    * real
    * realtime
      * real and realtime registers are identical.
      * The default value of a real register is 0. No range,bit range or word range, is allowed for declaring a real register.
      * When assigning values x and z to a realregister,these values are treated as O.
* Parameter
  * Paramter is a constant, used to specify delays and widths of variables.
  * The value of the parameter can be overridden by
    * using a defparam statement
    * or by specifying the parameter value in the module instantiation statement.
