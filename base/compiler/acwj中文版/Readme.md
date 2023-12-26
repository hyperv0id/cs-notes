# A Compiler Writing Journey

In this Github repository, I'm documenting my journey to write a
self-compiling compiler for a subset of the C language.
I'm also writing out the details so that,
if you want to follow along, there will be an explanation of what
I did, why, and with some references back to the theory of compilers.

But not too much theory, I want this to be a practical journey.

Here are the steps I've taken so far:

 + [Part  0](00-Introduction.md):  Introduction to the Journey
 + [Part  1](01-Scanner.md):       Introduction to Lexical Scanning
 + [Part  2](02-Parser.md):        Introduction to Parsing
 + [Part  3](03-Precedence.md):    Operator Precedence
 + [Part  4](04-Assembly.md):      An Actual Compiler
 + [Part  5](05-Statements.md):    Statements
 + [Part  6](06-Variables.md):     Variables
 + [Part  7](07-Comparisons.md):   Comparison Operators
 + [Part  8](08-If_Statements.md): If Statements
 + [Part  9](09-While_Loops.md):   While Loops
 + [Part 10](10-FOR%20Loops.md):     For Loops
 + [Part 11](11-Functions_pt1.md): Functions, part 1
 + [Part 12](12-Types_pt1.md):     Types, part 1
 + [Part 13](13-Functions_pt2.md): Functions, part 2
 + [Part 14](14-ARM_Platform.md):  Generating ARM Assembly Code
 + [Part 15](15-Pointers_pt1.md):  Pointers, part 1
 + [Part 16](16-Global_Vars.md):   Declaring Global Variables Properly
 + [Part 17](17-Scaling_Offsets.md): Better Type Checking and Pointer Offsets
 + [Part 18](18-Lvalues_Revisited.md): Lvalues and Rvalues Revisited
 + [Part 19](19-Arrays_pt1.md):    Arrays, part 1
 + [Part 20](20-Char_Str_Literals.md): Character and String Literals
 + [Part 21](21-More_Operators.md): More Operators
 + [Part 22](22-Design_Locals.md): Design Ideas for Local Variables and Function Calls
 + [Part 23](23-Local_Variables.md): Local Variables
 + [Part 24](24_Function_Params.md): Function Parameters
 + [Part 25](25_Function_Arguments.md): Function Calls and Arguments
 + [Part 26](26_Prototypes.md):    Function Prototypes
 + [Part 27](27_Testing_Errors.md): Regression Testing and a Nice Surprise
 + [Part 28](28_Runtime_Flags.md): Adding More Run-time Flags
 + [Part 29](29_Refactoring.md):   A Bit of Refactoring
 + [Part 30](30_Design_Composites.md): Designing Structs, Unions and Enums
 + [Part 31](31_Struct_Declarations.md): Implementing Structs, Part 1
 + [Part 32](32_Struct_Access_pt1.md): Accessing Members in a Struct
 + [Part 33](33_Unions.md):        Implementing Unions and Member Access
 + [Part 34](34_Enums_and_Typedefs.md): Enums and Typedefs
 + [Part 35](35_Preprocessor.md):  The C Pre-Processor
 + [Part 36](36_Break_Continue.md): `break` and `continue`
 + [Part 37](37_Switch.md):        Switch Statements
 + [Part 38](38_Dangling_Else.md): Dangling Else and More
 + [Part 39](39_Var_Initialisation_pt1.md): Variable Initialisation, part 1
 + [Part 40](40_Var_Initialisation_pt2.md): Global Variable Initialisation
 + [Part 41](41_Local_Var_Init.md): Local Variable Initialisation
 + [Part 42](42_Casting.md):       Type Casting and NULL
 + [Part 43](43_More_Operators.md): Bugfixes and More Operators
 + [Part 44](44_Fold_Optimisation.md): Constant Folding
 + [Part 45](45_Globals_Again.md): Global Variable Declarations, revisited
 + [Part 46](46_Void_Functions.md): Void Function Parameters and Scanning Changes
 + [Part 47](47_Sizeof.md):        A Subset of `sizeof`
 + [Part 48](48_Static.md):        A Subset of `static`
 + [Part 49](49_Ternary.md):       The Ternary Operator
 + [Part 50](50_Mop_up_pt1.md):    Mopping Up, part 1
 + [Part 51](51_Arrays_pt2.md):    Arrays, part 2
 + [Part 52](52_Pointers_pt2.md):  Pointers, part 2
 + [Part 53](53_Mop_up_pt2.md):    Mopping Up, part 2
 + [Part 54](54_Reg_Spills.md):    Spilling Registers
 + [Part 55](55_Lazy_Evaluation.md): Lazy Evaluation
 + [Part 56](56_Local_Arrays.md):  Local Arrays
 + [Part 57](57_Mop_up_pt3.md):    Mopping Up, part 3
 + [Part 58](58-Ptr_Increments.md): Fixing Pointer Increments/Decrements
 + [Part 59](59-WDIW_pt1.md):      Why Doesn't It Work, part 1
 + [Part 60](60-TripleTest.md):    Passing the Triple Test
 + [Part 61](61-What_Next.md):     What's Next?
 + [Part 62](62-Cleanup.md):       Code Cleanup
 + [Part 63](63-QBE.md):           A New Backend using QBE

There isn't a schedule or timeline for the future parts, so
just keep checking back here to see if I've written any more.

## Copyrights

I have borrowed some of the code, and lots of ideas, from the 
[SubC](http://www.t3x.org/subc/) compiler written by Nils M Holm.
His code is in the public domain. I think that my code is substantially
different enough that I can apply a different license to my code.

Unless otherwise noted,

 + all source code and scripts are (c) Warren Toomey under
   the GPL3 license.
 + all non-source code documents (e.g. English documents,
   image files) are (c) Warren Toomey under the Creative
   Commons BY-NC-SA 4.0 license.
