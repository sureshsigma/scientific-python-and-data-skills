# Chapter 5 — Loops and Functions

## 5.1 Learning Objectives

After completing this chapter, you should be able to:

- explain why repetition is important in scientific computing;
- use `for` loops to repeat a calculation;
- use `range()` to generate a sequence of values;
- understand the role of indentation in loops;
- use `while` loops for condition-based repetition;
- distinguish between `for` and `while` loops;
- create simple Python functions;
- pass values to functions using parameters;
- return results using `return`;
- apply loops and functions to simple electronics and semiconductor calculations;
- recognize how reusable functions improve scientific programs.

---

5.2 Why Do We Need Repetition?

Consider an experiment in which the voltage across a resistor is measured at several values:

$$
V=1,2,3,4,5\,\mathrm{V}.
$$

For each voltage, we may want to calculate current using Ohm's law:

$$
I=\frac{V}{R}.
$$

One approach is to write the calculation repeatedly:

```python
R = 100

I1 = 1 / R
I2 = 2 / R
I3 = 3 / R
I4 = 4 / R
I5 = 5 / R
```

This works, but it is repetitive.

If an experiment contains 1,000 measurements, writing 1,000 separate calculations would be impractical.

Programming provides a better solution:

> **Repeat the same instruction automatically.**

Python uses **loops** for repetition.

---

5.3 The `for` Loop

A `for` loop repeats a block of code for each item in a sequence.

A simple example is:

```python
for i in range(5):
    print(i)
```

Output:

```text
0
1
2
3
4
```

The loop executes five times.

The variable `i` takes the values:

$$
0,1,2,3,4.
$$

Notice that `range(5)` stops before 5.

---

5.4 Understanding `range()`

The `range()` function is commonly used with `for` loops.

For example:

```python
range(5)
```

produces the sequence:

$$
0,1,2,3,4.
$$

We can specify a starting value:

```python
for i in range(1, 6):
    print(i)
```

Output:

```text
1
2
3
4
5
```

The general form is:

```python
range(start, stop)
```

The `stop` value is not included.

We can also specify a step:

```python
for i in range(0, 11, 2):
    print(i)
```

Output:

```text
0
2
4
6
8
10
```

Here:

- start = 0;
- stop = 11;
- step = 2.

---

5.5 A First Scientific Loop

Suppose a resistor has:

$$
R=100\,\Omega.
$$

We want to calculate current for voltages from \(1\) V to \(5\) V.

Using:

$$
I=\frac{V}{R},
$$

we can write:

```python
R = 100

for V in range(1, 6):
    I = V / R
    print(V, I)
```

Output:

```text
1 0.01
2 0.02
3 0.03
4 0.04
5 0.05
```

The loop has replaced five separate calculations.

---

5.6 Making the Output Readable

The same calculation can be formatted more clearly:

```python
R = 100

for V in range(1, 6):
    I = V / R
    print(f"Voltage = {V} V, Current = {I:.3f} A")
```

Output:

```text
Voltage = 1 V, Current = 0.010 A
Voltage = 2 V, Current = 0.020 A
Voltage = 3 V, Current = 0.030 A
Voltage = 4 V, Current = 0.040 A
Voltage = 5 V, Current = 0.050 A
```

The program now produces a simple table of calculated values.

---

5.7 Repetition in Scientific Experiments

Many scientific calculations have the structure:

$$
x_1\rightarrow f(x_1),
$$

$$
x_2\rightarrow f(x_2),
$$

$$
x_3\rightarrow f(x_3),
$$

and so on.

A loop allows us to write the function once and apply it repeatedly.

For example:

```python
for V in range(1, 6):
    I = V / 100
    print(I)
```

The general idea is:

$$
\boxed{
\text{Input value}
\rightarrow
\text{Scientific calculation}
\rightarrow
\text{Result}
}
$$

repeated for many input values.

---

5.8 Looping Over Floating-Point Values

`range()` works with integers.

It cannot directly generate:

```python
0.1, 0.2, 0.3, 0.4, 0.5
```

using:

```python
range()
```

For example, this is not valid:

```python
range(0.1, 0.6, 0.1)
```

At this stage, we can work around this by using integer values and converting them.

For example:

```python
for i in range(1, 6):
    V = i * 0.1
    print(V)
```

Output:

```text
0.1
0.2
0.30000000000000004
0.4
0.5
```

The value `0.30000000000000004` illustrates floating-point representation.

Later, NumPy will provide more convenient tools for generating numerical sequences.

---

5.9 Looping Through a Sequence of Values

A `for` loop can also iterate directly through values.

For example:

```python
voltages = [1, 2, 3, 4, 5]

for V in voltages:
    print(V)
```

Output:

```text
1
2
3
4
5
```

The list used here is introduced only to demonstrate the loop concept. We will study lists systematically in a later chapter.

---

5.10 Looping Through Measurement Values

Suppose measured voltages are:

```python
voltages = [0.5, 0.7, 0.9, 1.1]
```

We can calculate current for a resistor:

```python
R = 100

for V in voltages:
    I = V / R
    print(f"V = {V:.1f} V, I = {I:.4f} A")
```

Output:

```text
V = 0.5 V, I = 0.0050 A
V = 0.7 V, I = 0.0070 A
V = 0.9 V, I = 0.0090 A
V = 1.1 V, I = 0.0110 A
```

This is a simple example of applying the same mathematical model to multiple measurements.

---

5.11 Nested Loops

A loop can contain another loop.

For example:

```python
for voltage in range(1, 4):
    for resistance in [100, 200]:
        current = voltage / resistance
        print(voltage, resistance, current)
```

Output:

```text
1 100 0.01
1 200 0.005
2 100 0.02
2 200 0.01
3 100 0.03
3 200 0.015
```

Nested loops can be useful when studying multiple combinations of experimental conditions.

However, they should be used carefully because deeply nested loops can make programs difficult to read.

---

5.12 The `while` Loop

A `while` loop repeats code as long as a condition remains true.

The general form is:

```python
while condition:
    statement
```

For example:

```python
i = 1

while i <= 5:
    print(i)
    i = i + 1
```

Output:

```text
1
2
3
4
5
```

The loop continues while:

$$
i\leq5.
$$

Each iteration increases `i` by 1.

---

5.13 `for` versus `while`

A `for` loop is often convenient when the number of repetitions or sequence of values is known.

A `while` loop is useful when repetition depends on a condition.

| Situation | Suitable loop |
| --- | --- |
| Repeat for a known sequence | `for` |
| Repeat a known number of times | `for` |
| Continue until a condition changes | `while` |
| Process values in a sequence | `for` |
| Condition-controlled repetition | `while` |

For example:

```python
for i in range(10):
    print(i)
```

is naturally expressed using a `for` loop.

A process such as:

> Continue measuring while temperature is below the limit

is conceptually suited to a `while` loop.

---

5.14 A `while` Loop in an Engineering Context

Suppose a simple model increases voltage until it reaches a maximum value.

```python
V = 0

while V <= 5:
    print(f"Voltage = {V} V")
    V = V + 1
```

Output:

```text
Voltage = 0 V
Voltage = 1 V
Voltage = 2 V
Voltage = 3 V
Voltage = 4 V
Voltage = 5 V
```

The loop stops when:

$$
V>5.
$$

This demonstrates condition-controlled repetition.

---

5.15 Avoiding Infinite Loops

A `while` loop must eventually make its condition false.

Consider:

```python
i = 1

while i <= 5:
    print(i)
```

This loop never changes `i`.

Therefore:

$$
i=1
$$

remains true forever.

The program can continue indefinitely.

The correct version is:

```python
i = 1

while i <= 5:
    print(i)
    i = i + 1
```

The value of `i` changes on every iteration, so eventually the condition becomes false.

> **Always check how a `while` loop will terminate.**

---

5.16 Why Do We Need Functions?

Loops reduce repetition in calculations.

Functions reduce repetition in **program structure**.

Suppose we repeatedly calculate current using:

$$
I=\frac{V}{R}.
$$

We could write the expression every time:

```python
I1 = V1 / R1
I2 = V2 / R2
I3 = V3 / R3
```

Instead, we can define a function once.

```python
def calculate_current(V, R):
    return V / R
```

Then use it whenever needed:

```python
I = calculate_current(5, 100)

print(I)
```

Output:

```text
0.05
```

The function contains the reusable calculation.

---

5.17 Anatomy of a Function

Consider:

```python
def calculate_current(V, R):
    return V / R
```

There are several components.

### `def`

The keyword `def` tells Python that we are defining a function.

### Function name

```python
calculate_current
```

The name describes what the function does.

### Parameters

```python
V, R
```

These are the values supplied to the function.

### Function body

```python
return V / R
```

This contains the calculation.

### Return value

The `return` statement sends the calculated result back to the part of the program that called the function.

---

5.18 Calling a Function

Once a function has been defined, it can be called.

```python
def calculate_current(V, R):
    return V / R

current = calculate_current(5, 100)

print(current)
```

Output:

```text
0.05
```

The values

$$
V=5
$$

and

$$
R=100
$$

are passed to the function.

The function calculates:

$$
I=\frac{5}{100}=0.05.
$$

---

5.19 Why Functions Are Useful

Functions provide several benefits.

### Reusability

Write a calculation once and use it many times.

### Readability

A meaningful function name can make a program easier to understand.

### Maintainability

If the calculation needs to change, it can be changed in one place.

### Testing

A function can be tested independently.

### Scientific reproducibility

A clearly defined function documents how a quantity was calculated.

This is especially important in scientific computing.

---

5.20 Function for Electrical Power

Electrical power is:

$$
P=VI.
$$

We can define:

```python
def calculate_power(V, I):
    return V * I
```

Then:

```python
P = calculate_power(12, 0.25)

print(P)
```

Output:

```text
3.0
```

The mathematical model is:

$$
P=VI.
$$

The Python implementation is:

```python
return V * I
```

---

5.21 Function with Scientific Units

The function:

```python
def calculate_power(V, I):
    return V * I
```

does not know that `V` is measured in volts and `I` in amperes.

We must define the units when using the function.

For example:

```python
V = 12       # V
I = 0.25     # A

P = calculate_power(V, I)

print(f"Power = {P} W")
```

Output:

```text
Power = 3.0 W
```

The comments document the expected units.

> **A function can perform numerical operations, but scientific meaning still depends on the programmer.**

---

5.22 Function with Multiple Steps

A function can contain several calculations.

Suppose we want to calculate both current and power for a resistor.

```python
def resistor_analysis(V, R):
    I = V / R
    P = V * I
    return I, P
```

We can call the function:

```python
I, P = resistor_analysis(5, 100)

print(I)
print(P)
```

Output:

```text
0.05
0.25
```

The function performs two related calculations.

This is an early example of creating a small scientific analysis procedure.

---

5.23 Functions and Loops Together

Loops and functions become particularly useful when used together.

Suppose:

```python
def calculate_current(V, R):
    return V / R
```

We can apply it to several voltages:

```python
R = 100

for V in range(1, 6):
    I = calculate_current(V, R)
    print(f"V = {V} V, I = {I:.3f} A")
```

Output:

```text
V = 1 V, I = 0.010 A
V = 2 V, I = 0.020 A
V = 3 V, I = 0.030 A
V = 4 V, I = 0.040 A
V = 5 V, I = 0.050 A
```

This is a powerful pattern:

$$
\boxed{
\text{Define calculation once}
\rightarrow
\text{Repeat calculation for many values}
}
$$

---

5.24 Semiconductor Example: Diode Current Model

A simplified diode model can be represented by:

$$
I=I_S
\left(
e^{\frac{V}{nV_T}}-1
\right),
$$

where:

- \(I\) is diode current;
- \(I_S\) is saturation current;
- \(V\) is applied voltage;
- \(n\) is the ideality factor;
- \(V_T\) is thermal voltage.

At this stage, we will not implement the exponential function because the required mathematical functions will be introduced later.

The important programming idea is that a scientific model can eventually be represented by a function.

For example, the structure might eventually become:

```python
def diode_current(V, Is, n, Vt):
    ...
```

This prepares us for later chapters, where scientific Python libraries will be introduced.

---

5.25 Function Parameters and Different Devices

Suppose the current calculation depends on resistance.

```python
def calculate_current(V, R):
    return V / R
```

The same function can be used for different resistors:

```python
print(calculate_current(5, 100))
print(calculate_current(5, 220))
print(calculate_current(5, 470))
```

Output:

```text
0.05
0.022727272727272728
0.010638297872340425
```

The calculation itself does not change.

Only the input parameters change.

This is one of the major benefits of functions.

---

5.26 Local Variables

Variables created inside a function normally belong to that function.

For example:

```python
def calculate_power(V, I):
    P = V * I
    return P
```

The variable `P` is used inside the function to calculate power.

The function returns its value.

This helps keep scientific calculations organized and reduces unnecessary interaction between different parts of a program.

A detailed discussion of variable scope will come later if needed.

---

5.27 Common Beginner Mistakes

### Mistake 1 — Forgetting indentation

Incorrect:

```python
for i in range(5):
print(i)
```

Correct:

```python
for i in range(5):
    print(i)
```

### Mistake 2 — Forgetting the colon

Incorrect:

```python
if x > 5
```

Correct:

```python
if x > 5:
```

The same rule applies to loops and function definitions.

### Mistake 3 — Forgetting to update a `while` loop

A `while` loop can become infinite if its controlling variable never changes.

### Mistake 4 — Confusing `return` and `print`

Consider:

```python
def calculate_current(V, R):
    print(V / R)
```

This displays the value but does not return it.

If another calculation needs the result, use:

```python
def calculate_current(V, R):
    return V / R
```

### Mistake 5 — Repeating code instead of creating a function

If the same scientific calculation appears many times, consider defining a function.

### Mistake 6 — Using unclear function names

Prefer:

```python
calculate_current()
```

over:

```python
calc1()
```

Meaningful names improve readability.

---

5.28 Worked Example: Generate a Current Table

A resistor has:

$$
R=220\,\Omega.
$$

Calculate current for:

$$
V=1,2,3,4,5\,\mathrm{V}.
$$

### Step 1 — Define the scientific relationship

$$
I=\frac{V}{R}.
$$

### Step 2 — Define a function

```python
def calculate_current(V, R):
    return V / R
```

### Step 3 — Repeat for several voltages

```python
R = 220

for V in range(1, 6):
    I = calculate_current(V, R)
    print(f"V = {V} V, I = {I:.4f} A")
```

Output:

```text
V = 1 V, I = 0.0045 A
V = 2 V, I = 0.0091 A
V = 3 V, I = 0.0136 A
V = 4 V, I = 0.0182 A
V = 5 V, I = 0.0227 A
```

The program has implemented the same physical relationship repeatedly without duplicating the calculation.

---

5.29 Worked Example: Classify Temperature Repeatedly

Suppose we have several temperature measurements:

```python
temperatures = [290, 310, 330, 360]
```

We want to classify each measurement as:

- Low: \(T<300\,\mathrm{K}\);
- Normal: \(300\leq T\leq350\,\mathrm{K}\);
- High: \(T>350\,\mathrm{K}\).

Define a function:

```python
def classify_temperature(T):
    if T < 300:
        return "Low"
    elif T <= 350:
        return "Normal"
    else:
        return "High"
```

Then use a loop:

```python
temperatures = [290, 310, 330, 360]

for T in temperatures:
    status = classify_temperature(T)
    print(f"T = {T} K, Status = {status}")
```

Output:

```text
T = 290 K, Status = Low
T = 310 K, Status = Normal
T = 330 K, Status = Normal
T = 360 K, Status = High
```

This example combines three ideas:

```text
Data
 ↓
Loop
 ↓
Function
 ↓
Decision
 ↓
Result
```

This pattern will become very important when we work with experimental datasets.

---

5.30 Exercises

### Exercise 1 — Basic `for` Loop

Write a program that prints the numbers:

$$
1,2,3,\ldots,10.
$$

Use `range()`.

---

### Exercise 2 — Squares

Use a `for` loop to calculate:

$$
1^2,2^2,3^2,\ldots,10^2.
$$

Display both the number and its square.

---

### Exercise 3 — Ohm's Law Table

A resistor has:

$$
R=100\,\Omega.
$$

Use a loop to calculate current for:

$$
V=1,2,\ldots,10\,\mathrm{V}.
$$

Display voltage and current.

---

### Exercise 4 — Function for Current

Write a function:

```python
calculate_current(V, R)
```

that returns

$$
I=\frac{V}{R}.
$$

Test it for at least three different values of \(V\) and \(R\).

---

### Exercise 5 — Function for Power

Write a function:

```python
calculate_power(V, I)
```

that returns:

$$
P=VI.
$$

Test it using:

$$
V=5\,\mathrm{V}
$$

and

$$
I=20\,\mathrm{mA}.
$$

Remember to convert the current to amperes.

---

### Exercise 6 — Temperature Classification

Write a function that classifies temperature as:

- Low if \(T<300\,\mathrm{K}\);
- Normal if \(300\leq T\leq350\,\mathrm{K}\);
- High if \(T>350\,\mathrm{K}\).

Use a loop to classify:

```python
temperatures = [280, 300, 325, 350, 375]
```

---

### Exercise 7 — `while` Loop

Write a program that starts at:

$$
V=0\,\mathrm{V}
$$

and increases voltage by \(1\,\mathrm{V}\) until it reaches \(5\,\mathrm{V}\).

Use a `while` loop.

---

5.31 Think and Apply

An experiment contains 500 voltage measurements.

A student writes the same three lines of Python code 500 times to calculate current.

1. Will the program work?
2. Why is this approach inefficient?
3. How can a loop help?
4. How can a function help?
5. What advantage do we get by combining the loop and function?

---

5.32 Chapter Summary

In this chapter, we learned that:

1. Loops allow Python to repeat calculations efficiently.
2. A `for` loop is useful for iterating through a known sequence or number of repetitions.
3. `range()` can generate integer sequences for loops.
4. A `while` loop repeats code while a condition remains true.
5. A `while` loop must have a mechanism that eventually makes its condition false.
6. Functions allow scientific calculations to be defined once and reused.
7. Function parameters allow the same procedure to work with different inputs.
8. `return` sends a result back from a function.
9. Loops and functions can be combined to perform repeated scientific calculations.
10. Decision making can be incorporated into functions.
11. Meaningful function names and clear structure improve scientific code.
12. Reusable code reduces duplication and supports reproducible analysis.

---

5.33 Key Takeaway

> **Loops automate repetition, while functions organize reusable calculations. Together, they allow a short Python program to perform the same scientific procedure across many measurements without repeatedly writing the same code.**

The next chapter introduces **lists, tuples, and dictionaries**, which provide structured ways to store and organize collections of scientific data.
