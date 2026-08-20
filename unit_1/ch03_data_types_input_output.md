# Chapter 3 — Data Types, Input and Output

## 3.1 Learning Objectives

After completing this chapter, you should be able to:

- explain why Python needs different data types;
- identify integers, floating-point numbers, strings, and Boolean values;
- use the `type()` function to inspect a value;
- convert values from one data type to another;
- use `input()` to accept information from a user;
- use `print()` to display results clearly;
- format scientific and engineering results;
- understand the difference between numerical data and text;
- use appropriate data types in simple scientific programs.

---

3.2 Why Do We Need Data Types?

A scientific program works with different kinds of information.

For example, a semiconductor experiment may involve:

- the number of measurements;
- voltage;
- current;
- temperature;
- device name;
- whether a measurement is valid;
- a calculated parameter.

These values do not all have the same nature.

Consider:

```python
voltage = 5.0
measurement_count = 100
device_name = "Silicon diode"
measurement_valid = True
```

Here:

- `5.0` is a numerical value;
- `100` is a whole number;
- `"Silicon diode"` is text;
- `True` represents a logical condition.

Python uses **data types** to distinguish these kinds of values.

A data type tells Python what kind of value it is dealing with and what operations are appropriate.

---

3.3 The Main Data Types in This Chapter

The four basic data types introduced here are:

| Data type | Python name | Example |
| --- | --- | --- |
| Integer | `int` | `25` |
| Floating-point number | `float` | `25.5` |
| String | `str` | `"Silicon"` |
| Boolean | `bool` | `True` |

These four types will appear repeatedly throughout scientific programming.

---

3.4 Integers

An integer is a whole number without a decimal part.

Examples:

```python
0
1
10
-5
100
```

Scientific examples include:

- number of measurements;
- number of devices tested;
- number of data points;
- number of experimental trials.

For example:

```python
number_of_measurements = 100
print(number_of_measurements)
```

Output:

```text
100
```

The value is an integer.

We can verify its type using `type()`:

```python
number_of_measurements = 100

print(type(number_of_measurements))
```

Output:

```text
<class 'int'>
```

The word `int` stands for **integer**.

---

3.5 Floating-Point Numbers

A floating-point number represents a number that can contain a fractional or decimal part.

Examples:

```python
5.0
3.14
0.25
-2.75
```

Scientific measurements are commonly represented using floating-point numbers.

For example:

```python
voltage = 5.0
current = 0.025
temperature = 300.15
```

Check the type:

```python
temperature = 300.15

print(type(temperature))
```

Output:

```text
<class 'float'>
```

The word `float` stands for **floating-point number**.

---

3.6 Integer and Floating-Point Values

Consider:

```python
a = 5
b = 5.0

print(type(a))
print(type(b))
```

Output:

```text
<class 'int'>
<class 'float'>
```

The numerical values are equal:

$$
5=5.0,
$$

but Python stores them as different data types.

This distinction becomes important when programs become more complex.

---

3.7 Strings

A string is a sequence of characters.

Strings are written inside quotation marks.

Examples:

```python
"Python"
"Semiconductor"
"Diode"
"Experiment 1"
```

For example:

```python
device_name = "Silicon diode"

print(device_name)
```

Output:

```text
Silicon diode
```

The type is:

```python
print(type(device_name))
```

Output:

```text
<class 'str'>
```

The abbreviation `str` stands for **string**.

Strings are useful for storing:

- device names;
- experiment names;
- sample identifiers;
- units written as text;
- labels;
- descriptions.

---

3.8 Strings Are Not Numbers

Consider:

```python
voltage = 5
```

and:

```python
voltage = "5"
```

The first stores a number.

The second stores the character `"5"` as text.

We can demonstrate the difference:

```python
a = 5
b = "5"

print(type(a))
print(type(b))
```

Output:

```text
<class 'int'>
<class 'str'>
```

This distinction is important when reading experimental data or accepting values from users.

---

3.9 Boolean Values

A Boolean value represents one of two logical states:

```python
True
False
```

For example:

```python
measurement_valid = True

print(measurement_valid)
```

Output:

```text
True
```

Check the type:

```python
print(type(measurement_valid))
```

Output:

```text
<class 'bool'>
```

Boolean values are useful for representing conditions such as:

- measurement valid / invalid;
- device operating / not operating;
- test passed / failed;
- temperature within limit / outside limit.

We will use Boolean values more extensively when we study decision-making.

---

3.10 The `type()` Function

Python provides the `type()` function to identify the data type of a value.

Examples:

```python
print(type(10))
print(type(10.5))
print(type("Python"))
print(type(True))
```

Output:

```text
<class 'int'>
<class 'float'>
<class 'str'>
<class 'bool'>
```

The `type()` function is especially useful when debugging a program.

If a calculation produces an unexpected result, checking the type of the values involved can help identify the problem.

---

3.11 Type Conversion

Sometimes we need to convert a value from one data type to another.

Common conversion functions include:

```python
int()
float()
str()
bool()
```

For example:

```python
x = "25"

print(type(x))
```

Output:

```text
<class 'str'>
```

The string can be converted into an integer:

```python
x = "25"

y = int(x)

print(y)
print(type(y))
```

Output:

```text
25
<class 'int'>
```

Similarly:

```python
x = "25.5"

y = float(x)

print(y)
print(type(y))
```

Output:

```text
25.5
<class 'float'>
```

---

3.12 Why Type Conversion Matters in Scientific Work

Suppose a measurement is provided as text:

```python
voltage = "5.0"
```

We cannot safely treat this as a numerical voltage until it is converted.

```python
voltage = "5.0"

voltage = float(voltage)

print(voltage)
```

Output:

```text
5.0
```

Now Python can perform numerical calculations using `voltage`.

For example:

```python
voltage = "5.0"
voltage = float(voltage)

resistance = 100
current = voltage / resistance

print(current)
```

Output:

```text
0.05
```

This becomes particularly important when data is read from files or entered by users.

---

3.13 Input from the User

The `input()` function allows a program to receive information from the user.

For example:

```python
name = input("Enter your name: ")

print("Hello", name)
```

If the user enters:

```text
Suresh
```

the output is:

```text
Hello Suresh
```

The important point is that `input()` returns the user's response as a **string**.

---

3.14 Numerical Input

Consider:

```python
voltage = input("Enter voltage: ")

print(type(voltage))
```

If the user enters:

```text
5
```

the output is:

```text
<class 'str'>
```

Even though the user entered a number, `input()` returns text.

Therefore, numerical input normally needs to be converted.

```python
voltage = float(input("Enter voltage: "))

print(voltage)
```

If the user enters:

```text
5
```

the output is:

```text
5.0
```

Now `voltage` is a floating-point number.

---

3.15 A Simple Interactive Scientific Calculation

We can combine input, conversion, calculation, and output.

Suppose we want a user to enter voltage and resistance and calculate current.

The mathematical model is:

$$
I=\frac{V}{R}.
$$

Python:

```python
V = float(input("Enter voltage (V): "))
R = float(input("Enter resistance (ohm): "))

I = V / R

print(I)
```

Suppose the user enters:

```text
Enter voltage (V): 5
Enter resistance (ohm): 100
```

Output:

```text
0.05
```

The complete workflow is:

$$
\boxed{
\text{User input}
\rightarrow
\text{Type conversion}
\rightarrow
\text{Calculation}
\rightarrow
\text{Output}
}
$$

---

3.16 Making Output Meaningful

The output

```text
0.05
```

is mathematically correct but not very informative.

A better output is:

```python
V = 5
R = 100

I = V / R

print("Current =", I, "A")
```

Output:

```text
Current = 0.05 A
```

Now the quantity and unit are visible.

This is an important principle in scientific programming:

> **Output should communicate the scientific meaning of a result, not merely display a number.**

---

3.17 Combining Strings and Values

Python's `print()` function can display several items.

```python
V = 5
R = 100
I = V / R

print("Voltage =", V, "V")
print("Resistance =", R, "ohm")
print("Current =", I, "A")
```

Output:

```text
Voltage = 5 V
Resistance = 100 ohm
Current = 0.05 A
```

This approach is useful for simple programs.

Later, we will use more powerful formatting techniques.

---

3.18 Formatted Strings

Python provides **f-strings** for convenient output formatting.

For example:

```python
V = 5
R = 220
I = V / R

print(f"Voltage = {V} V")
print(f"Resistance = {R} ohm")
print(f"Current = {I} A")
```

Output:

```text
Voltage = 5 V
Resistance = 220 ohm
Current = 0.022727272727272728 A
```

We can also control the number of decimal places.

```python
print(f"Current = {I:.3f} A")
```

Output:

```text
Current = 0.023 A
```

The expression

```text
{I:.3f}
```

means that the value of `I` should be displayed with three digits after the decimal point.

Remember:

> Formatting changes how a value is displayed. It does not necessarily change the underlying value.

---

3.19 Scientific Output

For very small or very large values, scientific notation can make output easier to read.

For example:

```python
current = 2.5e-6

print(f"Current = {current:.2e} A")
```

Output:

```text
Current = 2.50e-06 A
```

This corresponds to:

$$
I=2.50\times10^{-6}\,\mathrm{A}.
$$

Scientific notation is particularly useful in semiconductor science because many device quantities can be very small.

---

3.20 A Semiconductor Example

Suppose a semiconductor device has a measured current of

$$
I=250\,\mu\mathrm{A}.
$$

We can enter the value using scientific notation:

```python
current = 250e-6

print(f"Current = {current:.2e} A")
```

Output:

```text
Current = 2.50e-04 A
```

Mathematically,

$$
250\,\mu\mathrm{A}
=
250\times10^{-6}\,\mathrm{A}
=
2.50\times10^{-4}\,\mathrm{A}.
$$

Python stores the numerical value in amperes. The scientist must keep track of the unit.

---

3.21 Strings and Scientific Labels

Strings are useful when we want to describe data.

For example:

```python
device = "Silicon diode"
measurement = "Forward current"

print(device)
print(measurement)
```

Output:

```text
Silicon diode
Forward current
```

We can combine labels with numerical values:

```python
device = "Silicon diode"
voltage = 0.7
current = 0.01

print(f"Device: {device}")
print(f"Voltage: {voltage} V")
print(f"Current: {current} A")
```

Output:

```text
Device: Silicon diode
Voltage: 0.7 V
Current: 0.01 A
```

This becomes useful when preparing readable experimental reports.

---

3.22 A Simple Measurement Record

Consider a measurement with:

$$
V=0.70\,\mathrm{V},
$$

$$
I=10\,\mathrm{mA}.
$$

We can represent the measurement as:

```python
device = "Silicon diode"
voltage = 0.70
current = 10e-3

print(f"Device: {device}")
print(f"Voltage: {voltage:.2f} V")
print(f"Current: {current:.3f} A")
```

Output:

```text
Device: Silicon diode
Voltage: 0.70 V
Current: 0.010 A
```

The program combines different data types:

- `device` → string;
- `voltage` → float;
- `current` → float.

Later chapters will show how multiple such measurements can be organized and analyzed.

---

3.23 Comparing Data Types in a Scientific Program

Consider:

```python
sample_id = "D01"
temperature = 300.15
measurement_count = 20
measurement_valid = True
```

The variables represent different types of information:

| Variable | Example value | Type |
| --- | --- | --- |
| `sample_id` | `"D01"` | `str` |
| `temperature` | `300.15` | `float` |
| `measurement_count` | `20` | `int` |
| `measurement_valid` | `True` | `bool` |

A scientific dataset often contains several kinds of information at the same time.

Understanding data types is therefore essential before we begin working with larger datasets.

---

3.24 Common Beginner Mistakes

### Mistake 1 — Forgetting that `input()` returns a string

This code:

```python
V = input("Enter voltage: ")
```

does not create a numerical variable.

If numerical calculation is required, use:

```python
V = float(input("Enter voltage: "))
```

### Mistake 2 — Trying to perform arithmetic with text

For example:

```python
V = "5"
R = 100

I = V / R
```

will produce an error because `V` is a string.

Convert it first:

```python
V = float(V)
```

### Mistake 3 — Confusing `"5"` with `5`

```python
"5"
```

is text.

```python
5
```

is a number.

### Mistake 4 — Assuming formatting changes the value

Displaying:

```text
0.023
```

instead of:

```text
0.022727...
```

does not automatically mean that the stored value has become \(0.023\).

### Mistake 5 — Using unclear output

Prefer:

```text
Current = 0.05 A
```

over:

```text
0.05
```

when presenting scientific results.

---

3.25 Worked Example: Interactive Ohm's Law Calculator

Create a program that accepts voltage and resistance from the user and calculates current.

### Mathematical model

$$
I=\frac{V}{R}.
$$

### Python

```python
V = float(input("Enter voltage (V): "))
R = float(input("Enter resistance (ohm): "))

I = V / R

print(f"Current = {I:.4f} A")
print(f"Current = {I * 1000:.2f} mA")
```

Suppose the user enters:

```text
Enter voltage (V): 5
Enter resistance (ohm): 220
```

Output:

```text
Current = 0.0227 A
Current = 22.73 mA
```

### Scientific interpretation

The calculated current is approximately

$$
I=0.0227\,\mathrm{A}
$$

or

$$
I=22.73\,\mathrm{mA}.
$$

The program demonstrates several concepts together:

- input;
- type conversion;
- variables;
- arithmetic;
- unit conversion;
- formatted output.

---

3.26 Worked Example: Device Measurement Record

Suppose an experiment records the following information:

- device: silicon diode;
- applied voltage: \(0.65\,\mathrm{V}\);
- current: \(2.5\,\mathrm{mA}\);
- temperature: \(300\,\mathrm{K}\).

Write a program to display the information clearly.

```python
device = "Silicon diode"
voltage = 0.65
current = 2.5e-3
temperature = 300

print(f"Device      : {device}")
print(f"Voltage     : {voltage:.2f} V")
print(f"Current     : {current:.2e} A")
print(f"Temperature : {temperature:.1f} K")
```

Output:

```text
Device      : Silicon diode
Voltage     : 0.65 V
Current     : 2.50e-03 A
Temperature : 300.0 K
```

The program is not performing a complex calculation. Its purpose is to demonstrate how Python can represent different kinds of scientific information and present them clearly.

---

3.27 Exercises

### Exercise 1 — Identify the Data Type

Determine the Python data type of each value:

```python
25
25.0
"25"
True
```

Use `type()` to verify your answers.

---

### Exercise 2 — Numerical Input

Write a program that asks the user to enter:

- voltage;
- resistance.

Convert both values to floating-point numbers and calculate the current using

$$
I=\frac{V}{R}.
$$

---

### Exercise 3 — Formatted Output

Modify the program from Exercise 2 so that the output appears as:

```text
Voltage    = 5.00 V
Resistance = 220.00 ohm
Current    = 22.73 mA
```

---

### Exercise 4 — Temperature

Ask the user to enter a temperature in Celsius.

Convert it to Kelvin using

$$
T_K=T_C+273.15.
$$

Display the result with two decimal places.

---

### Exercise 5 — Semiconductor Current

Ask the user to enter a current in microamperes.

Convert the value to amperes.

For example, if the user enters:

```text
250
```

the program should calculate:

$$
250\,\mu\mathrm{A}
=
2.5\times10^{-4}\,\mathrm{A}.
$$

---

### Exercise 6 — Measurement Information

Create variables for:

- sample ID;
- device type;
- voltage;
- current;
- temperature.

Display all values in a clearly formatted output.

---

3.28 Think and Apply

A student writes:

```python
V = input("Enter voltage: ")
R = input("Enter resistance: ")

I = V / R
```

The program produces an error.

Explain why.

What changes are required to make the program perform the calculation correctly?

---

3.29 Chapter Summary

In this chapter, we learned that:

1. Python works with different types of data.
2. The basic data types introduced here are `int`, `float`, `str`, and `bool`.
3. `type()` can be used to inspect a value's data type.
4. Values can be converted using functions such as `int()`, `float()`, and `str()`.
5. `input()` accepts information from the user, but the returned value is a string.
6. Numerical input usually needs to be converted before mathematical calculations.
7. `print()` displays information.
8. f-strings provide a convenient way to format scientific output.
9. Scientific notation is useful for representing very large and very small quantities.
10. Scientific programs often contain different types of information at the same time.
11. Clear output should communicate the quantity, value, and unit.
12. Data types and input/output provide the foundation for working with experimental data.

---

3.30 Key Takeaway

> **Scientific programs rarely work with numbers alone. They work with measurements, labels, identifiers, conditions, and results. Understanding Python data types and learning to control input and output are essential steps toward working with real scientific datasets.**

The next chapter introduces **decision making with `if`, `elif`, and `else`**, allowing a Python program to respond differently to different scientific conditions.
