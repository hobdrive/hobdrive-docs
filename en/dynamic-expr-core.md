# Dynamic Expressions: Core Syntax

The expression evaluator is based on an Excel-like implementation. It can parse mathematical expressions with support for the most commonly used functions.

Examples:

```
4*(24/2-5)+14
cos(Pi/4)*sin(Pi/6)^2
1-1/E^(0.5^2)
min(5;2;9;10;42;35)
```

## Data Types

Five data types are supported:

| Type | Description |
|------|-------------|
| `double` | Floating-point number (default for all calculations) |
| `int` | Integer. Use the `int()` function to convert explicitly |
| `hexadecimal` | Hexadecimal number. Converted to `double` in calculations |
| `string` | Text string |
| `boolean` | Logical value (`true` / `false`) |

> **Note:** Integers and hexadecimals are always converted to `double` when used in calculations. Use `int()` to convert to integer explicitly.

## Built-in Constants

| Constant | Description |
|----------|-------------|
| `Pi` | The number π ≈ 3.14159… |
| `E` | Euler's number e ≈ 2.71828… |

## Function Reference

### Math Functions

| Function | Description | Example |
|----------|-------------|---------|
| `Abs(x)` | Absolute value | `Abs(-5)` → `5` |
| `Acos(x)` | Arc cosine (inverse cosine) | `Acos(0)` → `1.5708` |
| `Asin(x)` | Arc sine (inverse sine) | `Asin(1)` → `1.5708` |
| `Atan(x)` | Arc tangent (inverse tangent) | `Atan(1)` → `0.7854` |
| `Atan2(y;x)` | Arc tangent of y/x, using signs to determine quadrant | `Atan2(1;1)` → `0.7854` |
| `Ceiling(x)` | Rounds up to the nearest integer | `Ceiling(4.1)` → `5` |
| `Cos(x)` | Cosine | `Cos(0)` → `1` |
| `Cosh(x)` | Hyperbolic cosine | `Cosh(0)` → `1` |
| `Exp(x)` | e raised to the power x | `Exp(1)` → `2.7183` |
| `Fact(x)` | Factorial | `Fact(5)` → `120` |
| `Floor(x)` | Rounds down to the nearest integer | `Floor(4.9)` → `4` |
| `Ln(x)` | Natural logarithm (base e) | `Ln(E)` → `1` |
| `Log(x)` | Common logarithm (base 10) | `Log(100)` → `2` |
| `Pow(x;y)` | x raised to the power y | `Pow(2;10)` → `1024` |
| `Round(x)` | Rounds to the nearest integer | `Round(4.5)` → `5` |
| `Sign(x)` | Returns the sign: −1, 0, or 1 | `Sign(-42)` → `-1` |
| `Sin(x)` | Sine | `Sin(Pi/2)` → `1` |
| `Sinh(x)` | Hyperbolic sine | `Sinh(1)` → `1.1752` |
| `Sqr(x)` | Square (x²) | `Sqr(5)` → `25` |
| `Sqrt(x)` | Square root | `Sqrt(25)` → `5` |
| `Trunc(x)` | Truncates to integer (removes decimal part) | `Trunc(4.9)` → `4` |

### Aggregate / Statistics Functions

| Function | Description | Example |
|----------|-------------|---------|
| `Avg(…)` | Average of values | `Avg(2;4;6)` → `4` |
| `Max(…)` | Maximum value | `Max(5;2;9)` → `9` |
| `Min(…)` | Minimum value | `Min(5;2;9)` → `2` |
| `StDev(…)` | Standard deviation | `StDev(2;4;6)` |
| `Var(…)` | Variance | `Var(2;4;6)` |

### Logic Functions

| Function | Description | Example |
|----------|-------------|---------|
| `And(a;b)` | Logical AND | `And(true;false)` → `false` |
| `Or(a;b)` | Logical OR | `Or(true;false)` → `true` |
| `Not(x)` | Logical NOT | `Not(true)` → `false` |
| `If(cond;then;else)` | Conditional: returns `then` if `cond` is true, otherwise `else` | `If(5>6;"yes";"no")` → `"no"` |

### String Functions

| Function | Description | Example |
|----------|-------------|---------|
| `Format(x;fmt)` | Formats a value using a format string | `Format(Pi;"0.00")` → `"3.14"` |
| `Hex(x)` | Converts a number to hexadecimal string | `Hex(255)` → `"FF"` |
| `Left(s;n)` | Returns the first `n` characters | `Left("hello";3)` → `"hel"` |
| `Right(s;n)` | Returns the last `n` characters | `Right("hello";3)` → `"llo"` |
| `Mid(s;start;n)` | Returns `n` characters starting at `start` | `Mid("hello";2;3)` → `"ell"` |
| `Len(s)` | Returns the length of a string | `Len("hello")` → `5` |
| `Lower(s)` | Converts to lowercase | `Lower("HELLO")` → `"hello"` |
| `Upper(s)` | Converts to uppercase | `Upper("hello")` → `"HELLO"` |
| `Val(s)` | Converts a string to a number | `Val("42")` → `42` |

### Utility Functions

| Function | Description |
|----------|-------------|
| `About` | Displays version information |
| `Clear` | Clears all user-defined variables and functions |
| `Help` | Displays help information |

## String Operations

The `&` operator concatenates strings:

```
"Hello " & "world"          // → "Hello world"
"Pi = " & Pi                // → "Pi = 3.14159265358979"
Len("hello world")          // → 11
```

## Boolean Operators and Conditionals

```
true != false               // → true
5 > 6 ? "hello" : "world"  // → "world"
If(5 > 6;"hello";"world")  // → "world"
```

## Variable and Function Declaration

Use `:=` to declare variables and functions:

```
x := 42
f(x) := x^2
f(x) := sin(x) / cos(x)    // declare dynamic functions using built-in functions
```

## Recursion and Scope

Functions can call themselves recursively. Variables declared as function parameters are local to that function; other variables are resolved from the global scope.

```
fac(n) := (n = 0) ? 1 : fac(n-1)*n
// fac calls itself with different parameters

f(x) := x * Y
// x is in function scope, Y is in global scope
```

## Operator Precedence

From highest to lowest priority:

| Priority | Operator | Description |
|----------|----------|-------------|
| 1 | `( )`, `f(x)` | Grouping, function calls |
| 2 | `!` `~` `-` `+` | Unary operators |
| 3 | `^` | Exponentiation (Excel rule: `a^b^c` → `(a^b)^c`) |
| 4 | `*` `/` `%` | Multiplication, division, modulo |
| 5 | `+` `-` | Addition, subtraction |
| 6 | `&` | String concatenation |
| 7 | `<` `<=` `>` `>=` | Comparison operators |
| 8 | `=` `!=` `<>` | Equality and inequality |
| 9 | `&&` | Logical AND |
| 10 | `\|\|` | Logical OR |
| 11 | `?:` | Ternary conditional |
| 12 | `:=` | Assignment |
