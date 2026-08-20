# Chapter 4 — Decision Making with Python

## 4.1 Learning Objectives

After completing this chapter, you should be able to:

- explain why scientific programs need decision-making statements;
- use comparison operators in Python;
- understand Boolean expressions;
- use `if` statements;
- use `if ... else` statements;
- use `if ... elif ... else` statements;
- combine multiple conditions using logical operators;
- apply decision making to simple electronics and semiconductor examples;
- distinguish between a numerical calculation and a decision based on that calculation.

---

4.2 Why Do Programs Need Decisions?

So far, our programs have followed a fixed sequence:

```text
Input
  ↓
Calculation
  ↓
Output
```

Scientific and engineering programs often need to do more.

Suppose a temperature measurement is obtained from a semiconductor device.

We may want the program to answer:

> Is the temperature within the specified operating range?

Or suppose a voltage is measured.

We may want to determine:

> Is the voltage above the specified limit?

These are **decision-making problems**.

A decision-making statement allows a program to choose what to do based on whether a condition is true or false.

The general idea is:

```text
Condition
   ↓
Is it true?
  / \
Yes  No
 ↓    ↓
Action Action
```

Python uses the `if`, `elif`, and `else` statements for this purpose.

---

4.3 Conditions and Boolean Values

A condition is an expression that can be evaluated as either:

```text
True
```

or

```text
False
```

For example:

```python
5 > 3
```

Output:

```text
True
```

The statement asks:

> Is 5 greater than 3?

The answer is true.

Similarly:

```python
5 < 3
```

Output:

```text
False
```

The statement asks:

> Is 5 less than 3?

The answer is false.

This connection is important:

$$
\boxed{
\text{Condition}
\rightarrow
\text{True or False}
\rightarrow
\text{Decision}
}
$$

---

4.4 Comparison Operators

Python provides comparison operators for constructing conditions.

| Meaning | Python operator | Example |
| --- | --- | --- |
| Equal to | `==` | `x == 5` |
| Not equal to | `!=` | `x != 5` |
| Greater than | `>` | `x > 5` |
| Less than | `<` | `x < 5` |
| Greater than or equal to | `>=` | `x >= 5` |
| Less than or equal to | `<=` | `x <= 5` |

Notice the difference between:

```python
x = 5
```

and:

```python
x == 5
```

The first is assignment.

The second is comparison.

---

4.5 Testing Numerical Conditions

Consider a voltage:

```python
V = 5
```

We can ask different questions.

```python
print(V > 3)
```

Output:

```text
True
```

```python
print(V < 3)
```

Output:

```text
False
```

```python
print(V == 5)
```

Output:

```text
True
```

```python
print(V != 5)
```

Output:

```text
False
```

The result of each comparison is a Boolean value.

---

4.6 The `if` Statement

The simplest decision-making structure is `if`.

The general syntax is:

```python
if condition:
    statement
```

For example:

```python
temperature = 350

if temperature > 300:
    print("Temperature is above 300 K")
```

Output:

```text
Temperature is above 300 K
```

The indented statement is executed only when the condition is true.

---

4.7 Indentation Matters

Python uses indentation to define a block of code.

Correct:

```python
temperature = 350

if temperature > 300:
    print("Temperature is above 300 K")
```

Incorrect:

```python
temperature = 350

if temperature > 300:
print("Temperature is above 300 K")
```

The second version produces an indentation error.

The indentation is therefore part of Python's syntax.

A common convention is to use four spaces for indentation.

---

4.8 A Scientific Example: Temperature Limit

Suppose a device should operate below

$$
T_{\max}=350\,\mathrm{K}.
$$

A measured temperature is:

$$
T=360\,\mathrm{K}.
$$

We can write:

```python
T = 360

if T > 350:
    print("Temperature exceeds the specified limit")
```

Output:

```text
Temperature exceeds the specified limit
```

The program has converted a numerical measurement into a decision.

The structure is:

$$
T>T_{\max}
\quad\Rightarrow\quad
\text{warning}.
$$

---

4.9 The `if ... else` Statement

Sometimes we need to specify what should happen when the condition is false.

The general structure is:

```python
if condition:
    statement_if_true
else:
    statement_if_false
```

For example:

```python
T = 330

if T > 350:
    print("Temperature exceeds the limit")
else:
    print("Temperature is within the limit")
```

Output:

```text
Temperature is within the limit
```

The program chooses one of two paths.

```text
             T > 350?
              /   \
            Yes    No
             ↓      ↓
         Warning   Normal
```

---

4.10 Voltage Limit Example

Suppose a device has a maximum permitted voltage:

$$
V_{\max}=5\,\mathrm{V}.
$$

A measurement is:

$$
V=5.4\,\mathrm{V}.
$$

Python:

```python
V = 5.4
V_max = 5.0

if V > V_max:
    print("Voltage exceeds the limit")
else:
    print("Voltage is within the limit")
```

Output:

```text
Voltage exceeds the limit
```

The mathematical condition is:

$$
V>V_{\max}.
$$

Python implements the same condition:

```python
V > V_max
```

---

4.11 Equality and Tolerance

In scientific work, exact equality between floating-point values requires care.

For example:

```python
x = 0.1 + 0.2

print(x)
```

Output may be:

```text
0.30000000000000004
```

Therefore:

```python
x == 0.3
```

may not behave as a beginner expects.

For basic programming, the important lesson is:

> Floating-point numbers are stored using finite numerical precision.

When scientific calculations require comparison within a tolerance, we can use a condition based on the difference.

For example, if a value \(x\) should be approximately equal to \(x_0\), we can test:

$$
|x-x_0|<\epsilon,
$$

where \(\epsilon\) is a small tolerance.

Python:

```python
x = 0.1 + 0.2
x0 = 0.3
epsilon = 1e-10

if abs(x - x0) < epsilon:
    print("Values are approximately equal")
```

Output:

```text
Values are approximately equal
```

The details of numerical precision will become more important as we progress into scientific computing.

---

4.12 Multiple Conditions with `elif`

Sometimes there are more than two possible outcomes.

Python provides `elif`, which means "else if".

The general structure is:

```python
if condition_1:
    statement_1
elif condition_2:
    statement_2
else:
    statement_3
```

For example, classify temperature:

```python
T = 320

if T < 300:
    print("Low temperature")
elif T <= 350:
    print("Normal operating range")
else:
    print("High temperature")
```

Output:

```text
Normal operating range
```

The program checks the conditions from top to bottom.

Once one condition is true, the corresponding block is executed and the remaining conditions are skipped.

---

4.13 Semiconductor Operating Conditions

Suppose a device is classified according to temperature:

$$
T<300\,\mathrm{K}
\quad\Rightarrow\quad
\text{Low},
$$

$$
300\,\mathrm{K}\leq T\leq350\,\mathrm{K}
\quad\Rightarrow\quad
\text{Normal},
$$

and

$$
T>350\,\mathrm{K}
\quad\Rightarrow\quad
\text{High}.
$$

Python:

```python
T = 340

if T < 300:
    status = "Low"
elif T <= 350:
    status = "Normal"
else:
    status = "High"

print(f"Temperature status: {status}")
```

Output:

```text
Temperature status: Normal
```

The program transforms a numerical measurement into a meaningful category.

---

4.14 Logical Operators

Scientific conditions often involve more than one requirement.

Python provides three important logical operators:

- `and`
- `or`
- `not`

### `and`

Both conditions must be true.

```python
temperature = 320
voltage = 4.5

if temperature < 350 and voltage < 5:
    print("Operating conditions are within limits")
```

Output:

```text
Operating conditions are within limits
```

Mathematically, this corresponds to:

$$
T<350
\quad\text{and}\quad
V<5.
$$

---

### `or`

At least one condition must be true.

```python
temperature = 360
voltage = 4.5

if temperature > 350 or voltage > 5:
    print("At least one limit has been exceeded")
```

Output:

```text
At least one limit has been exceeded
```

---

### `not`

The `not` operator reverses a Boolean value.

```python
measurement_valid = True

print(not measurement_valid)
```

Output:

```text
False
```

---

4.15 Combining Conditions

Suppose a device is considered to be operating normally when:

$$
300\leq T\leq350\,\mathrm{K}
$$

and

$$
0<V\leq5\,\mathrm{V}.
$$

Python:

```python
T = 325
V = 4.5

if 300 <= T <= 350 and 0 < V <= 5:
    print("Device is operating within the specified range")
else:
    print("Device is outside the specified range")
```

Output:

```text
Device is operating within the specified range
```

Python allows chained comparisons such as:

```python
300 <= T <= 350
```

which can be read naturally as:

$$
300\leq T\leq350.
$$

---

4.16 Decision Making with Experimental Data

Suppose a laboratory measurement should satisfy a voltage range:

$$
4.8\,\mathrm{V}\leq V\leq5.2\,\mathrm{V}.
$$

A measurement is:

$$
V=5.1\,\mathrm{V}.
$$

Python:

```python
V = 5.1

if 4.8 <= V <= 5.2:
    print("Measurement is within the acceptable range")
else:
    print("Measurement is outside the acceptable range")
```

Output:

```text
Measurement is within the acceptable range
```

This is a simple example of **data validation**.

Later, when we work with real datasets, similar ideas will be used to identify values that may require attention.

---

4.17 Decision Making Does Not Change the Data

Suppose:

```python
V = 5.1
```

and the program determines that the voltage is within the acceptable range.

The `if` statement does not change `V`.

It simply determines which instruction should be executed.

This distinction is important:

> A calculation produces a value; a decision evaluates a condition involving that value.

For example:

$$
I=\frac{V}{R}
$$

is a calculation.

Whereas:

$$
I>I_{\max}
$$

is a condition.

---

4.18 Nested Decisions

A decision can contain another decision.

For example:

```python
T = 330
V = 4.5

if T <= 350:
    if V <= 5:
        print("Both conditions are within limits")
```

Output:

```text
Both conditions are within limits
```

Nested decisions can be useful, but they should not be overused.

When conditions become complicated, clear logical expressions are often easier to read.

For example:

```python
if T <= 350 and V <= 5:
    print("Both conditions are within limits")
```

is simpler in this case.

---

4.19 Common Beginner Mistakes

### Mistake 1 — Using `=` instead of `==`

Incorrect for comparison:

```python
if V = 5:
```

Correct:

```python
if V == 5:
```

Remember:

```text
=   assignment
==  comparison
```

### Mistake 2 — Forgetting the colon

Incorrect:

```python
if V > 5
    print("High voltage")
```

Correct:

```python
if V > 5:
    print("High voltage")
```

### Mistake 3 — Incorrect indentation

Python uses indentation to identify the statements belonging to a decision block.

### Mistake 4 — Reversing the condition

If the requirement is:

> Report a warning when temperature exceeds 350 K.

the condition should be:

```python
T > 350
```

not:

```python
T < 350
```

### Mistake 5 — Confusing `and` with `or`

For example:

```python
T < 350 and V < 5
```

requires both conditions to be true.

Whereas:

```python
T < 350 or V < 5
```

requires only one condition to be true.

---

4.20 Worked Example: Device Safety Check

Suppose a device has the following operating limits:

$$
T_{\max}=350\,\mathrm{K}
$$

and

$$
V_{\max}=5\,\mathrm{V}.
$$

The measured values are:

$$
T=340\,\mathrm{K},
\qquad
V=4.8\,\mathrm{V}.
$$

Write a program to determine whether both values are within their limits.

### Python

```python
T = 340
V = 4.8

T_max = 350
V_max = 5

if T <= T_max and V <= V_max:
    print("Device is within operating limits")
else:
    print("Device is outside operating limits")
```

Output:

```text
Device is within operating limits
```

### Scientific interpretation

The program checks the conditions

$$
T\leq350\,\mathrm{K}
$$

and

$$
V\leq5\,\mathrm{V}.
$$

Both conditions are satisfied, so the program reports that the measured operating point is within the specified limits.

---

4.21 Worked Example: Classifying a Measurement

Suppose a measured voltage is classified as:

- below \(0.5\,\mathrm{V}\): Low;
- from \(0.5\,\mathrm{V}\) to \(1.0\,\mathrm{V}\): Normal;
- above \(1.0\,\mathrm{V}\): High.

For

$$
V=0.75\,\mathrm{V},
$$

use:

```python
V = 0.75

if V < 0.5:
    status = "Low"
elif V <= 1.0:
    status = "Normal"
else:
    status = "High"

print(f"Voltage status: {status}")
```

Output:

```text
Voltage status: Normal
```

The example demonstrates how a continuous numerical quantity can be converted into a categorical description.

---

4.22 Worked Example: Measurement Validation

Suppose a laboratory specification requires current to satisfy:

$$
1\,\mathrm{mA}\leq I\leq10\,\mathrm{mA}.
$$

A measured current is:

$$
I=7.5\,\mathrm{mA}.
$$

Python:

```python
I_mA = 7.5

if 1 <= I_mA <= 10:
    print("Measurement accepted")
else:
    print("Measurement requires review")
```

Output:

```text
Measurement accepted
```

Here, the program performs a simple validation step.

In later chapters, data validation will become more systematic when we work with collections of measurements.

---

4.23 Exercises

### Exercise 1 — Comparison Operators

Let:

```python
V = 5
```

Use Python to determine the result of:

```python
V > 3
V < 3
V == 5
V != 5
V >= 5
V <= 5
```

---

### Exercise 2 — Temperature Warning

A device should not operate above:

$$
T_{\max}=350\,\mathrm{K}.
$$

Write a program that prints:

```text
Warning
```

when the measured temperature exceeds the limit.

---

### Exercise 3 — Voltage Classification

Write a program to classify voltage as:

- Low if \(V<2\,\mathrm{V}\);
- Normal if \(2\leq V\leq5\,\mathrm{V}\);
- High if \(V>5\,\mathrm{V}\).

Test the program with at least three different values.

---

### Exercise 4 — Operating Range

A device operates normally when:

$$
300\leq T\leq350\,\mathrm{K}.
$$

Write a program to determine whether a measured temperature is within this range.

---

### Exercise 5 — Two Operating Limits

A device is within its specified operating range when:

$$
T\leq350\,\mathrm{K}
$$

and

$$
V\leq5\,\mathrm{V}.
$$

Write a Python program using `and`.

---

### Exercise 6 — Measurement Validation

A current measurement is acceptable when:

$$
0.5\,\mathrm{mA}\leq I\leq5\,\mathrm{mA}.
$$

Write a program that accepts a current value from the user and reports whether the measurement is acceptable.

---

4.24 Think and Apply

A student writes:

```python
T = 360

if T > 350:
    print("Safe")
else:
    print("Unsafe")
```

The program runs correctly.

1. Is the Python syntax correct?
2. Does the output agree with the intended meaning of the messages?
3. What is wrong with the scientific logic?
4. How would you correct the program?

---

4.25 Chapter Summary

In this chapter, we learned that:

1. Programs often need to make decisions based on data.
2. A condition produces a Boolean result: `True` or `False`.
3. Python provides comparison operators such as `>`, `<`, `==`, `>=`, and `<=`.
4. The `if` statement executes code when a condition is true.
5. `if ... else` provides two possible paths.
6. `if ... elif ... else` allows multiple possible outcomes.
7. `and`, `or`, and `not` combine or modify logical conditions.
8. Indentation is part of Python syntax.
9. Decision making can be applied to operating limits, measurement validation, and scientific classification.
10. A decision evaluates data; it does not automatically change the underlying measurement.
11. Scientific correctness requires the condition to reflect the physical or experimental requirement.

---

4.26 Key Takeaway

> **Decision making allows a Python program to respond to scientific conditions. A measurement becomes useful not only when we calculate it, but also when we can evaluate it against meaningful scientific criteria.**

The next chapter introduces **loops and functions**, allowing us to repeat calculations efficiently and organize frequently used scientific procedures into reusable blocks of code.
