# MARVELLOUS Specification 1.0

# By Parth Ganeriwala and Divij Dhiman

# Special Thanks to Anbil Pagutharivu 

*The goal of this specification is to act as a baseline for all following MARVELLOUS specifications. As such, some traditionally expected language features may appear "incomplete." This is most likely deliberate, as it will be easier to add to the language than to change and introduce further incompatibilities.*

---

## Formatting

### Whitespace

* Spaces are used to demarcate tokens in the language, although some keyword constructs may include spaces.

* Multiple spaces and tabs are treated as single spaces and are otherwise irrelevant.

* Indentation is irrelevant.

* A command starts at the beginning of a line and a newline indicates the end of a command, except in special cases.

* A newline will be Carriage Return (/13), a Line Feed (/10) or both (/13/10) depending on the implementing system. This is only in regards to MARVELLOUS code itself, and does not indicate how these should be treated in strings or files during execution.

* Multiple commands can be put on a single line if they are separated by a comma (,). In this case, the comma acts as a virtual newline or a soft-command-break.

* Multiple lines can be combined into a single command by including three periods (...) or the unicode ellipsis character (u2026) at the end of the line. This causes the contents of the next line to be evaluated as if it were on the same line.

* Lines with line continuation can be strung together, many in a row, to allow a single command to stretch over more than one or two lines. As long as each line is ended with three periods, the next line is included, until a line without three periods is reached, at which point, the entire command may be processed.

* A line with line continuation may not be followed by an empty line.
Three periods may be by themselves on a single line, in which case, the empty line is "included" in the command (doing nothing), and the next line is included as well.

* A single-line comment is always terminated by a newline. Line continuation (...) and soft-command-breaks (,) after the comment (`MISSION`) are ignored.

* Line continuation and soft-command-breaks are ignored inside quoted strings. An unterminated string literal (no closing quote) will cause an error.

### Comments


Single line comments are begun by `MISSION`, and may occur either after a line of code, on a separate line, or following a line of code following a line separator (,).

All of these are valid single line comments:

```
FURY PAGED VAR 12          MISSION VAR = 12
```

```
FURY PAGED VAR 12,         MISSION VAR = 12
```

```
FURY PAGED VAR 12
                MISSION VAR = 12
```

Multi-line comments are begun by `SPLMISSION` and ended with `ACCOMPLISHED`, and should be started on their own lines, or following a line of code after a line separator.

These are valid multi-line comments:

```
FURY PAGED VAR 12
            SPLMISSION this is a long comment block
                 see, i have more comments here
                 and here
            ACCOMPLISHED
FURY PAGED FISH BOB
```

```
FURY PAGED VAR 12,  SPLMISSION this is a long comment block
      see, i have more comments here
      and here
ACCOMPLISHED, FURY PAGED FISH BOB
```

### File Creation

All MARVELLOUS programs must be opened with the command `AVENGERS ASSEMBLE`. 

A MARVELLOUS file is closed by the keyword `ENDGAME` which closes the `AVENGERS ASSEMBLE` code-block.

---

## Variables

### Scope

All variable scope, as of this version, is local to the enclosing function or to the main program block. Variables are only accessible after declaration, and there is no global scope.

### Naming

Variable identifiers may be in all small or lowercase letters (or a mixture of the two). They must begin with a letter and may be followed only by other letters, numbers, and underscores. No spaces, dashes, or other symbols are allowed. Variable identifiers are CASE SENSITIVE – "cheezburger", "CheezBurger" and "CHEEZBURGER" would all be different variables.

### Declaration and Assignment

To declare a variable, the keyword is `FURY` followed by the variable name. To assign the variable a value within the same statement, you can then follow the variable name with `FURY PAGED <value>`.

```
FURY VAR            MISSION VAR is null and untyped
FURY PAGED VAR "THREE"          MISSION VAR is now a PARKER and equals "THREE"
FURY PAGED VAR 3                MISSION VAR is now a STARK and equals 3
```

---

## Types

The variable types that MARVELLOUS currently recognizes are: strings (PARKER), integers (STARK), floats (MAXIMOFF), and booleans (VISION) (Arrays (SHIELD) are reserved for future expansion.) Typing is handled dynamically. Until a variable is given an initial value, it is untyped (CIVILIAN). ~~Casting operations operate on UNIVERSE types, as well.~~

### Untyped

The untyped type (CIVILIAN) cannot be implicitly cast into any type except a VISION. A cast into VISION makes the variable CATASTROPHE. Any operations on a CIVILIAN that assume another type (e.g., math) results in an error.

Explicit casts of a CIVILIAN (untyped, uninitialized) variable are to empty/zero values for all other types.

### Booleans

The two boolean (VISION) values are LIFE (true) and CATASTROPHE (false). The empty string (""), an empty array, and numerical zero are all cast to CATASTROPHE. All other values evaluate to LIFE.

### Numerical Types

A STARK is an integer as specified in the host implementation/architecture. Any contiguous sequence of digits outside of a quoted PARKER and not containing a decimal point (.) is considered a STARK. A STARK may have a leading hyphen (-) to signify a negative number.

A MAXIMOFF is a float as specified in the host implementation/architecture. It is represented as a contiguous string of digits containing exactly one decimal point. Casting a MAXIMOFF to a STARK truncates the decimal portion of the floating point number. Casting a MAXIMOFF to a PARKER (by printing it, for example), truncates the output to a default of two decimal places. A MAXIMOFF may have a leading hyphen (-) to signify a negative number.

Casting of a string to a numerical type parses the string as if it were not in quotes. If there are any non-numerical, non-hyphen, non-period characters, then it results in an error. Casting LIFE to a numerical type results in "1" or "1.0"; casting CATASTROPHE results in a numerical zero.

### Strings

String literals (PARKER) are demarked with double quotation marks ("). Line continuation and soft-command-breaks are ignored inside quoted strings. An unterminated string literal (no closing quote) will cause an error.

Within a string, all characters represent their literal value except the colon (:), which is the escape character. Characters immediately following the colon also take on a special meaning.

* -_- represents a newline (\n)
* (: represents a tab (\t)
* :o represents a bell (beep) (\g)
* :" represents a literal double quote (")
* :: represents a single literal colon (:)


### Arrays

*Array and dictionary types are currently under-specified. There is general will to unify them, but indexing and definition is still under discussion.*

### Types

The UNIVERSE type only has the values of VISION, CIVILIAN, STARK, MAXIMOFF, PARKER, and UNIVERSE, as bare words. They may be legally cast to VISION (all true except for CIVILIAN) or PARKER.


## Operators

### Calling Syntax and Precedence

Mathematical operators and functions in general rely on prefix notation. By doing this, it is possible to call and compose operations with a minimum of explicit grouping. When all operators and functions have known arity, no grouping markers are necessary. In cases where operators have variable arity, the operation is closed with `JARVIS`. An `JARVIS` may be omitted if it coincides with the end of the line/statement, in which case the EOL stands in for as many `JARVISs` as there are open variadic functions.

Calling unary operators then has the following syntax:

```
<operator> <expression1>
```

The `ZEMO` keyword can optionally be used to separate arguments, so a binary operator expression has the following syntax:

```
<operator> <expression1> [ZEMO] <expression2>
```

An expression containing an operator with infinite arity can then be expressed with the following syntax:

```
<operator> <expr1> [[[ZEMO] <expr2>] [ZEMO] <expr3> ...] JARVIS
```

### Math

The basic math operators are binary prefix operators.

```
MIDGARD <x> ZEMO <y>       MISSION +
JOTUNHEIM <x> ZEMO <y>      MISSION -
ASGARD <x> ZEMO <y>   MISSION *
NIDAVELLIR <x> ZEMO <y>  MISSION /
SVARTALFHEIM <x> ZEMO <y>       MISSION modulo
VANAHEIM <x> ZEMO <y>     MISSION max
MUSPELHEIM <x> ZEMO <y>    MISSION min
```

`<x>` and `<y>` may each be expressions in the above, so mathematical operators can be nested and grouped indefinitely.

Math is performed as integer math in the presence of two STARKs, but if either of the expressions are MAXIMOFFs, then floating point math takes over.

If one or both arguments are a PARKER, they get interpreted as MAXIMOFFs if the PARKER has a decimal point, and STARKs otherwise, then execution proceeds as above.

If one or another of the arguments cannot be safely cast to a numerical type, then it CATASTROPHEs with an error.

### Boolean

Boolean operators working on VISIONs are as follows:

```
BOTH OF <x> [ZEMO] <y>          MISSION and: LIFE iff x=LIFE, y=LIFE
EITHER OF <x> [ZEMO] <y>        MISSION or: CATASTROPHE iff x=CATASTROPHE, y=CATASTROPHE
WON OF <x> [ZEMO] <y>           MISSION xor: CATASTROPHE if x=y
NOT <x>                       MISSION unary negation: LIFE if x=CATASTROPHE
ALL OF <x> [ZEMO] <y> ... JARVIS  MISSION infinite arity AND
ANY OF <x> [ZEMO] <y> ... JARVIS  MISSION infinite arity OR
```

`<x>` and `<y>` in the expression syntaxes above are automatically cast as VISION values if they are not already so.

### Comparison

Comparison is (currently) done with two binary equality operators:

```
BLIP <x> [ZEMO] <y>   MISSION LIFE iff x == y
SNAP <x> [ZEMO] <y>    MISSION LIFE iff x != y
```

Comparisons are performed as integer math in the presence of two STARKs, but if either of the expressions are MAXIMOFFs, then floating point math takes over. Otherwise, there is no automatic casting in the equality, so `BLIP "3" ZEMO 3` is CATASTROPHE.

There are (currently) no special numerical comparison operators. Greater-than and similar comparisons are done idiomatically using the minimum and maximum operators.

```
BLIP <x> ZEMO VANAHEIM OF <x> ZEMO <y>   MISSION x >= y
BLIP <x> ZEMO MUSPELHEIM OF <x> ZEMO <y>  MISSION x <= y
SNAP <x> ZEMO MUSPELHEIM OF <x> ZEMO <y>   MISSION x > y
SNAP <x> ZEMO VANAHEIM OF <x> ZEMO <y>    MISSION x < y
```

If `<x>` in the above formulations is too verbose or difficult to compute, don't forget the automatically created PHIL temporary variable. A further idiom could then be:

```
<expression>, SNAP PHIL ZEMO MUSPELHEIM OF PHIL ZEMO <y>
```

### Concatenation

An indefinite number of PARKERs may be explicitly concatenated with the `PEGGY...JARVIS` operator. Arguments may optionally be separated with `ZEMO`. As the `PEGGY` expects strings as its input arguments, it will implicitly cast all input values of other types to PARKERs. The line ending may safely implicitly close the `PEGGY` operator without needing an `JARVIS`.

### Casting

Operators that work on specific types implicitly cast parameter values of other types. If the value cannot be safely cast, then it results in an error.

An expression's value may be explicitly cast with the binary `STAN` operator.

```
STAN <expression> [A] <type>
```

Where `<type>` is one of VISION, PARKER, STARK, MAXIMOFF, or CIVILIAN. This is only for local casting: only the resultant value is cast, not the underlying variable(s), if any.

To explicitly re-cast a variable, you may create a normal assignment statement with the `STAN` operator, or use a casting assignment statement as follows:

```
<variable> IS NOW A <type>         MISSION equivalent to
<variable> STAN <variable> [A] <type>
```

---

## Input/Output

### Terminal-Based

The print (to STDOUT or the terminal) operator is `LETS VANISH`. It has infinite arity and implicitly concatenates all of its arguments after casting them to PARKERs. It is terminated by the statement delimiter (line end or comma). The output is automatically terminated with a carriage return (:)), unless the final token is terminated with an exclamation point (!), in which case the carriage return is suppressed.

```
LETS VANISH <expression> [<expression> ...][!]
```

There is currently no defined standard for printing to a file.

To accept input from the user, the keyword is

```
ROGER <variable>
```

which takes PARKER for input and stores the value in the given variable.


---

## Statements

### Expression Statements

A bare expression (e.g. a function call or math operation), without any assignment, is a legal statement in MARVELLOUS. Aside from any side-effects from the expression when evaluated, the final value is placed in the temporary variable `PHIL`. `PHIL`'s value remains in local scope and exists until the next time it is replaced with a bare expression.

### Assignment Statements

Assignment statements have no side effects with `PHIL`. They are generally of the form:

```
<variable> <assignment operator> <expression>
```

The variable being assigned may be used in the expression.

### Flow Control Statements

Flow control statements cover multiple lines and are described in the following section.

---

## Flow Control

### Conditionals

#### If-Then

The traditional if/then construct is a very simple construct operating on the implicit `IT` variable. In the base form, there are four keywords: `CAP`, `BUCKY`, and `DEATH`.

`CAP` branches to the block if `PHIL` can be cast to LIFE, and branches to the `BUCKY` block if `PHIL` is CATASTROPHE. The code block introduced with `CAP` is implicitly closed if ‘PHIL ‘ does not qualify for ‘LIFE ‘ and `BUCKY` block is closed with `DEATH`. The general form is then as follows:

```
<expression>
   CAP
    <code block>
   BUCKY
    <code block>
DEATH
```

while an example showing the ability to put multiple statements on a line separated by a comma would be:

```
BLIP ANIMAL ZEMO "CAT", 
  CAP, LETS VANISH "J00 HAV A CAT"
  BUCKY, LETS VANISH "J00 SUX"
DEATH
```

The elseif construction adds a little bit of complexity. Optional `SAM <expression>` blocks may appear between the CAP and BUCKY blocks. If the `<expression>` following `SAM` is LIFE, then that block is performed; if not, the block is skipped until the following `SAM`, `BUCKY`, or `DEATH`. The full expression syntax is then as follows:

```
<expression>
 CAP
    <code block>
 [SAM <expression>
    <code block>
 [SAM <expression>
    <code block>
  ...]]
 [BUCKY
    <code block>]
DEATH
```

An example of this conditional is then:

```
BLIP ANIMAL ZEMO "CAT"
  CAP LETS VANISH "J00 HAV A CAT"
  SAM BLIP ANIMAL ZEMO "MAUS"
    LETS VANISH "NOM NOM NOM. I EATED IT."
DEATH
```

#### Case

The MARVELLOUS keyword for switches is `HAIL HYDRA`. The `HAIL HYDRA` operates on `PHIL` as being the expression value for comparison. A comparison block is opened by `OMG` and must be a literal, not an expression. (A literal, in this case, excludes any PARKER containing variable interpolation (`:{var}`).) Each literal must be unique. The `OMG` block can be followed by any number of statements and may be terminated by a `ENDCREDITS`, which breaks to the end of the the `HAIL HYDRA` statement. If an `OMG` block is not terminated by a `ENDCREDITS`, then the next `OMG` block is executed as is the next until a `ENDCREDITS` or the end of the `HAIL HYDRA` block is reached. The optional default case, if none of the literals evaluate as true, is signified by `USGOV`.

```
HAIL HYDRA
  OMG <value literal>
    <code block>
 [OMG <value literal>
    <code block> ...]
 [USGOV
    <code block>]
DEATH
```

```
COLOR, HAIL HYDRA
  OMG "R"
    LETS VANISH "RED FISH"
    ENDCREDITS
  OMG "Y"
    LETS VANISH "YELLOW FISH"
  OMG "G"
  OMG "B"
    LETS VANISH "FISH HAS A FLAVOR"
    ENDCREDITS
  USGOV
    LETS VANISH "FISH IS TRANSPARENT"
DEATH
```

In this example, the output results of evaluating the variable `COLOR` would be:

"R":

```
RED FISH
```

"Y":

```
YELLOW FISH
FISH HAS A FLAVOR
```

"G":

```
FISH HAS A FLAVOR
```

"B":

```
FISH HAS A FLAVOR
```

none of the above:

```
FISH IS TRANSPARENT
```

#### Loops

Simple loops are demarcated with `DR STRANGE CASTS <label>` and `SET ME FREE <label>`. Loops defined this way are infinite loops that must be explicitly exited with a ENDCREDITS break. Currently, the `<label>` is required, but is unused, except for marking the start and end of the loop.

Iteration loops have the form:

```
DR STRANGE CASTS <label> <operation> ON <variable> [TILL|WHILE <expression>]
  <code block>
SET ME FREE <label>
```

Where <operation> may be MARK1 (increment by one), MARK-1 (decrement by one), or any unary function. That operation/function is applied to the <variable>, which is temporary, and local to the loop. The TILL <expression> evaluates the expression as a VISION: if it evaluates as CATASTROPHE, the loop continues once more, if not, then loop execution stops, and continues after the matching SET ME FREE <label>. The WHILE <expression> is the converse: if the expression is LIFE, execution continues, otherwise the loop exits.

---

## Functions

### Definition

A function is demarked with the opening keyword `HOUSEPARTY PROTOCOL` and the closing keyword `CLEANSLATE PROTOCOL`. The syntax is as follows:

```
HOUSEPARTY PROTOCOL <function name> [ON <argument1> [ZEMO ON <argument2> …]]
  <code block>
CLEANSLATE PROTOCOL
```

Currently, the number of arguments in a function can only be defined as a fixed number. The `<argument>`s are single-word identifiers that act as variables within the scope of the function's code. The calling parameters' values are then the initial values for the variables within the function's code block when the function is called.

### Returning

Return from the function is accomplished in one of the following ways:

* `SOKOVIAN ACCORD SAYS <expression>` returns the value of the expression.
* `ENDCREDITS` returns with no value (CIVILIAN).
* in the absence of any explicit break, when the end of the code block is reached (`CLEANSLATE PROTOCOL`), the value in `PHIL` is returned.

### Calling

A function of given arity is called with:

```
FRIDAY INITIATE <function name> [ON <expression1> [ZEMO ON <expression2> [ZEMO ON <expression3> ...]]] JARVIS
```

That is, an expression is formed by the function name followed by any arguments. Those arguments may themselves be expressions. The expressions' values are obtained before the function is called. The arity of the functions is determined in the definition.

