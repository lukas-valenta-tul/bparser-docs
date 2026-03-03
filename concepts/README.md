# BParser

The BParser project allows for fast mathematical computations using Python syntax. The main interface is the Parser class.


# Parsing evaluation structure

The typical workflow of the Parser class is:  
1. Init
   * Set variables
   * Set result variable
   * Set parsed expression
2. Compile
3. Run
4. Read result variable

The `set_variable`, `set_var_copy` and `set_constant` methods save their arguments in the Parser.symbols_ map for later use.
Parser class **requires** one variable of name "\_result\_". This is where the computed values will be saved. Its shape does not matter.

Parser.parse takes the input expression and creates the Abstract Syntax Tree (AST) and sets 3 mathematical constants TODO: which.
After parsing the `symbols` and `free_symbols` methods may be used to get all the symbols/variables (not)used in the parsed expression.

compile
