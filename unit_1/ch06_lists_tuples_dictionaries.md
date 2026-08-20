# Chapter 6 — Lists, Tuples, and Dictionaries

## 6.1 Learning Objectives

After completing this chapter, you should be able to:

- explain why collections of values are needed in scientific programming;
- create and access Python lists;
- use indexing and slicing with lists;
- modify list elements;
- add and remove values from lists;
- use lists with loops;
- understand tuples and their main characteristics;
- create dictionaries using key-value pairs;
- retrieve and modify dictionary values;
- choose an appropriate data structure for simple scientific data;
- organize basic electronics and semiconductor measurements using Python collections.

---

6.2 Why Do We Need Collections?

So far, we have worked mainly with individual values.

For example:

```python
V = 5
I = 0.05
R = 100
```

An experiment, however, rarely produces only one measurement.

Suppose voltage is measured at five points:

$$
V_1=1\,\mathrm{V},
\quad
V_2=2\,\mathrm{V},
\quad
V_3=3\,\mathrm{V},
\quad
V_4=4\,\mathrm{V},
\quad
V_5=5\,\mathrm{V}.
$$

We could create five separate variables:

```python
V1 = 1
V2 = 2
V3 = 3
V4 = 4
V5 = 5
```

This quickly becomes inconvenient.

What if an experiment contains 1,000 measurements?

Python provides **collections** that allow multiple values to be stored together.

The three collections introduced in this chapter are:

- lists;
- tuples;
- dictionaries.

---

6.3 Lists

A **list** is an ordered collection of values.

A list is created using square brackets:

```python
voltages = [1, 2, 3, 4, 5]
```

The variable `voltages` now contains five values.

We can display the list:

```python
print(voltages)
```

Output:

```text
[1, 2, 3, 4, 5]
```

Lists are one of the most commonly used Python data structures.

---

6.4 Why Lists Are Useful in Scientific Work

A list can represent a collection of measurements.

For example:

```python
temperatures = [298.15, 300.15, 302.15, 304.15]
```

or:

```python
currents = [0.01, 0.015, 0.021, 0.030]
```

A list allows the measurements to be handled as one collection rather than as separate variables.

This becomes particularly useful when combined with loops.

---

6.5 Indexing a List

Each element of a list has a position called an **index**.

Python uses zero-based indexing.

Consider:

```python
voltages = [1, 2, 3, 4, 5]
```

The positions are:

```text
Value:   1   2   3   4   5
Index:   0   1   2   3   4
```

Therefore:

```python
print(voltages[0])
```

Output:

```text
1
```

and:

```python
print(voltages[3])
```

Output:

```text
4
```

The first element is always at index `0`.

---

6.6 Accessing the Last Element

Python also allows negative indexing.

For:

```python
voltages = [1, 2, 3, 4, 5]
```

the index `-1` refers to the last element:

```python
print(voltages[-1])
```

Output:

```text
5
```

Similarly:

```python
print(voltages[-2])
```

Output:

```text
4
```

Negative indexing is useful when we need values relative to the end of a collection.

---

6.7 Length of a List

The `len()` function returns the number of elements in a list.

```python
voltages = [1, 2, 3, 4, 5]

print(len(voltages))
```

Output:

```text
5
```

For scientific data, `len()` can tell us how many observations are currently stored.

For example:

```python
current_measurements = [0.01, 0.02, 0.03, 0.04]

print(len(current_measurements))
```

Output:

```text
4
```

---

6.8 Iterating Through a List

A list can be processed using a `for` loop.

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

This is one of the most important patterns in Python:

```text
Collection
    ↓
Loop
    ↓
Process each value
```

---

6.9 Applying a Calculation to a List

Suppose a resistor has:

$$
R=100\,\Omega.
$$

The measured voltages are:

```python
voltages = [1, 2, 3, 4, 5]
```

We can calculate current for each voltage:

```python
R = 100
voltages = [1, 2, 3, 4, 5]

for V in voltages:
    I = V / R
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

The list stores the inputs, while the loop processes them.

---

6.10 Modifying a List

Lists are **mutable**, meaning their elements can be changed.

Consider:

```python
voltages = [1, 2, 3, 4, 5]

voltages[2] = 3.3

print(voltages)
```

Output:

```text
[1, 2, 3.3, 4, 5]
```

The value at index `2` has been changed from `3` to `3.3`.

This is useful when correcting or updating data.

---

6.11 Adding Values to a List

The `append()` method adds a value to the end of a list.

```python
voltages = [1, 2, 3]

voltages.append(4)

print(voltages)
```

Output:

```text
[1, 2, 3, 4]
```

We can continue adding values:

```python
voltages.append(5)
```

The list becomes:

```text
[1, 2, 3, 4, 5]
```

This is useful when measurements become available one at a time.

---

6.12 Inserting Values

The `insert()` method adds an element at a specified position.

```python
voltages = [1, 3, 4]

voltages.insert(1, 2)

print(voltages)
```

Output:

```text
[1, 2, 3, 4]
```

The first argument specifies the index.

The second argument specifies the value to insert.

---

6.13 Removing Values

The `remove()` method removes a specified value.

```python
voltages = [1, 2, 3, 4]

voltages.remove(3)

print(voltages)
```

Output:

```text
[1, 2, 4]
```

The `pop()` method removes an element using its index.

```python
voltages = [1, 2, 3, 4]

removed = voltages.pop(2)

print(removed)
print(voltages)
```

Output:

```text
3
[1, 2, 4]
```

Data modification should be performed carefully when working with experimental measurements because changing or removing observations can affect later analysis.

---

6.14 Slicing a List

Slicing allows us to select part of a list.

Consider:

```python
voltages = [1, 2, 3, 4, 5]
```

The expression:

```python
voltages[1:4]
```

returns:

```text
[2, 3, 4]
```

The general form is:

```python
list[start:stop]
```

The `stop` index is not included.

For example:

```python
voltages[0:3]
```

returns:

```text
[1, 2, 3]
```

Slicing is useful when working with subsets of measurements.

---

6.15 Slicing with a Step

We can also specify a step:

```python
voltages = [1, 2, 3, 4, 5, 6]

print(voltages[0:6:2])
```

Output:

```text
[1, 3, 5]
```

The structure is:

```python
[start:stop:step]
```

---

6.16 Lists Can Contain Different Data Types

Python lists can contain different types of values.

For example:

```python
measurement = ["D01", 0.7, 0.0025, 300]
```

This list contains:

- a string;
- floating-point values;
- an integer.

However, for numerical scientific analysis, it is usually clearer to keep collections of numerical measurements separate from descriptive information.

For example:

```python
sample_id = "D01"
voltage = 0.7
current = 0.0025
temperature = 300
```

Later, structured data tools will provide better ways to represent such records.

---

6.17 Lists and Functions

Functions can accept lists as arguments.

For example:

```python
def display_measurements(values):
    for value in values:
        print(value)
```

Then:

```python
voltages = [1, 2, 3, 4, 5]

display_measurements(voltages)
```

Output:

```text
1
2
3
4
5
```

This demonstrates how functions and collections can work together.

---

6.18 Lists of Experimental Measurements

Suppose a diode experiment produces:

```python
voltage = [0.1, 0.2, 0.3, 0.4, 0.5]
current = [0.001, 0.002, 0.004, 0.008, 0.015]
```

Each position represents a paired observation.

For example:

$$
(V_1,I_1)=(0.1,0.001),
$$

$$
(V_2,I_2)=(0.2,0.002),
$$

and so on.

We can process corresponding values using `zip()`:

```python
for V, I in zip(voltage, current):
    print(f"V = {V:.1f} V, I = {I:.3f} A")
```

Output:

```text
V = 0.1 V, I = 0.001 A
V = 0.2 V, I = 0.002 A
V = 0.3 V, I = 0.004 A
V = 0.4 V, I = 0.008 A
V = 0.5 V, I = 0.015 A
```

The `zip()` function pairs elements by position.

This idea is important for experimental datasets.

---

6.19 Tuples

A tuple is another ordered collection.

Tuples are written using parentheses:

```python
measurement = (0.7, 0.0025)
```

The two values could represent:

$$
(V,I)=(0.7\,\mathrm{V},0.0025\,\mathrm{A}).
$$

We can access elements using indexing:

```python
print(measurement[0])
print(measurement[1])
```

Output:

```text
0.7
0.0025
```

Tuples are similar to lists but have an important difference.

A tuple is **immutable**.

---

6.20 Lists versus Tuples

Consider:

```python
values = [1, 2, 3]
```

This can be modified:

```python
values[0] = 10
```

But:

```python
values = (1, 2, 3)
```

cannot be modified in the same way.

Trying:

```python
values[0] = 10
```

produces an error.

A simple comparison is:

| Feature | List | Tuple |
| --- | --- | --- |
| Ordered | Yes | Yes |
| Indexed | Yes | Yes |
| Mutable | Yes | No |
| Syntax | `[]` | `()` |

Tuples are useful when a collection should remain fixed.

---

6.21 A Measurement as a Tuple

A single measurement can naturally be represented as a tuple.

For example:

```python
measurement = (0.65, 0.010)
```

which may represent:

$$
(V,I)=(0.65\,\mathrm{V},0.010\,\mathrm{A}).
$$

We can unpack the tuple:

```python
V, I = measurement

print(V)
print(I)
```

Output:

```text
0.65
0.01
```

Tuple unpacking provides a convenient way to assign the elements to meaningful variables.

---

6.22 A Collection of Measurements

We can create a list of tuples:

```python
measurements = [
    (0.1, 0.001),
    (0.2, 0.002),
    (0.3, 0.004),
    (0.4, 0.008)
]
```

Each tuple represents one observation.

We can process the measurements:

```python
for V, I in measurements:
    print(f"V = {V:.1f} V, I = {I:.3f} A")
```

Output:

```text
V = 0.1 V, I = 0.001 A
V = 0.2 V, I = 0.002 A
V = 0.3 V, I = 0.004 A
V = 0.4 V, I = 0.008 A
```

This structure is useful for understanding how paired experimental data can be represented before we introduce NumPy arrays.

---

6.23 Dictionaries

A dictionary stores information as **key-value pairs**.

A dictionary is written using curly brackets:

```python
device = {
    "name": "Silicon diode",
    "voltage": 0.7,
    "current": 0.01
}
```

Here:

- `"name"` is a key;
- `"Silicon diode"` is its value;
- `"voltage"` is a key;
- `0.7` is its value;
- `"current"` is a key;
- `0.01` is its value.

---

6.24 Accessing Dictionary Values

We can retrieve a value using its key:

```python
print(device["name"])
```

Output:

```text
Silicon diode
```

Similarly:

```python
print(device["voltage"])
```

Output:

```text
0.7
```

The key provides a meaningful label for the value.

This is one of the main advantages of dictionaries.

---

6.25 Why Dictionaries Are Useful

Consider:

```python
measurement = {
    "sample_id": "D01",
    "voltage": 0.7,
    "current": 0.0025,
    "temperature": 300
}
```

The data is self-describing.

We do not need to remember that:

```text
measurement[1]
```

means voltage.

Instead:

```python
measurement["voltage"]
```

clearly identifies the quantity.

Dictionaries are therefore useful for storing descriptive information about an individual scientific observation.

---

6.26 Modifying a Dictionary

Dictionary values can be changed.

```python
measurement = {
    "voltage": 0.7,
    "current": 0.0025
}

measurement["voltage"] = 0.75

print(measurement)
```

Output:

```text
{'voltage': 0.75, 'current': 0.0025}
```

A new key-value pair can also be added:

```python
measurement["temperature"] = 300
```

The dictionary becomes:

```text
{'voltage': 0.75, 'current': 0.0025, 'temperature': 300}
```

---

6.27 Looping Through a Dictionary

We can loop through dictionary keys:

```python
measurement = {
    "voltage": 0.7,
    "current": 0.0025,
    "temperature": 300
}

for key in measurement:
    print(key)
```

Output:

```text
voltage
current
temperature
```

We can retrieve both keys and values using `.items()`:

```python
for key, value in measurement.items():
    print(key, value)
```

Output:

```text
voltage 0.7
current 0.0025
temperature 300
```

---

6.28 Dictionary for Device Information

A dictionary can represent basic information about a semiconductor device:

```python
device = {
    "device_type": "Silicon diode",
    "material": "Silicon",
    "temperature_K": 300,
    "forward_voltage_V": 0.7
}
```

We can access individual properties:

```python
print(device["material"])
print(device["forward_voltage_V"])
```

Output:

```text
Silicon
0.7
```

This resembles a simple record.

---

6.29 Choosing the Right Data Structure

Different structures are useful for different purposes.

### List

Use a list when you need an ordered collection that may change.

Example:

```python
temperatures = [298, 300, 302, 304]
```

### Tuple

Use a tuple when you need an ordered collection that should not be changed.

Example:

```python
measurement = (0.7, 0.0025)
```

### Dictionary

Use a dictionary when values need meaningful labels.

Example:

```python
measurement = {
    "voltage": 0.7,
    "current": 0.0025
}
```

A simple decision guide is:

```text
Need a sequence?
      ↓
     List
      |
Need a fixed sequence?
      ↓
    Tuple

Need labelled values?
      ↓
  Dictionary
```

---

6.30 Combining Collections

Python allows collections to be combined.

For example, a list can contain dictionaries:

```python
measurements = [
    {
        "voltage": 0.5,
        "current": 0.001
    },
    {
        "voltage": 0.6,
        "current": 0.002
    },
    {
        "voltage": 0.7,
        "current": 0.004
    }
]
```

We can process them:

```python
for measurement in measurements:
    print(
        f"V = {measurement['voltage']} V, "
        f"I = {measurement['current']} A"
    )
```

Output:

```text
V = 0.5 V, I = 0.001 A
V = 0.6 V, I = 0.002 A
V = 0.7 V, I = 0.004 A
```

This is a useful conceptual bridge toward structured datasets.

Later, NumPy and other scientific tools will provide more efficient representations for numerical data.

---

6.31 Common Beginner Mistakes

### Mistake 1 — Forgetting that indexing starts at zero

For:

```python
values = [10, 20, 30]
```

the first element is:

```python
values[0]
```

not:

```python
values[1]
```

### Mistake 2 — Using the wrong brackets

Lists:

```python
[1, 2, 3]
```

Tuples:

```python
(1, 2, 3)
```

Dictionaries:

```python
{"voltage": 5}
```

### Mistake 3 — Modifying a tuple

Tuples are immutable.

### Mistake 4 — Confusing dictionary keys and values

In:

```python
{"voltage": 5}
```

`"voltage"` is the key and `5` is the value.

### Mistake 5 — Mixing unrelated measurements carelessly

A list such as:

```python
[5, "voltage", 0.02, True]
```

is technically valid but may not be a sensible structure for numerical analysis.

Choose a data structure according to the scientific problem.

---

6.32 Worked Example: Store and Process Experimental Data

Suppose a simple experiment produces voltage-current pairs:

$$
(0.1,0.001),
(0.2,0.002),
(0.3,0.004),
(0.4,0.008).
$$

Represent the data as a list of tuples:

```python
measurements = [
    (0.1, 0.001),
    (0.2, 0.002),
    (0.3, 0.004),
    (0.4, 0.008)
]
```

Process the measurements:

```python
for V, I in measurements:
    print(f"Voltage = {V:.1f} V, Current = {I:.3f} A")
```

Output:

```text
Voltage = 0.1 V, Current = 0.001 A
Voltage = 0.2 V, Current = 0.002 A
Voltage = 0.3 V, Current = 0.004 A
Voltage = 0.4 V, Current = 0.008 A
```

The program now treats the measurements as a collection rather than as unrelated variables.

---

6.33 Worked Example: Device Record

Create a dictionary containing information about a semiconductor device.

```python
device = {
    "sample_id": "D01",
    "device": "Silicon diode",
    "temperature_K": 300,
    "forward_voltage_V": 0.70,
    "current_A": 0.0025
}
```

Display the information:

```python
print(f"Sample ID       : {device['sample_id']}")
print(f"Device          : {device['device']}")
print(f"Temperature     : {device['temperature_K']} K")
print(f"Forward voltage : {device['forward_voltage_V']:.2f} V")
print(f"Current         : {device['current_A']:.2e} A")
```

Output:

```text
Sample ID       : D01
Device          : Silicon diode
Temperature     : 300 K
Forward voltage : 0.70 V
Current         : 2.50e-03 A
```

This is a simple example of a self-describing scientific record.

---

6.34 Exercises

### Exercise 1 — Create a List

Create a list containing the voltages:

$$
1,2,3,4,5\,\mathrm{V}.
$$

Print the list and its length.

---

### Exercise 2 — Indexing

For:

```python
temperatures = [290, 300, 310, 320, 330]
```

print:

1. the first value;
2. the third value;
3. the last value.

---

### Exercise 3 — Slicing

Using:

```python
voltages = [1, 2, 3, 4, 5, 6]
```

create a slice containing:

```text
2, 3, 4
```

---

### Exercise 4 — List and Loop

Create a list of five voltages.

Use a loop to calculate current for a resistor of:

$$
R=100\,\Omega.
$$

---

### Exercise 5 — Tuple

Create a tuple containing:

$$
V=0.7\,\mathrm{V}
$$

and

$$
I=2\,\mathrm{mA}.
$$

Unpack the tuple into two variables.

---

### Exercise 6 — Dictionary

Create a dictionary containing:

- sample ID;
- material;
- temperature;
- voltage;
- current.

Display each value using its key.

---

### Exercise 7 — Measurement Records

Create a list containing three measurement tuples.

Each tuple should contain:

```text
(voltage, current)
```

Use a loop to display the measurements.

---

### Exercise 8 — Device Records

Create a list of two dictionaries representing two semiconductor samples.

Each dictionary should contain:

- sample ID;
- device type;
- temperature;
- voltage;
- current.

Use a loop to display the information.

---

6.35 Think and Apply

Suppose a laboratory experiment produces 500 voltage-current measurements.

A student proposes storing them as:

```python
V1 = ...
V2 = ...
V3 = ...
...
V500 = ...
```

Another student proposes:

```python
voltages = [...]
```

Explain why the second approach is more suitable.

Then consider the following:

```python
measurement = {
    "voltage": 0.7,
    "current": 0.0025,
    "temperature": 300
}
```

Why might a dictionary be useful for describing one measurement?

---

6.36 Chapter Summary

In this chapter, we learned that:

1. Scientific programs often need to work with collections of values.
2. Lists store ordered and changeable collections.
3. Python uses zero-based indexing.
4. Negative indexing can access values from the end of a list.
5. Slicing allows subsets of lists to be selected.
6. `append()`, `insert()`, `remove()`, and `pop()` can modify lists.
7. Lists work naturally with loops.
8. Tuples store ordered collections that cannot normally be modified.
9. Dictionaries store labelled information using key-value pairs.
10. Dictionaries are useful for representing self-describing scientific records.
11. Lists, tuples, and dictionaries serve different purposes.
12. Collections provide an important bridge between simple Python programs and real scientific datasets.

---

6.37 Key Takeaway

> **Real scientific work involves collections of measurements, not isolated numbers. Lists, tuples, and dictionaries provide the basic structures needed to organize those measurements before we move to specialized scientific tools such as NumPy.**

The next chapter introduces **basic file handling**, allowing Python programs to read data from files and save results for later use.
