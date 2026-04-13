# BParser

The BParser project allows for fast mathematical computations using Python syntax. The main interface is the Parser class.


# Parsing evaluation structure

The typical workflow of the Parser class is:  
1. Define
   * Set variables
   * Set result variable
   * Set parsed expression
2. Compile
3. Run

## Define
The Parser class constructor requires the `max_vec_size` integer.  
This specifies the maximum number of computed expressions per Parser.run() and the expected size of input variable pointers.  
Setting `max_vec_size` to 1 will make BParser work as expected by a regular math library. This is not the optimal way of computing expressions with a lot of data!  
Setting `max_vec_size` to a large number will cause BParser to allocate memory blocks which are too large for the CPU cache, making the computation inefficient.
The large number is dependent on the shape of input variables, SIMD size of target processor and CPU cache size.


The `set_variable`, `set_var_copy` and `set_constant` methods save their arguments in the `Parser.symbols_` map for later use.  
Parser class **requires** one variable of name "\_result\_". This is where the computed values will be saved.  
Processor will throw an exception if the "\_result\_" variable was not set or its shape does not match the result shape of the input expression.

`Parser.parse` takes the input expression and creates the Abstract Syntax Tree (AST) and sets 3 constants, *e*, pi and epsilon, which is the machine epsilon of `double`.  
It will throw an exception if the input expression doesn't follow the expression syntax or is using unknown (syntactic) symbols.  

After parsing, the `free_symbols` and `symbols` methods may be used to get all the symbols (variables) which are (not)used in the parsed expression.

## Compile

The `Parser.compile` method creates a directed acyclic graph (ExpressionDAG) from the AST and variables in the `Parser.symbols_` map.  
It will throw an exception if the AST has a variable which is not in the `symbols_` map.

It will also create a processor for the next step.

## Run
`Parser.set_subset` allows the user to specify which SIMD blocks from `max_vec_size` to compute. By default there is no subset.  
The input is a vector of indexes of the SIMD blocks. Each CPU can have different SIMD size.

Example to compute all for `max_vec_size = 16` where SIMD size = 2:
```cpp
Parser p(16);
...
p.set_subset({0,1,2,3,4,5,6,7});
```

`Parser.run` will run the processor, computing the compiled ExpressionDAG and saving the results in the `_result_` variable.
