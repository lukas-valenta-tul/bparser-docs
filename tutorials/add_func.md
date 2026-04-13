# Adding a math function to BParser

This tutorial talks about adding a new math function for Array represented tensors.  
To add a new math function for ScalarNode represented scalars see [adding a scalar function in BParser](https://github.com/lukas-valenta-tul/bparser-docs/).
If you only want to apply a scalar function elementwise for one Array or for an element pair from two Arrays, see [Adding an Array elementwise scalar function to the parser](#adding-an-array-elementwise-scalar-function-to-the-parser)

To add a math function to BParser:
1. Write your function
    - It must be static or defined in the bparser namespace
    - The return type must be `bparser::Array`
    - The name should match the desired name used in the parser expression
    - The parameters must be 1, 2 or 3 `bparser::Array` const type references. In place Array changes are not supported.
    - Example function definitions:
    ```cxx
    struct Array {
      //...
      static Array foo(const Array& a) {;}
      static Array bar(const Array& a, const Array& b){;}
      static Array baz(const Array& a, const Array& b, const Array& c){;}
      //...
    };
    ```
    - You may use the `bparser::Array::wrap_array()` function and `bparser::details::ScalarWrapper` class to take advantage of the [Eigen math library](https://libeigen.gitlab.io/eigen/docs-3.4/)
2. Add the function to the parser
    1. Open the `include/grammar_impl.hh` file
    2. Find the `func.add` function call at line number ~152 with the list of all other already implemented functions
    3. Add another line with the syntax `FN("parser_func_name", c++_func_ptr)`  
    Example:
    ```cxx
    func.add
      FN("abs"    , unary_array<_abs_>())
      //...
      FN("det"    , &Array::det)
      FN("inv"    , &Array::inv)
      FN("foo"    , &Array::foo)
      ;
    ```
3. Write a test to ensure your function works properly
   1. Open `test/test_parser.cc` and scroll down to the `test_expression` function.
   The order of the tests in the function is similar to the order of the parser definitions in the `include/grammar_impl.hh` file.
   2. Add relevant tests using the following:
       - `BP_ASSERT` macro to throw an exception when the test fails
       - `test_expr` function to test an expected result and optionally expected shape
       - `fail_expr` function to test an expected Exception message
       - Predefined BParser variables:
           - `as1` scalar array [1]
           - `av2` vector array [2,2,2]
           - `cs3` constant scalar [3]
           - `cv4` constant vector [4,5,6]
           - `bv5` scalar array [88,... ,133,134]
           - `bv6` scalar array [100,... ,122,123]
       - Example:
      ```cxx
      void test_expression() {
        //...
        BP_ASSERT(fail_expr("foo(cs3)"         , "Expected exception message here"));
	    BP_ASSERT(test_expr("a=[1,2,3]; foo(a)", {3,2,1}, {3}));
        //...
      }
      ```

## Adding an Array elementwise scalar function to the parser
To add a function to the parser which will apply a scalar function elementwise for one Array or for an element pair from two Arrays do:
1. Open the `include/grammar_impl.hh` file
2. Find the `func.add` function call at line number ~152 with the list of all other already implemented functions
3. Add another line with the syntax `FN("parser_func_name", unary_array<>)` for one Array elementwise function or
    `FN("parser_func_name", binary_array<>)` for two Array elementwise function  
    Example:
    ```cxx
    func.add
      FN("abs"    , unary_array<_abs_>()) //this will call the _abs_ scalar function for all elements
      //...
      FN("minimum", binary_array<_min_>()) //this will return an array containing the min value between both input Arrays
      /...
      ;
    ```
