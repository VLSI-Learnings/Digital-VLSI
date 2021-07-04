# Expression and Operators

* Expressions
  * An expression is formed using operands and operators. An expression can be used wherever a value is expected.
  * An operand can be one of the following:
    1. Constant
       * The negative value of an integer with no base specifier is treated as a signed value, while an integer with a base specifier is treated as an unsigned value.
    2. Parameter
    3. Net
       * A value in a net is interpreted as an unsigned value, in the continuous assignment.
    4. Register
       * A value in an integer register is interpreted as a signed 2's complement number,
       * while a value in a reg register or a time register is interpreted as an unsigned number.
       * Values in real and realtime registersare interpreted as signed floating point values.
    5. Bit-select
       * A bit-select extracts a particular bit from a vector.
    6. Part-select
       * In a part-select, a continuous sequence of bits of a vector is selected
    7. Memory element
       * A memory element selects one word of a memory
       * No part-select or bit-select of a memory is allowed.
       * to read a bit-select or a part-select of a word in memory is to assign the memory element to a register and then use a part-select or a bit-select of this register.
    8. Functional call
       * A function call can be used in an expression. It can either be a system function call (starts with the $ character) or a user-defined function call.
* **Operators**
  * Operatorsin Verilog HDL are classified into the following categories:
    1. **Arithmetic operators**

        |Symbol|operation|
        |-|-|
        |+|Unary plus|
        |-|Unary minus|
        |+|Binary plus|
        |-|Binary minus|
        |*|Multiply|
        |/|Divide|
        |%|Modulus|
       * The % (modulus) operator gives the remainder with the sign of the first operand.
       * If any bit of an operand in an arithmetic operation is an x or a z, the entire result is an x.
       * The size of the result of an arithmetic expression is determined by the size of the largest operand.
       * In case of an assignment, it is determined by the size of the left-hand side target as well.
       * Unsigned & signed
         * When performing arithmetic operations and assignments, it is important to note which operands are being treated as unsigned values and which are being treated as signed values.
         * An unsigned value is stored in:
           * a net
           * a reg register
           * an integer in base format notation
         * A signed value is stored in:
           * an integer register
           * an integer in decimal form
    2. **Relational operators**

        |Symbol|operation|
        |-|-|
        |<|Less than|
        |<=|Less than or equal to|
        |>|Greater than|
        |>=|Greater than or equal to|
        * The result of a relational operator is true(the value 1) or false (the value 0).
        * Result is an x if any bit in either of the operands is an x or a z
    3. **Equality operators**

        |Symbol|operation|
        |-|-|
        |==|Logical equality|
        |!=|Logical inequality|
        |===|Case equality|
        |!==|Case inequality|
        * In case comparisons, values x and z are compared strictly as values, that is, with no interpretations, and the result can never be an unknown,
        * while in logical comparisons, values x and z have their usual meaning and the result may be unknown; that is, for logical comparisons if either operand contains an x or a z, the result is the unknown value (x).

          ```verilog
          a=4'b11x0
          b=4'b11x0
          a==b   //gives x
          a===b  //gives 1
          ```

        * If the operands are of unequal lengths, the smaller operand is zero-filled on the most significant side.
    4. Logical operators

        |Symbol|operation|
        |-|-|
        |&&|Logical and|
        |\|\||Logical or|
        |!|Unary logical negation|
        * For vector operands, a non-zero vector is treated as a 1.
        * If a bit in any of the operands is an x, the result is also an x.
    5. Bitwise operators

        |Symbol|operation|
        |-|-|
        |&|Bit-wise and|
        |^|Bit-wise xor|
        |^~ or ~^|Bit-wise xnor|
        |\||Bit-wise or|
        |~|Unary bit-wise negation|
        * These operators operate bit-by-bit, on corresponding bits of the input operands and produce a vector result.
    6. Reduction operators

        |Symbol|operation|
        |-|-|
        |&|Reduction and|
        |~&|Reduction nand|
        |^|Reduction xor|
        |^~ or ~^|Reduction xnor|
        |\||Reduction or|
        |~\||Reduction nor|
        * The reduction operators operate on all bits of a single operand and produce a 1-bit  result.
        * The operators are:
          * & (reduction and): If any bit is 0, the result is 0,else if any bit is an x or a z, the result is an x, else the result is a 1.
          * ~& (reduction nand): Invert of & reduction operator.
          * | (reduction or): If any bit is a 1,the result is 1,else if any bit is an x or a z, the result is an x, else the result is 0.
          * ~| (reduction nor): Invert of | reduction operator.
          * ^ (reduction xor): If any bit is an x or a z, the result is an x, else if there are even number of l's in the operand,the result is 0, else the result is 1.
          * ~^ (reduction xnor): Invert of ^ reduction operator.
    7. Shift operators

        |Symbol|operation|
        |-|-|
        |<<|Left shift|
        |>>|Right shift|
        * The shift operation shifts the left operand by the right operand number of times. It is a logical shift. The vacated bits are filled with 0. If the right operand evaluates to an x or a z, the result of the shift operation is an x.
    8. Conditional operators

        |?:|Conditional operator|
        |-|-|
        * cond_expr ? exprl : expr2
        * If cond_expr is true (that is, has value 1), exprl is selected,if cond_expr is false (value 0), exprl is selected.If cond_expr is an x or a z, the result is a bitwise operation on exprl and exprl.
    9. Concatenation and replication operator

        |{}|concatenation|
        |-|-|
        * Concatenation of unsized constant numbers is not allowed as the size of these numbers are not known.
  * All operators associate left to right except for the conditional operator that associates right to left.
* Kinds of expressions
  * A **constant expression** is an expression that evaluates to a constant value at compile time. More specifically, a constant expression can be made up of:
    1. constant literals, such as 'b10 and 326
    2. parameter name
  * A **scalar expression** is an expression that evaluates to a 1-bit result. If a scalar result is expected but the expression produces a vector result, the least significant bit of the vector is used(the rightmost bit).
