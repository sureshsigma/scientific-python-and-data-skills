# Chapter 1 — Introduction to Python

## Learning Objectives

After completing this chapter, you should be able to:

- explain what programming is and why it is useful in scientific work;
- describe what Python is and why it is widely used in science and engineering;
- identify applications of Python in electronics and semiconductor technology;
- distinguish between Python, a development environment, and a Python library;
- run a simple Python program in a Jupyter Notebook or Google Colab;
- use Python to perform a simple scientific calculation;
- recognize the relationship between a scientific problem, its mathematical representation, Python code, and the resulting interpretation.

---

## 1.1 Why Programming?

A computer can perform calculations extremely quickly, but it does not decide by itself what calculation should be performed. We must provide a sequence of instructions.

This process of writing instructions for a computer is called **programming**.

Consider a simple electrical calculation. Suppose a resistor has resistance \(R\) and a voltage \(V\) is applied across it. From Ohm's law,

$$
V = IR,
$$

where:

- \(V\) is voltage,
- \(I\) is current,
- \(R\) is resistance.

If we want to calculate current, we rearrange the equation:

$$
I = \frac{V}{R}.
$$

For example, if

$$
V = 5\,\mathrm{V}
\qquad\text{and}\qquad
R = 100\,\Omega,
$$

then

$$
I = \frac{5\,\mathrm{V}}{100\,\Omega}
  = 0.05\,\mathrm{A}
  = 50\,\mathrm{mA}.
$$

A calculator can perform this calculation easily.

The situation changes when an experiment produces hundreds or thousands of measurements. Repeating the same calculation manually becomes inefficient and increases the possibility of errors.

A program allows us to describe the calculation once and apply it repeatedly.

> **Key idea:** Programming is a way of converting a procedure into instructions that a computer can execute.

---

## 1.2 What Is Python?

**Python is a high-level, general-purpose programming language.**

The terms *high-level* and *general-purpose* describe important characteristics of Python.

### High-level language

A high-level programming language provides instructions that are relatively easy for humans to read and understand.

For example:

```{code-cell} python
I = V / R
```

is easy to relate to the mathematical expression

$$
I = \frac{V}{R}.
$$

### General-purpose language

Python is not designed for only one discipline. It is used for:

- scientific computing;
- engineering;
- data analysis;
- artificial intelligence;
- automation;
- finance;
- education;
- research;
- software development.

This flexibility is one reason Python is useful for students of semiconductor science and technology.

---

## 1.3 Why Python for Scientific Work?

Scientific work often combines four elements:

$$
\boxed{
\text{Mathematics}
+
\text{Data}
+
\text{Computation}
+
\text{Interpretation}
}
$$

Python provides a practical connection between these elements.

A typical scientific workflow can be represented as:

```text
Scientific problem
       ↓
Experiment or simulation
       ↓
Measurements / data
       ↓
Python
       ↓
Numerical computation
       ↓
Visualization
       ↓
Analysis
       ↓
Scientific interpretation
```

Python is therefore not simply a programming language in this context. It becomes a **tool for scientific work**.

---

## 1.4 Python in Science

Python is used across many scientific disciplines.

### Physics

Python can be used for:

- numerical calculations;
- simulation;
- experimental data analysis;
- signal processing;
- visualization.

### Chemistry

Python can support:

- spectroscopy data analysis;
- molecular calculations;
- numerical modelling;
- visualization.

### Biology

Applications include:

- biological data analysis;
- image analysis;
- statistical analysis;
- computational biology.

### Materials Science

Python can be used for:

- experimental measurements;
- material characterization;
- numerical modelling;
- property analysis.

Although the scientific questions differ between disciplines, the computational workflow is often similar:

> **Collect data → process data → calculate → visualize → interpret.**

---

## 1.5 Python in Engineering

Engineering problems frequently involve repeated calculations, measurements, simulations, and optimization.

Python can be used for:

- numerical computation;
- simulation;
- measurement-data processing;
- automation;
- signal processing;
- optimization;
- visualization.

For example, suppose a temperature sensor records the temperature of a device every second.

A dataset may contain hundreds or thousands of observations:

$$
T_1,T_2,T_3,\ldots,T_n.
$$

Python can be used to calculate quantities such as the mean temperature,

$$
\bar{T}
=
\frac{1}{n}
\sum_{i=1}^{n}T_i,
$$

the minimum and maximum temperature, and the standard deviation.

---

## 1.6 Python in Electronics

Electronics provides many natural applications for programming.

Consider a measurement system that records voltage and current.

The observations may be represented as pairs:

$$
(V_1,I_1),\,
(V_2,I_2),\,
\ldots,\,
(V_n,I_n).
$$

Python can help us:

1. store the measurements;
2. perform calculations;
3. check and organize the data;
4. create graphs;
5. compare measurements;
6. analyze trends.

For example, voltage-current measurements can be used to construct an \(I\)-\(V\) characteristic.

> **Python produces the graph; scientific knowledge is required to interpret the graph.**

---

## 1.7 Python in Semiconductor Technology

Semiconductor science provides many applications for scientific programming.

### Semiconductor devices

Python can be used when working with data from:

- diodes;
- Zener diodes;
- LEDs;
- photodiodes;
- solar cells;
- transistors;
- sensors.

### Measurements

Typical measurements may include:

- voltage;
- current;
- temperature;
- resistance;
- conductivity;
- optical response.

### Analysis

Python can support:

- \(I\)-\(V\) characteristics;
- temperature-dependent measurements;
- parameter estimation;
- numerical calculations;
- statistical analysis;
- visualization.

A typical semiconductor-data workflow is:

```text
Semiconductor device
        ↓
Experiment / simulation
        ↓
Measurements
        ↓
Data
        ↓
Python
        ↓
Analysis
        ↓
Scientific interpretation
```

Python does not replace semiconductor physics. It provides computational tools for working with the data generated by semiconductor experiments and simulations.

---

## 1.8 Python as a Scientific Tool

It is useful to distinguish three related ideas.

### Python

Python is the programming language.

### Development environment

A development environment is where Python programs are written and executed.

Examples include:

- Jupyter Notebook;
- Google Colab;
- Visual Studio Code;
- other Python IDEs.

### Python libraries

A library is a collection of reusable programming tools.

Three libraries are particularly important in this course:

| Library | Primary purpose |
| --- | --- |
| NumPy | Numerical computing and array operations |
| Matplotlib | Data visualization |
| SciPy | Scientific and numerical analysis |

---

## 1.9 Jupyter Notebook and Google Colab

Scientific computing often requires more than code.

A useful scientific document may contain:

- explanations;
- mathematical equations;
- Python code;
- numerical output;
- graphs;
- interpretation.

A notebook environment allows these components to be combined in a single document.

### Jupyter Notebook

Jupyter Notebook provides an interactive environment in which code can be executed in cells.

A notebook can contain:

```text
Explanation
     ↓
Equation
     ↓
Python code
     ↓
Output
     ↓
Graph
     ↓
Interpretation
```

### Google Colab

Google Colab provides a notebook-based Python environment that runs through a web browser.

The basic workflow is:

> Write code → run code → inspect output → modify code → run again.

---

## 1.10 Our First Python Program

Let us begin with the simplest possible program.

```{code-cell} python
print("Hello, Python!")
```

The output is:

```text
Hello, Python!
```

The `print()` function displays information.

For example:

```{code-cell} python
print("Scientific Python")
print("Semiconductor Science and Technology")
```

Output:

```text
Scientific Python
Semiconductor Science and Technology
```

At this stage, the important idea is simple:

> Python executes the instructions we provide.

---

## 1.11 Python as a Calculator

Python can also perform arithmetic calculations.

```{code-cell} python
a = 10
b = 3

print(a + b)
print(a - b)
print(a * b)
print(a / b)
```

Output:

```text
13
7
30
3.3333333333333335
```

The basic arithmetic operators are:

| Operation | Python operator | Example |
| --- | --- | --- |
| Addition | `+` | `a + b` |
| Subtraction | `-` | `a - b` |
| Multiplication | `*` | `a * b` |
| Division | `/` | `a / b` |
| Power | `**` | `a ** b` |
| Remainder | `%` | `a % b` |

---

## 1.12 A First Scientific Calculation

For a resistive element,

$$
V = IR.
$$

Rearranging,

$$
I = \frac{V}{R}.
$$

Suppose:

$$
V = 5\,\mathrm{V}
\qquad\text{and}\qquad
R = 100\,\Omega.
$$

```{code-cell} python
V = 5
R = 100

I = V / R

print(I)
```

Output:

```text
0.05
```

Therefore,

$$
I = 0.05\,\mathrm{A}
  = 50\,\mathrm{mA}.
$$

The code and mathematical expression represent the same calculation:

$$
I=\frac{V}{R}
\quad\longleftrightarrow\quad
\texttt{I = V / R}.
$$

---

## 1.13 A Second Scientific Example: Electrical Power

Electrical power is given by

$$
P = VI.
$$

Suppose

$$
V=12\,\mathrm{V}
\qquad\text{and}\qquad
I=0.25\,\mathrm{A}.
$$

Then

$$
P=(12)(0.25)=3\,\mathrm{W}.
$$

```{code-cell} python
V = 12
I = 0.25

P = V * I

print(P)
```

Output:

```text
3.0
```

Thus,

$$
P=3.0\,\mathrm{W}.
$$

This correspondence between mathematical notation and Python syntax will be a recurring theme throughout the book.

---

## 1.14 Python and Physical Units

Python stores numerical values. It does not automatically understand their physical units.

For example:

```{code-cell} python
V = 5
```

Python knows that `V` contains the number \(5\).

It does not automatically know that:

$$
V=5\,\mathrm{V}.
$$

Consider:

```{code-cell} python
current = 20
```

Does `20` mean:

$$
20\,\mathrm{A},
$$

$$
20\,\mathrm{mA},
$$

or

$$
20\,\mu\mathrm{A}?
$$

The code alone does not tell us.

> **Important:** Numerical computation and physical interpretation are different tasks.

For scientific work, always keep track of:

- the quantity;
- its numerical value;
- its unit;
- the equation or model used.

---

## 1.15 Python Is a Tool — Not the Science

Suppose we want to calculate current using Ohm's law:

$$
I=\frac{V}{R}.
$$

The following Python statement is syntactically valid:

```python
I = R / V
```

However, it does not represent Ohm's law.

The computer can execute the statement correctly, but the scientific model is wrong.

> **A program can be syntactically correct and scientifically incorrect.**

The student must understand the physical system, mathematical model, units, assumptions, and expected behaviour.

---

## 1.16 From Individual Calculations to Data

Scientific experiments usually generate multiple observations.

Suppose an experiment records voltage and current:

$$
(V_1,I_1),\,
(V_2,I_2),\,
(V_3,I_3),\,
\ldots,\,
(V_n,I_n).
$$

For a small number of observations, values can be entered manually.

For larger datasets, we need efficient ways to:

- store data;
- process data;
- perform calculations;
- generate graphs;
- summarize observations.

This is where Python data structures and scientific libraries become important.

---

## 1.17 The Scientific Python Workflow

Throughout this course, we will repeatedly use the following workflow.

### Step 1 — Define the scientific problem

For example:

> How does current vary with applied voltage?

### Step 2 — Obtain the data

Data may come from an experiment, simulation, laboratory instrument, or supplied dataset.

### Step 3 — Store the data

Initially, we will use Python data structures. Later, we will use NumPy arrays.

### Step 4 — Compute

We may calculate derived quantities, averages, deviations, numerical functions, or fitted values.

### Step 5 — Visualize

Graphs can reveal patterns that are difficult to see in a table of numbers.

### Step 6 — Interpret

Ask:

> **What does the result mean scientifically?**

Scientific interpretation remains the responsibility of the scientist or engineer.

---

## 1.18 Python, Mathematics, and Scientific Interpretation

The three layers of scientific computing should remain connected.

### Mathematical model

$$
P=VI.
$$

### Python implementation

```{code-cell} python
V = 12
I = 0.25
P = V * I
print(P)
```

### Scientific interpretation

$$
P=3\,\mathrm{W}.
$$

The device consumes \(3\,\mathrm{W}\) of electrical power under the stated operating conditions.

The workflow is therefore:

$$
\boxed{
\text{Scientific concept}
\rightarrow
\text{Mathematical model}
\rightarrow
\text{Python implementation}
\rightarrow
\text{Numerical result}
\rightarrow
\text{Scientific interpretation}
}
$$

---

## 1.19 Common Beginner Mistakes

### Mistake 1 — Treating Python as a replacement for scientific knowledge

Python can perform a calculation, but it does not explain the physical meaning of the result.

### Mistake 2 — Ignoring units

A numerical result without its unit may be scientifically incomplete.

### Mistake 3 — Confusing syntax with scientific correctness

A Python program can run successfully and still implement the wrong equation.

### Mistake 4 — Learning commands without understanding the problem

The goal is not to memorize Python commands. The goal is to use Python to solve scientific and engineering problems.

### Mistake 5 — Treating graphs as conclusions

A graph shows a pattern. The scientist must explain the physical meaning of that pattern.

---

## 1.20 What You Will Learn in Unit I

The first unit follows a progression from basic programming to scientific computing.

```text
Python fundamentals
        ↓
Variables and data types
        ↓
Input and output
        ↓
Decision making
        ↓
Loops and functions
        ↓
Lists and dictionaries
        ↓
File handling
        ↓
NumPy arrays
        ↓
Mathematical operations
        ↓
Statistical operations
        ↓
Linear spacing
        ↓
Numerical calculations
        ↓
Matplotlib
        ↓
Scientific visualization
```

By the end of Unit I, you should be able to write small Python programs and use Python to work with basic scientific datasets.

---

## 1.21 Chapter Summary

In this chapter, we learned that:

1. **Programming** is the process of writing instructions for a computer.
2. **Python** is a high-level, general-purpose programming language.
3. Python is widely used in science and engineering because it combines readable syntax with a large scientific ecosystem.
4. Python can support work in electronics and semiconductor technology.
5. Jupyter Notebook and Google Colab provide convenient environments for scientific Python.
6. Python can perform mathematical and scientific calculations.
7. Scientific quantities must be interpreted together with their units and physical meaning.
8. A program can be syntactically correct but scientifically incorrect.
9. Scientific Python connects a scientific problem, mathematical model, code, numerical result, and interpretation.
10. Later chapters will extend these ideas to data structures, NumPy, and Matplotlib.

---

## 1.22 Exercises

### Exercise 1 — Ohm's Law

A resistor has resistance

$$
R=220\,\Omega
$$

and is connected to a

$$
V=5\,\mathrm{V}
$$

source.

Write a Python program to calculate the current.

### Exercise 2 — Electrical Power

A device operates at

$$
V=12\,\mathrm{V}
$$

and draws

$$
I=0.25\,\mathrm{A}.
$$

Write a Python program to calculate its power using

$$
P=VI.
$$

### Exercise 3 — Temperature Conversion

The relationship between Celsius and Kelvin temperature is

$$
T_K=T_C+273.15.
$$

Write a Python program to convert

$$
25\,^\circ\mathrm{C}
$$

to Kelvin.

### Exercise 4 — Multiple Calculations

A circuit contains a resistor of

$$
R=100\,\Omega.
$$

Calculate the current for each of the following voltages:

$$
V=1\,\mathrm{V},\quad
2\,\mathrm{V},\quad
3\,\mathrm{V},\quad
4\,\mathrm{V},\quad
5\,\mathrm{V}.
$$

At this stage, write separate Python calculations. In a later chapter, we will learn how to automate repeated calculations.

---

## 1.23 Think and Apply

A semiconductor experiment produces \(1{,}000\) voltage-current measurements.

A student says:

> "I can calculate everything using a calculator, so I do not need Python."

Do you agree?

Explain your answer by considering:

- repeated calculations;
- data storage;
- automation;
- visualization;
- reproducibility;
- scientific interpretation.

---

## Key Takeaway

> **Python is not being taught simply as a programming language. It is being introduced as a scientific tool for computation, data handling, visualization, and reproducible analysis.**

The next chapter introduces Python as a scientific calculator and develops the concepts of **variables, assignment, expressions, arithmetic operators, numerical calculations, scientific notation, and units**.
